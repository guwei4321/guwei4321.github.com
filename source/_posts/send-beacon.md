title: send-beacon
date: 2018-06-14 16:06:45
tags:
- JavaScript
categories:
- JavaScript
---


````javascript
function imgPing(url, callback) {
    var key = '__SOME_RANDOM_KEY__' + (+new Date());
    var img = new Image();
    window[key] = img;
    img.onload = img.onerror = img.onabort = function () {
        img.onload = img.onerror = img.onabort = null;
        window[key] = null;
        img = null;
        if (callback) {
            callback();
        }
    };
    img.src = concatUrl;
    return true;
}

````