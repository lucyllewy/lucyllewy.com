---
title: "Linq Style Field Selection"
date: 2021-07-20T17:12:39+01:00
draft: false
categories: [c#, programming]
---

I've been toying with rewriting my [Snapstats](https://snapstats.org) website to C# and .NET. The site tracks the public Snap Store to aggregate and make data about Snaps easily available.

As part of the rewrite I need to move the GraphQL API service over to C#. I found that using GraphQL.NET I can easily produce the API, and query my data in response to incoming requests. One feature that is common in .NET is the use of LINQ which allows to map classes to the database.

Because I'm looking to migrate the database into Azure CosmosDB the use of Entity Framework and LINQ is less helpful, although there is one feature that I really like that I discovered through GraphQL.Net: selecting fields of a class through the use of a lambda function. With GraphQL.Net this takes the form similar to:

```csharp
Field(t => t.name);
```

Here we see that you pass a lambda function `t => t.name` that accepts a single argument and returns a single response. The argument is an instance of the class that you want to select the field from, while the returned value is the value of the field you want to select. LINQ then takes the returned value and extracts the field name from it discarding the empty value of the field. (The field was empty because the instance was uninitialised.)

Now, as part of my writing the SQL queries that back my C#-based GraphQL API I desired the ability to use this pattern myself to make the queries more dynamic. Originally I managed to do this in a less desirable way, that required my lambdas to return a `nameof()` instead of the original field. This was due to the fact that once you've returned the field to the calling method it no longer has the same `nameof()` because it's now saved into a different variable name. This looked like below on the calling side:

```csharp
context.ToSqlOrderedQuerySpec<Snap>(o => typoeof(o.name))
```

And the `ToSqlOrderedQuerySpec()` method looked similar to:

```csharp
public static SqlQuerySpec ToSqlOrderedQuerySpec<TSource>(this IResolveFieldContext<object> context, Func<TSource, string?> keySelector, string order = "ASC")
{
    /* the rest of the SQL Query constructed here */

    var orderBy = GetFieldForKey(keySelector);

    if (!string.IsNullOrEmpty(orderBy))
    {
        queryTextStringBuilder.Append($" ORDER BY c.{orderBy} {order}");
    }

    return new SqlQuerySpec(queryTextStringBuilder.ToString(), parameters);
}
```

Finally, the `GetFieldForKey` helper looked like this:

```csharp
public static string? GetFieldForKey<TSource>(Func<TSource, string?> keySelector)
{
    TSource instance = new();
    return keySelector(instance);
}
```

This is not the best, so I went looking for alternatives.

After a lot of code trawling and Googling with BING I finally landed on a Stack Overflow answer (where else!?) that finally pointed me in the right direction to implement the pattern in my own code. The magic ingredient was to use the LINQ `Expression` type to wrap the `Func` declaration and pull the results of the lambda from the lambda cast to LINQ's `MemberExpression` type.

The method `ToSqlOrderedQuerySpec` needs it's signature updating to:

```csharp
using System.Linq;
public static SqlQuerySpec ToSqlOrderedQuerySpec<TSource>(this IResolveFieldContext<object> context, Expression<Func<TSource, object?>> keySelector, string order = "ASC")
{
    /* everything else in this method stays the same */
}
```

And the `GetFieldForKey` helper changes to suit:

```csharp
using System.Linq;
public static string? GetFieldForKey<TSource>(Expression<Func<TSource, object?>> keySelector)
{
    PropertyInfo? key;
    if (keySelector.Body is MemberExpression expr)
    {
        key = expr.Member as PropertyInfo;
    }
    else
    {
        var op = ((UnaryExpression)keySelector.Body).Operand;
        key = ((MemberExpression)op).Member as PropertyInfo;
    }

    return key.Name();
}
```

The reason for the `if` construct is to account for type boxing that causes lambdas selecting `DateTime` typed fields, for example, to be a `UnaryExpression` instead of a `MemberExpression`. The difference seems to be due to complex types vs simple ones such as `int`, `string`, and `bool`, though I'm not an expert so I can't explain why that makes the labmda change `Expression` subtype.

Finally, to call it we change the lambda to return the field instead of the name of the field:

```csharp
context.ToSqlOrderedQuerySpec<Snap>(o => o.name)
```

As you can see above, I have achieved the target of LINQ-style field selection for my own SQL generation routines. This allows me complete control of SQL generation without deferring to LINQ-to-SQL, which can sometimes create suboptimal queries.