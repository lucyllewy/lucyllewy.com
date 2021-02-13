---
title: "Lazy Loading images in WordPress"
excerpt: "Browser-native image lazy loading in WordPress for faster load times, with JavaScript fallback using LazySizes.js to support older browsers."
date: 2020-06-16 20:00:00
published: true
categories: [development, WordPress]
tags: [lazy-loading, WordPress]
cover_image: /wp-content/uploads/2020/06/close-up-photo-of-woman-using-her-mobile-phone-4100843-1024x683.jpg
featuredImage: /wp-content/uploads/2020/06/close-up-photo-of-woman-using-her-mobile-phone-4100843-1024x683.jpg
wpid: 10758
---

While WordPress Core is working towards their own Native Lazy Loading of images, I have been using something for a while already to do the job. I'm sure I found this from, or was inspired by, someone else's code but I don't recall where. If it is your code, please leave a comment so that I may correctly attribute it.

Step one
--------

The first step is to hook into the `wp_get_attachment_image_attributes` filter to override the in-built `src`, `srcset`, and `sizes` attributes set by Core. We rename these attributes to `data-$attribute_name`. The new image data doesn't do anything by itself. We must move it to the correct place using a small shim in the second code snippet below: (Both snippets may be placed into your theme's `functions.php` file)

```php
<?php
add_filter( 'wp_get_attachment_image_attributes', 'lazyload_images', 99 );
function lazyload_images( $attr ) {
    $return = [];
    foreach ( $attr as $key => $value ) {
        switch( $key ) {
        case 'sizes':
        case 'src':
        case 'srcset':
            $key = "data-$key";
            break;
        }
        $return[ $key ] = $value;
    }

    $return['class'] .= ' lazyload';
    $return['loading'] = 'lazy';

    return $return;
}
```

Step two
--------

For the second, and final, step, we print a small bit of JavaScript into the footer of our site to load a fallback script. Alternatively, we rewrite the `data-$attribute_name` attributes back to their original names for native lazy loading. This code detects at load time whether the browser supports native lazy loading, or we need to use something else to mimic it. In this case the fallback script we use is [LazySizes.js](https://github.com/aFarkas/lazysizes):

```php
<?php
add_action( 'wp_footer', 'native_lazyload_handler' );
function native_lazyload_handler() {
    ?>
    <script>
    if ('loading' in HTMLImageElement.prototype) {
        const images = document.querySelectorAll('img.lazyload');
        images.forEach(img => {
            img.sizes = img.dataset.sizes;
            img.src = img.dataset.src;
            img.srcset = img.dataset.srcset;
        });
    } else {
        // Dynamically import the LazySizes library
        const script = document.createElement('script');
        script.src = 'https://cdnjs.cloudflare.com/ajax/libs/lazysizes/5.2.2/lazysizes.min.js';
        script.async = true;
        document.body.appendChild(script);
    }
    </script>
    <?php
}
```

Last thoughts
-------------

You might recall, we also add the class name of `lazyload` in the first snippet. So, if the browser doesn't support native lazy loading then the `LazySizes.js` fallback loads. This uses the `data-$attribute_name` attributes unchanged. Those we also set in the first snippet.

If, however, the browser *does* support native lazy loading then we simply rewrite the `data-$attribute_name` back to their original names. Then we let the browser do its thing. In this case, we don't load any extra JavaScript.

For even faster load times we set the LazySizes script to the `async` load method. With this, the browser loads it asynchronously without blocking the page render. This means that the Time Till First Meaningful Paint is quicker because the browser doesn't wait for the script to load. Once LazySizes loads, or the attributes rewritten for Native Lazy Loading, then the images pop into place as soon as they are visible. Both browser native lazy loading and LazySizes 5.0+ support loading without the page jumping about as the images are loaded. To do this, they use the `width` and `height` attributes. These attributes indicate the correct aspect ratio of the image, which the browser and LazySizes use to cut out the right amount of space for the image.

- - - - - -

Photo by **[Matilda Wormwood](https://www.pexels.com/@matilda-wormwood?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)** from **[Pexels](https://www.pexels.com/photo/close-up-photo-of-woman-using-her-mobile-phone-4100843/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)**