### Introduction:
Roblox has started to use WebP instead of PNG, but they have chosen to use a very lossy WebP, and not a lossless one, which is a shame. PNG is still being stored, so I made a plugin to fix it.

### Inject code using one of these browser extensions:
- Gecko based: <https://addons.mozilla.org/en-GB/firefox/addon/abc-js-css-injector/>
- Chromium based: <https://chrome.google.com/webstore/detail/nbhcbdghjpllgmfilhnhkllmkecfmpld>
- Either/Other: <https://violentmonkey.github.io/>

### Code:
```js
// ==UserScript==
// @name        Roblox WebP To PNG
// @namespace   Roblox WebP To PNG
// @match       https://www.roblox.com/*
// @grant       none
// @version     1.3
// @author      @Knewest on Github
// @source https://github.com/Knewest/Roblox-WebP-To-PNG
// ==/UserScript==

// This code changes all the lossy versions of the images on Roblox to be lossless.

// Function to replace image source:
function replaceImageSource(elements, partialOriginalSrc, newSrc, index = 0) {
    if (index < elements.length) {
        var element = elements[index];
        var image = element.querySelector('img');
        if(image) {
            var currentSrc = image.getAttribute('src');
            if(currentSrc.includes(partialOriginalSrc)) {
                var newSrcWithReplacement = currentSrc.replace(partialOriginalSrc, newSrc);
                image.setAttribute('src', newSrcWithReplacement);
            }
        }
        // Call the function recursively for the next element after a delay:
        setTimeout(function() {
            replaceImageSource(elements, partialOriginalSrc, newSrc, index + 1);
        }, 25);
    }
}

// MutationObserver() function to watch for changes in the DOM:
var observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {
        if (mutation.type === 'childList') {
            replaceImageSources();
        }
    });
});
var observerConfig = { childList: true, subtree: true };
observer.observe(document, observerConfig);

// Function to replace image sources for all specified elements:
function replaceImageSources() {
    var ProfileBodyShot = document.querySelectorAll('.thumbnail-2d-container.thumbnail-span');
    var ProfileHeadShot = document.querySelectorAll('.thumbnail-2d-container.avatar-card-image.profile-avatar-thumb');
    var HomeFriendList = document.querySelectorAll('.thumbnail-2d-container');
    var GameIcons1 = document.querySelectorAll('.thumbnail-2d-container.game-card-thumb-container');
    var GameIcons2 = document.querySelectorAll('.thumbnail-2d-container.brief-game-icon');
    var GameIconsDiscover = document.querySelectorAll('.thumbnail-2d-container.game-card-thumb');
    var MarketplaceIcons = document.querySelectorAll('.ng-isolate-scope');
    var ItemPage= document.querySelectorAll('.thumbnail-2d-container');

    // Replace image sources for the first set of elements with a delay between each change:
    replaceImageSource(ProfileBodyShot, 'Webp/noFilter', 'PNG/noFilter');
    replaceImageSource(ProfileHeadShot, 'Webp/noFilter', 'PNG/noFilter');
    replaceImageSource(HomeFriendList, 'Webp/noFilter', 'PNG/noFilter');
    replaceImageSource(GameIcons1, 'Webp', 'PNG');
    replaceImageSource(GameIcons2, 'Webp/noFilter', 'PNG/noFilter');
    replaceImageSource(GameIconsDiscover, 'Webp', 'PNG');
    replaceImageSource(MarketplaceIcons, 'Webp', 'PNG');
    replaceImageSource(ItemPage, 'Webp', 'PNG');
}

// Call the function once initially to replace image sources on page load:
replaceImageSources();

// v1.3
// https://github.com/Knewest
// Copyright (Boost Software License 1.0) 2024-2024 Knew
```

### Comparison between lossy WebP and PNG (WebP/PNG):
![PNG](https://github.com/Knewest/Roblox-WebP-To-PNG/assets/94736474/1b43a4de-9df1-4856-847d-fbae77eea88d)
