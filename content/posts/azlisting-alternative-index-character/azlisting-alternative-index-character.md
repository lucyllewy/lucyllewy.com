---
title: "How to make the A-Z Listing plugin for WordPress use a different Index Letter for a post"
date: 2017-08-17 02:26:50
published: true
categories: [development, WordPress]
tags: [A-Z-Listing, WordPress]
wpid: 1735
---

This post talks about our [A-Z Listing Plugin](https://a-z-listing.com/), which is [available on WordPress.org](https://WordPress.org/plugins/a-z-listing/).

How did we get here?
--------------------

After [a long battle with a very considerate person in the WordPress.org forums](https://WordPress.org/support/topic/a_z-by-surname-second-word-of-post-title/) where I repeatedly failed to get them working with various attempts at writing code they could drop into their site, I decided I needed to write it up and explain how to do this once and for all.

### The scenario

- You have a series of posts with Proper Nouns (people's names) as their title
- You want to index them on their Family Name (sometimes called the "Last Name")

The concept is fairly simple, in that you need to hook into the filter that I've provided and return the right result with the index letter changed. This is the rub in my attempt at helping the person on WordPress.org in that I really didn't understand my own code well enough, despite having written it, so I kept suggesting non-working solutions.

The final code
--------------

```php
add_filter( 'a_z_listing_item_indices', 'my_a_z_index_filter', 10, 3 );

function my_a_z_index_filter( $indices, $item, $item_type ) {
    // make sure we're filtering the right post type
    if ( 'post-type-we-want' === get_post_type( $item ) ) {
        // pull the title and get the first letter of the second word
        $full_name = explode( ' ', $item->post_title );

        // the last word is in the last element of $title_parts so check it is there
        $last_name = array_pop( $full_name );

        // ensure we actually found a last name
        if ( $last_name )  {
            // cut the first letter out for our index
            $index = substr( $last_name, 0, 1 );

            // set up a new empty array
            $indices = array();

            // only the first names are left in $full_name. Join them together again
            $first_names = join( ' ', $full_name );

            // Now put the last name first and separate with a comma+space from the first names
            $formatted_name = "{$last_name}, {$first_names}";

            // add the new name associated with the item into the new array we created
            $indices[ $index ][] = array(
                'title' => $formatted_name,
                'item' => $item,
            );

            // return a new array with our new index letter instead of the
            // indices already discovered by the plugin
            return $indices;
        }
    }

    // if we get here we didn't override the indices so return
    // those already discovered.
    return $indices;
}
```

The new way (Jan 2019)
----------------------

Since this post was published I have added a new filter that is much more friendly to use. The new filter does not require a specific format for the return value. Instead, it just needs an array of letters for the post to be indexed against.

```php
add_filter( 'a_z_listing_item_index_letter', 'my_a_z_index_filter', 10, 3 );

function my_a_z_index_filter( $index_letters, $item, $item_type ) {
    // make sure we're filtering the right post type
    if ( 'post-type-we-want' === get_post_type( $item ) ) {
        // pull the title and get the first letter of the second word
        $full_name = explode( ' ', $item->post_title );

        // the last word is in the last element of $title_parts so check it is there
        $last_name = array_pop( $full_name );

        // ensure we actually found a last name
        if ( $last_name )  {
            // cut the first letter out for our index
            $index = substr( $last_name, 0, 1 );

            // set up a new empty array overwriting the old indices
            $index_letters = array( $index );
        }
    }

    // return the indices item's indices
    return $index_letters;
}
```