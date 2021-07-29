---
title: "Trialling Drupal"
date: 2010-07-17 21:33:21
published: true
categories: [random]
wpid: 716
---

Dani has been utilising the Drupal Content Management System for few of his other projects taken on board as part of commitments to the ~~IND-Web.com WebHost~~ and [Bowl Hat Web Developer](https://bowlhat.net/). These projects are due to go live soon, ~~but Dani doesn't wish to publicise them just yet~~. (Dani has just noticed that they have already publicised both sites, [R. Llewellyn Electrical Contractor](https://rllewellyn.co.uk/) and ~~Treasure Trove Designs~~, previously in his About Dani page on this very site.) Both of these sites are already discoverable via Google, but will inform visitors about the incomplete status.

As part of this work, Dani found that they really quite liked the Drupal CMS for it's sheer scope of ability. They have now used the CMS on three separate sites including this blog. Previous work had utilised the WordPress system due to it's low barrier-to-entry. However, Dani found that at times WordPress can be very limiting, and especially so for non-blog-oriented applications. Where WordPress is primarily a blog engine with CMS on-the-side; Drupal is first and foremost a CMS which means that, while blogging can be an integral part of any site, a site needn't have the blog parts dragging the rest down.

Also, Dani finds the theming system a bit less clumsy, and a lot less undocumented. WordPress tends to feel difficult to get along with due to the excess number of black-box parts of everything that a programmer is meant to play with, such as themes and modules. Take themes for a simple example: it is clearly documented that a programmer should use a content "loop" but the workings of this loop requires many bad programming practices, such as global variables, and it never is clear why one needs to call `the_loop()` function straight after starting a programmatic loop. Wouldn't it be much simpler if instead of saying

```php
while(have_posts()) { the_loop();
```

to set up a loop which calls the "WordPress loop" (these two names conflict in their meanings within WordPress where a loop is required to interact with the "WordPress loop".) you utilise object orientation and say something similar to:

```php
<?php while ($post = $wp->getPost()) { ?>
    <div class="post">
        <h1><?php print $post->title; ?></h1>
        <small>By <?php print $post->author; ?> on <?php print $post->formattedDate; ?></small>
        <?php print $post->content; ?>
    </div>
<?php } ?>
```

This would be the complete loop. Note the use of a single global variable, `$wp`, and absolutely no use of global functions, `have_posts()` and `the_loop()`, which trample all over pre-existent variables thereby preventing any thought of nested loops.

With Dani's way WordPress theme designers would be able to use constructs similar to the following, which are completely impossible without major hair pulling in WordPress as it stands today:

```php
<?php while ($outerpost = $wp->getPost()) { ?>
    <div class="post">
        <h1><?php print $outerpost->title; ?></h1>
        <div class="postcontent"><?php print $outerpost->content; ?></div>
<?php
    $query = $wp->query("query string for a related set of posts");
    if ($query->postCount() > 0) {
?>
        <ul class="related">
<?php while ($innerpost = $query->getPost()) { ?>
            <li><a href="<?php $innerpost->permalink; ?>">
                <?php print $innerpost->title; ?>
            </a></li>
<?php } ?>
        </ul>
    </div>
<?php } ?>
```

Dani has not looked at the capability of Drupal in regards to nested content, but another thing that isn't possible in WordPress easily, yet is fairly simple in Drupal is theming the landing page differently to the rest of the site. Ok, so Drupal actually allows theming of any random page you desire to invent, which also includes the landing page. The was this is done is to create a separate page-front.tpl.php file in your theme's folder with the design for your landing page. This is used on both of the aforementioned sites that Dani has designed and provided hosting for.

Looking forward to the future, Dani feels that Drupal, with it's extreme versatility, and even it's capability to hide features from the control panel for the designated administrator, should be the way forward for his projects from now-on. (The locking facility will be used on both aforementioned sites, due to ongoing business needs of the owner not requiring the full feature-set of Drupal so they needn't be aware such functionality exists unless they require it.)