### Introduction:
Roblox has started to use WebP instead of PNG, but they have chosen to use a very lossy WebP, and not a lossless one, which is a shame. PNG is still being stored, so I made a plugin to fix it.

### Inject code using one of these browser extensions:
- Gecko based: <https://addons.mozilla.org/en-GB/firefox/addon/abc-js-css-injector/>
- Chromium based: <https://chrome.google.com/webstore/detail/nbhcbdghjpllgmfilhnhkllmkecfmpld>

### Code:
```js
// This code changes all the lossy versions of the images on Roblox to be lossless.

// Function to replace image source:
function replaceImageSource(element, partialOriginalSrc, newSrc) {
    var image = element.querySelector('img');
    if(image) {
        var currentSrc = image.getAttribute('src');
        if(currentSrc.includes(partialOriginalSrc)) {
            var newSrcWithReplacement = currentSrc.replace(partialOriginalSrc, newSrc);
            image.setAttribute('src', newSrcWithReplacement);
        }
    }
}

// Watch for changes in the DOM:
var observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {
        if (mutation.type === 'childList') {
            mutation.addedNodes.forEach(function(node) {
                if (node.nodeType === Node.ELEMENT_NODE) {
                    replaceImageSources(node);
                }
            });
        }
    });
});
var observerConfig = { childList: true, subtree: true };
observer.observe(document, observerConfig);

// Queue to store images waiting for replacement:
var replacementQueue = [];
var isProcessingQueue = false;

// Function to replace image sources for specified element:
function replaceImageSources(element) {
    var ProfileBodyShot = element.querySelectorAll('.thumbnail-2d-container.thumbnail-span');
    var ProfileHeadShot = element.querySelectorAll('.thumbnail-2d-container.avatar-card-image.profile-avatar-thumb');
    var HomeFriendList = element.querySelectorAll('.thumbnail-2d-container');
    var GameIcons1 = element.querySelectorAll('.thumbnail-2d-container.game-card-thumb-container');
    var GameIcons2 = element.querySelectorAll('.thumbnail-2d-container.brief-game-icon');
    var GameIconsDiscover = element.querySelectorAll('.thumbnail-2d-container.game-card-thumb');
    var MarketplaceIcons = element.querySelectorAll('.ng-isolate-scope');
    var ItemPage= element.querySelectorAll('.thumbnail-2d-container');

    // Replace image sources for the first set of elements
    enqueueReplacements(ProfileBodyShot, 'Webp/noFilter', 'PNG/noFilter');
    enqueueReplacements(ProfileHeadShot, 'Webp/noFilter', 'PNG/noFilter');
    enqueueReplacements(HomeFriendList, 'Webp/noFilter', 'PNG/noFilter');
    enqueueReplacements(GameIcons1, 'Webp', 'PNG');
    enqueueReplacements(GameIcons2, 'Webp/noFilter', 'PNG/noFilter');
    enqueueReplacements(GameIconsDiscover, 'Webp', 'PNG');
    enqueueReplacements(MarketplaceIcons, 'Webp', 'PNG');
    enqueueReplacements(ItemPage, 'Webp', 'PNG');

    processQueue();
}

// Function to enqueue replacements:
function enqueueReplacements(elements, partialOriginalSrc, newSrc) {
    elements.forEach(function(element) {
        replacementQueue.push({ element: element, partialOriginalSrc: partialOriginalSrc, newSrc: newSrc });
    });
}

// Function to process replacement queue:
function processQueue() {
    if (!isProcessingQueue && replacementQueue.length > 0) {
        isProcessingQueue = true;
        var replacement = replacementQueue.shift();
        replaceImageSource(replacement.element, replacement.partialOriginalSrc, replacement.newSrc);
        setTimeout(function() {
            isProcessingQueue = false;
            processQueue();
        }, 0);
    }
}

// Call the function once initially to replace image sources on page load:
replaceImageSources(document);

// v1.2 
// https://github.com/Knewest
// Copyright (Boost Software License 1.0) 2024-2024 Knew
```

### Comparison between lossy WebP and PNG (WebP/PNG):
![PNG](https://github.com/Knewest/Roblox-WebP-To-PNG/assets/94736474/1b43a4de-9df1-4856-847d-fbae77eea88d)
