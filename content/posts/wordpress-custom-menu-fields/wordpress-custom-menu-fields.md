---
title: "WordPress custom menu fields"
date: 2014-06-13 17:39:48
published: true
categories: [development, WordPress]
tags: [WordPress]
wpid: 881
---

I've packaged-up a plugin that I wrote a while ago for a project that I worked on which required a WordPress site's menu items to have a separately addressable excerpt field which could override the linked page's in-built excerpt.

The plugin that I've now released was half of the resolution to this requirement, which allows for simple programmatic field additions to the menu system.

[Custom Menu Fields is available at the WordPress.org plugin repo.](https://WordPress.org/plugins/custom-menu-fields/)

To use this plugin all one needs to do is install and activate the plugin as normal and then include a snippet similar to below in their theme's functions.php (or custom plugin code. Just be sure it loads earlier than the 'init' action!):

```php
<?php
add_action('init', 'menu_excerpt__add_menu_field');
function menu_excerpt__add_menu_field() {
     if (!is_callable('bh_add_custom_menu_fields'))
         return;
     bh_add_custom_menu_fields(array(
         'excerpt' => array(
            'description' => 'Excerpt',
            'type' => 'textarea',
            )));
}
?>
```

Now you can get access to the 'excerpt' field by using the attribute on the menu variable you assign when you get the menu from WordPress.

e.g. to print the excerpt created above:

```php
$menu = wp_nav_menu('theme_location' => 'your-theme-location-name', 'echo' => false);
print $menu->excerpt;
```