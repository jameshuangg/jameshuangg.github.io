---
layout: post
title: 'Google Lighthouse'
date: 2021-12-08 22:40:00 -0500
---

Beware! Lighthouses indicate to mariners they are headed for danger. Likewise, Google's lighthouse is an indicator of performance, accessibility, SEO and more for webpages in the vast ocean that is the internet.

# Performance

The single performance score is a weighted average of the following metrics:

- First Contentful Paint (10%)
  - Measures the time for the first piece of DOM content to render in the browser.
- Speed Index (10%)
  - Measures the time for a page to become visually complete. Visual completeness is achieved when the visual diff to the final frame is minimized.
- Largest Contentful Paint (25%)
  - Measures the render time for the largest image or text block in the user's visible area.
  - Elements considered: \<img\>, \<image\>, \<video\>, background loaded using url(), block-level elements containing text elements.
- Time to interactive (10%)
  - Measures how long it takes a page to set up event handlers and respond to user interactions within 50 milliseconds.
- Total Blocking Time (30%)
  - Measures the total time between FCP and TTI where a page is blocked from responding to user input. Any task that takes longer than 50 ms is blocking and it's duration after 50 ms is summed to calculated TBT.
- Cumulative Layout Shift (15%)
  - Is a metric that scores how much visible elements change position from one frame to the next.
