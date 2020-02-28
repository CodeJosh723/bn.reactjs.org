---
id: faq-internals
title: ভার্চুয়াল DOM এবং এর অভ্যন্তরীন
permalink: docs/faq-internals.html
layout: docs
category: FAQ
---

### ভার্চুয়াল DOM কি? {#what-is-the-virtual-dom}
ভার্চুয়াল DOM হলো একটা প্রোগ্রামিং ধারণা যেখানে UI এর একটা আদর্শ অথবা ভার্চুয়াল প্রতিনিধি মেমোরিতে রাখা হয় এবং তা প্রকৃত DOM এর সাথে মিলিয়ে রাখা হয় ReactDOM এর মতো লাইব্রেরির মাধ্যমে। এই কাজটাকে বলে [reconciliation](/docs/reconciliation.html)।

এই প্রক্রিয়া React এর ঘোষণাভিত্তিক API কে সচল করেঃ আপনি বলবেন React কে বলবেন কোন state এ UI থাকতে হবে আর React নিশ্চিত করবে যাতে DOM সেই state এ থাকে। এই প্রক্রিয়া attribute manipulation, event handling এবং manual DOM হালনাগাদের মতো কাজ বাদ দেয়, যা অন্যথায় আপনার করতে হত আপনার অ্যাপ তৈরি করতে।

Since "virtual DOM" is more of a pattern than a specific technology, people sometimes say it to mean different things. In React world, the term "virtual DOM" is usually associated with [React elements](/docs/rendering-elements.html) since they are the objects representing the user interface. React, however, also uses internal objects called "fibers" to hold additional information about the component tree. They may also be considered a part of "virtual DOM" implementation in React.

### Is the Shadow DOM the same as the Virtual DOM? {#is-the-shadow-dom-the-same-as-the-virtual-dom}

No, they are different. The Shadow DOM is a browser technology designed primarily for scoping variables and CSS in web components. The virtual DOM is a concept implemented by libraries in JavaScript on top of browser APIs.

### What is "React Fiber"? {#what-is-react-fiber}

Fiber is the new reconciliation engine in React 16. Its main goal is to enable incremental rendering of the virtual DOM. [Read more](https://github.com/acdlite/react-fiber-architecture).
