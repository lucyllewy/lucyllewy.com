---
title: "That thar welcome page"
date: 2008-11-06 10:00:31
published: true
categories: [random]
wpid: 231
---

As a follow up to the bendy corners post the other day, I thought I should explain how I got my theme to display my welcome page on the main blog listing. Well, it's actually quite simple, and requires the index.php of your theme to be modified with the following:

```php
<?php $welcome = get_page_by_title('Welcome'); ?>
<?php if ($welcome && $welcome != '') { ?>
    <div id="welcome">
        <h2><?php print($welcome->post_title); ?></h2>
        <?php print(wpautop($welcome->post_content)); ?>
    </div>
<?php } ?>
```

What this does, is selects the "page" in WordPress' database with the title "Welcome" (including the upper case W) and outputs it inside a div with a known id so that we can style it, using css, and play with it's properties via javascript.

The advantage that this method has over the alternative of hard coding the welcome page into the theme file, is that you can very easily update the content of your welcome page at any time using the standard WordPress authoring techniques.