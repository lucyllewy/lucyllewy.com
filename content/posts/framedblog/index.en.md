---
title: "FramedBlog"
date: 2015-10-27 21:09:23
published: true
categories: [random, WordPress]
wpid: 176
---

**No longer actively developed. see the AJAXBlog page instead**

ok, here goes the instructions of how to have bookmark capability with a framed WordPress page:

1. move your WordPress installation into a folder (from here on out called `WordPress/`) and update the settings of WordPress to use the new location (If you can't get into the wp-admin pages after the move, edit the `wp_options` table in mysql and change any instances of the old url to the new one. In my install that was only the very first option in that table.).
2. create in the directory above WordPress/ a file called index.html which will be your frameset.
3. Once you've finished adding the desired frame layout, add the code from below inside the `<head></head>` section.
4. download and install [this plugin (framedblog.zip)](/downloads/framedblog.zip), a simple affair which will import the `prototype.js` library included with WordPress and send click event notifications to the "parent" frame, ie your frameset.
5. navigate to your frame page and bask in the glory of bookmarkability!!

```html
<script src="wp-includes/js/prototype.js" type="text/javascript"></script>
<script type="text/javascript">
//<![CDATA[
var wpdir = 'WordPress/'
var baseurl = 'https://bowlhat.net/';
Event.observe(window, 'load', function() {
    currentdoc = window.location.href;
    idx = currentdoc.indexOf('#');
    var loaddoc = wpdir;
    if ((idx > -1) &amp;&amp; (idx != currentdoc.length)) {
        loaddoc += currentdoc.substring(idx+1);
        setTimeout(checkAddressBar, 1000);
    }
    mainframe.location.href = loaddoc;
});
function checkAddressBar() {
    idx = window.location.href.indexOf('#');
    loaddoc = false;
    if ((idx > -1) &amp;&amp; (idx != window.location.href.length)) {
        loaddoc = window.location.href.substring(idx+1);
    }
    var mainframe = document.getElementById('mainframe');
    if ((loaddoc == '/') &amp;&amp; ((mainframe.src == baseurl+wpdir) || mainframe..src == baseurl+wpdir+'/')) {
        setTimeout(checkAddressBar, 1000);
        return;
    }
    if (loaddoc &amp;&amp; baseurl+wpdir+loaddoc != mainframe.src) {
        mainframe.src = baseurl+wpdir+loaddoc;
    }
    setTimeout(checkAddressBar, 1000);
}
function setlocation(location) {
    location = new String(location);
    if (!location.match(baseurl)) {
        if (location.match(/^((((ht)|f)tp[s]?)://)|(mailto:)/)) {
            window.location.href = location;
            return;
        }
    }
    currentdoc = window.location.href;
    idx = currentdoc.indexOf("#");
    if (idx > -1) {
        myself = currentdoc.substring(0,(idx));
    } else {
        myself = currentdoc;
    }
    location = location.substring(myself.length + wpdir.length);
    if (!location) location = '/';
    window.location.href = myself + '#' + location;
}
//]]>
</script>
```