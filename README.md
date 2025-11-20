# 🚫 Remove YouTube Comments – Tampermonkey Script

A lightweight userscript that **completely removes** the comments section from **YouTube videos** and **YouTube Shorts**—so you can focus on the content without distractions.

---

## ✨ Features

- Works on **regular YouTube videos** (`youtube.com/watch?v=...`)
- Works on **YouTube Shorts** (`youtube.com/shorts/...`)
- **Permanently removes** comment elements (not just hidden via CSS)
- Handles YouTube’s **dynamic page navigation** (single-page app behavior)
- No timers, no bloat—uses efficient DOM mutation observation
- Compatible with modern YouTube UI (as of 2025)

---

## 🛠️ Installation

1. Install [Tampermonkey](https://www.tampermonkey.net/) (available for Chrome, Firefox, Edge, and other browsers).
2. Click **"Create a new script..."** in Tampermonkey dashboard.
3. Paste the code below (or import this script directly).
4. Save — and enjoy a cleaner YouTube!

---

## 📜 Script Code (`remove-youtube-comments.user.js`)

```javascript
// ==UserScript==
// @name         Remove YouTube Comments
// @namespace    http://tampermonkey.net/
// @version      2025-11-20
// @description  Removes comments section on YouTube videos and Shorts.
// @author       inselberg
// @match        https://www.youtube.com/watch*
// @match        https://www.youtube.com/shorts/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Targets comments sections using multiple reliable selectors
    const selectors = [
        '#comments',                     // Standard video comments
        '#comment-section',              // Alternate comment container
        'ytd-comments',                  // Web Component for comments
        'ytd-comment-section-renderer',  // Legacy comment section
        '#comments-section',             // Sometimes used in newer layouts
    ];

    // Create a single combined selector string
    const combinedSelector = selectors.join(', ');

    // Function to remove comments
    function removeComments() {
        const commentElements = document.querySelectorAll(combinedSelector);
        commentElements.forEach(el => {
            if (el && el.parentNode) {
                el.remove();
            }
        });
    }

    // Initial cleanup
    removeComments();

    // Watch for dynamic content (e.g., SPA navigation in YouTube)
    const observer = new MutationObserver(removeComments);
    observer.observe(document.body, {
        childList: true,
        subtree: true
    });

})();
