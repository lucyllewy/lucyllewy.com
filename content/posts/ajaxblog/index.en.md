---
title: "AJAXBlog"
date: 2008-06-12 23:36:05
published: true
categories: [development, WordPress]
wpid: 196
---

> This is very old and very bad. Do not use it 😁👍🏻

Following up from my FramedBlog, I've now replaced my frames with an AJAX load system. This allows the sidebar and any header and footer to remain constant. This was initially due to my desire to have a music player written in flash to remain loaded and playing throughout pageloads. At first, I thought it would be better to use Frames, but have since been playing with asynchronous loading. The AJAX method allows for pages to load without first showing an empty screen/frame, and as previously stated for my flash music player to remain loaded.

AJAXBlog is built upon the techniques I learned for use with the FramedBlog, and owes it's existence to the previous project.

Here are the instructions:

1. edit your theme's functions.php and add a line:   
    
  `wp_enqueue_script('ajaxblog','-content/themes/yourtheme/js/ajaxblog.js',array('scriptaculous'));`
2. download ajaxblog.js at the end of this article
3. edit ajaxblog.js and set the `var AJAXBlogBaseURL =` line to the top level web address of your WordPress directory _without_ the trailing `/`. e.g. `https://bowlhat.net/` (where my WordPress install is accessed by the top-level domain only) or ...net/WordPress (where WordPress is the dir containing my wp install)
4. upload ajaxblog.js to your webspace in the `wp-content/themes/yourtheme/js` directory (or another of your choosing, remembering to update the `wp_enqueue_script` call in `functions.php`).

```javascript
/**
 * AJAXBlog - intercepts anchor clicks and requests the document asynchronously
 * Copyright (C) 2010  Daniel Llewellyn aka Fremen the Honeymonster
 * ( daniel@bowlhat.net - https://bowlhat.net/ )
 *
 *  This program is free software: you can redistribute it and/or modify
 *  it under the terms of the GNU Affero General Public License as
 *  published by the Free Software Foundation, either version 3 of the
 *  License, or (at your option) any later version.
 * 
 *  This program is distributed in the hope that it will be useful,
 *  but WITHOUT ANY WARRANTY; without even the implied warranty of
 *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *  GNU Affero General Public License for more details.
 *
 *  You should have received a copy of the GNU Affero General Public License
 *  along with this program.  If not, see .
 */

var AJAXBlog = {Opts: {}};
// this domain matching requires that you manually enter any subdirectories yourself in the BaseUrl, AdminUrl and LoginUrl vars.
window.location.href.match(new RegExp('^(http[s]?:\/\/\w+(\.\w+))*\/'));
AJAXBlog.Opts.BaseUrl = new String(RegExp.$1);
AJAXBlog.Opts.AdminUrl = new String(AJAXBlog.Opts.BaseUrl+'-admin');
AJAXBlog.Opts.LoginUrl = new String(AJAXBlog.Opts.BaseUrl+'-login.php');
/* don't fiddle with anything below unless ya know what ya doing */

AJAXBlog.RegEx = {
    AdminUrl: new RegExp(`^${AJAXBlog.Opts.AdminUrl}`),
    LoginUrl: new RegExp(`^${AJAXBlog.Opts.LoginUrl}`),
    AnchorPath: new RegExp(`(#(.*))$`),
    QueryString: new RegExp(`(\?.*)(#(.*(#\w*)?))?$`)
}

var AJAXBlogDocDomain = ``;
var AJAXBlogCurrentDocument = window.location.href;
var AJAXBlogEventTracker = ``;
var AJAXBlogTimeTracker = ``;
var AJAXBlogRequest = null;
var AJAXBlogRequestedURL = ``;
var AJAXBlogAdsenseHidden = 0;
var AJAXBlogTimeout = null;

//AJAXBlog_clsTimeTracker - see http://code.google.com/apis/analytics/docs/eventTrackerWrappers.html
// this AJAXBlog_clsTimeTracker code is copyright 2009 Google Inc.
// it is released under the terms of the APACHE v2.0 License.
var AJAXBlog_clsTimeTracker = function(opt_bucket) { if (opt_bucket) { this.bucket_ = opt_bucket.sort(this.sortNumber); } else { this.bucket_ = AJAXBlog_clsTimeTracker.DEFAULT_BUCKET; } }
AJAXBlog_clsTimeTracker.prototype.startTime_;
AJAXBlog_clsTimeTracker.prototype.stopTime_;
AJAXBlog_clsTimeTracker.prototype.bucket_;
AJAXBlog_clsTimeTracker.DEFAULT_BUCKET = [100, 250, 500, 1500, 2500, 5000];
AJAXBlog_clsTimeTracker.prototype._getTimeDiff = function() { return (this.stopTime_ - this.startTime_); };
AJAXBlog_clsTimeTracker.prototype.sortNumber = function(a, b) { return (a - b); }
AJAXBlog_clsTimeTracker.prototype._recordStartTime = function(opt_time) { if (opt_time != undefined) { this.startTime_ = opt_time; } else { this.startTime_ = (new Date()).getTime(); } };
AJAXBlog_clsTimeTracker.prototype._recordEndTime = function(opt_time) { if (opt_time != undefined) { this.stopTime_ = opt_time; } else { this.stopTime_ = (new Date()).getTime(); } };
AJAXBlog_clsTimeTracker.prototype._setHistogramBuckets = function(buckets_array) { this.bucket_ = buckets_array.sort(this.sortNumber); };
AJAXBlog_clsTimeTracker.prototype._track = function(opt_event_label) {
    var i;
    var bucketString;
    for(i = 0; i < this.bucket_.length; i++) {
        if ((this._getTimeDiff()) < this.bucket_[i]) {
            if (i == 0) {
                bucketString = `0-${this.bucket_[0]}`;
                break;
            } else {
                bucketString = `${this.bucket_[i - 1]}-${this.bucket_[i] - 1}`;
                break;
            }
        }
    }
    if (!bucketString) {
        bucketString = `${this.bucket_[i - 1]}+`;
    }
    window.pageTracker._trackEvent(bucketString, opt_event_label, this._getTimeDiff());
};
// END AJAXBlog_clsTimeTracker

jQuery(document).ready(function() {
    if (window.pageTracker) {
        AJAXBlogEventTracker = window.pageTracker._createEventTracker(`AJAXBlog`);
        AJAXBlogTimeTracker = new AJAXBlog_clsTimeTracker();
    }
    indexdoc = new String(window.location.href);

    anchorval = basedoc = querystring = ``;
    if (indexdoc.match(AJAXBlog.RegEx.AnchorPath)) {
        anchorval = RegExp.$2;
        basedoc = indexdoc.replace(AJAXBlog.RegEx.AnchorPath, ``);
    } else basedoc = indexdoc;
    if (basedoc.match(AJAXBlog.RegEx.QueryString)) {
        querystring = RegExp.$1;
        basedoc = basedoc.replace(AJAXBlog.RegEx.QueryString, ``);
    }

    if (querystring.match(new RegExp(`[\?&amp;]s=`))) {
        AJAXBlogSetLinkHandlers();
        AJAXBlogSetTimeout(50);
        AJAXBlogLoading(`hide`);
        return;
    }

    if (basedoc == AJAXBlog.Opts.BaseUrl || basedoc == `${AJAXBlog.Opts.BaseUrl}/`) {
        basedoc.match(/^http[s]?:\/\/(.+?)(\/.*)?$/);
        AJAXBlogDocDomain = new RegExp(`^http[s]?:\/\/${RegExp.$1}`);

        if (anchorval) {
            window.location.href= `#${anchorval}`;
        }

        AJAXBlogSetLinkHandlers();
        AJAXBlogSetTimeout(50);
        AJAXBlogLoading(`hide`);
        return;
    }

    if (!basedoc.match(AJAXBlog.RegEx.AdminUrl) &amp;&amp; !basedoc.match(AJAXBlog.RegEx.LoginUrl)) {
        myurl = querystring = newhome = ``;
        if (basedoc.match(AJAXBlog.RegEx.QueryString)) {
            querystring = RegExp.$1;
            if (RegExp.$3) myurl = RegExp.$3;
            else myurl = myurl.replace(AJAXBlog.RegEx.QueryStringOnly, ``);
        } else if (basedoc.match(AJAXBlog.RegEx.AnchorPath)) {
            myurl = RegExp.$2;
        } else {
            myurl = basedoc.replace(new RegExp(AJAXBlog.Opts.BaseUrl), ``);
            newhome = AJAXBlog.Opts.BaseUrl;
        }
        if (myurl) {
            window.location.href = `${newhome}${querystring}#${myurl}`;
            AJAXBlogSetTimeout(50);
            return;
        }
    }

    AJAXBlogSetLinkHandlers();
    AJAXBlogSetTimeout(50);
    AJAXBlogLoading(`hide`);
});

function AJAXBlogLoading(showorhide) {
    try {
        if (showorhide == `show` || showorhide == 1) jQuery(`#loadingscreen`).style.display = `block`;
        else jQuery(`#loadingscreen`).style.display = `none`;
    } catch (e) {}
}

function AJAXBlogSetTimeout(delay) {
    AJAXBlogTimeout = setTimeout(AJAXBlogCheckAddressBar, ((delay) ? delay : 500));
}

function AJAXBlogUpdateAddress(newlocation) {
    newlocation = new String(newlocation);

    if (newlocation.match(AJAXBlogDocDomain) &amp;&amp; !newlocation.match(AJAXBlog.RegEx.AdminUrl) &amp;&amp; !newlocation.match(AJAXBlog.RegEx.LoginUrl)) {
        newlocation = newlocation.substr(AJAXBlog.Opts.BaseUrl.length);
        baseurl = `#`;
        if (newlocation.match(AJAXBlog.RegEx.QueryString)) {
            baseurl = RegExp.$1 + baseurl;
            newlocation = newlocation.replace(AJAXBlog.RegEx.QueryStringOnly, ``);
        }
        window.location.href = baseurl + newlocation;
        return 1;
    }
    return 0;
}

function AJAXBlogCheckAddressBar() {
    anchorval = basedoc = ``;
    loaddoc = new String(window.location.href);
    if (loaddoc.match(AJAXBlog.RegEx.AnchorPath)) {
        basedoc = RegExp.$1;
        anchorval = RegExp.$2;
    }
    loaddoc = anchorval;
    if (loaddoc != `` &amp;&amp; AJAXBlog.Opts.BaseUrl+loaddoc != AJAXBlogCurrentDocument) {
        AJAXBlogNav(AJAXBlog.Opts.BaseUrl+loaddoc);
    }
    AJAXBlogSetTimeout();
}

function AJAXBlogUnsetLinkHandlers() {
    jQuery(`a`).each(function(anchor) {
        jQuery(anchor).unbind(`click`, AJAXBlogClickHandler);
    });
}

function AJAXBlogSetLinkHandlers() {
    jQuery(`a`).not('.thickbox,.noajax,[rel^=lightbox]').each(function(anchor) {
        jQuery(anchor).bind(`click`, AJAXBlogClickHandler);
    });
}

function AJAXBlogClickHandler(event) {
    myurl = event.target.href;
    if (AJAXBlogRequest) {
        res = confirm(`Are you sure you want to navigate away from this page? The browser is currently awaiting data from the server. You can either wait it out, or continue this request.`);
        if (!res) {
            event.preventDefault();
            event.stopPropagation();
            return;
        }
        if (AJAXBlogEventTracker) {
            AJAXBlogUnsetLinkHandlers();
            AJAXBlogTimeTracker._recordEndTime();
            AJAXBlogTimeTracker._track(AJAXBlogEventTracker, `AJAXBlog`, `Navigate Away while awaiting ${AJAXBlogRequestedURL}`);
        }
    }
    if (AJAXBlogUpdateAddress(myurl)) {
        event.preventDefault();
        event.stopPropagation();
    }
}

function AJAXBlogNav(url) {
    if (AJAXBlogCurrentDocument == url) return;
        if (AJAXBlogEventTracker) { AJAXBlogTimeTracker._recordStartTime(); }
        AJAXBlogCurrentDocument = url;
        AJAXBlogLoading(`show`);
        AJAXBlogUnsetLinkHandlers();
        AJAXBlogRequest = jQuery(`#outercontent`).load(`${url}  #content`, function(responseText, textStatus, XMLHttpRequest) {
        AJAXBlogRequest = null;
        AJAXBlogSetLinkHandlers();
        if (typeof(window[`tb_init`]) == `function`) {
            tb_init(`a.thickbox, area.thickbox, input.thickbox`);
        }

        if (textStatus == `error`) {
            jQuery(`#content`).empty().append(`<h2>Error</h2><p>There was an error getting the content you requested. Go <a href="javascript:history.go(-1)">back</a> and try again.</p>`);
        }

        if (AJAXBlogEventTracker) {
            AJAXBlogTimeTracker._recordEndTime();
            AJAXBlogTimeTracker._track(url);
        }

        AJAXBlogLoading(`hide`);
    });
}

////////END OF AJAXBLOG
```