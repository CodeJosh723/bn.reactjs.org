---
id: thinking-in-react
title: React এ চিন্তা করা
permalink: docs/thinking-in-react.html
redirect_from:
  - 'blog/2013/11/05/thinking-in-react.html'
  - 'docs/thinking-in-react-zh-CN.html'
prev: composition-vs-inheritance.html
---

আমাদের মতে, React হচ্ছে বড় এবং দ্রুত ওয়েব অ্যাপ বানানোর সবথেকে ভালো উপায়। আমাদের ফেইসবুক এবং ইনস্টাগ্রামে এটি খুব ভালো স্কেল হয়েছে।

React এর অন্যতম চমৎকার দিক হচ্ছে, এটা দিয়ে অ্যাপ তৈরির সময় যেভাবে ভাবা লাগে। এই লেখাটিতে, React এ একটি সার্চ উপোযোগী প্রোডাক্ট টেবিল বানিয়ে, এই চিন্তা করার বিষয়টি তুলে ধরবো।

## মক থেকে শুরু করি {#start-with-a-mock}

মনে করি, আমাদের কাছে ইতোমধ্যে একটি JSON API এবং ডিজানারের তৈরি একটি মক আছে। মকটি এরকম:

![Mockup](../images/blog/thinking-in-react-mock.png)

আমাদের JSON API কিছু ডেটা পাঠায় যা এমন:

```
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## ধাপ ১: UI কে কম্পোনেন্ট Hierarchy তে ভেঙ্গে ফেলা  {#step-1-break-the-ui-into-a-component-hierarchy}

একদম শুরুতে যে কাজটা আপনার করা উচিৎ, তা হলো প্রতিটি কম্পোনেন্ট (এবং সাবকম্পোনেন্ট) এর চারদিকে বক্স আঁকানো এবং প্রতিটার একটি নাম দেওয়া। আপনার ডিজাইনার হয়তো আগেই এটা করে ফেলেছে, তাই তার সাথে যোগাযোগ করুণ! তার ফটোশপ লেয়ারের নামই হয়ত হতে পারে আপনার React কম্পোনেন্ট এর নাম!

কিন্ত আপনি কিভাবে বুঝবেন যে কো কাজটার নিজেরই একটা কম্পোনেন্ট থাকা দরকার? একটা নতুন ফাংশন কিংবা নতুন অবজেক্ট লাগবে কিনা যেভাবে চিন্তা করেন, ঠিক সেভাবেই চিন্তা করুণ। এমন পরিচিত একটা পদ্ধতি হলো [Single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), যার মতে, সাধারণত একটি কম্পোনেন্ট এর শুধু একটি কাজই করা উচিৎ। বড় হয়ে গেলে একে ক্ষুদ্র ক্ষুদ্র সাবকম্পোনেন্ট ভেঙ্গে ফেলা উচিৎ। 

যেহেতু আপনি প্রায়শই একটি JSON ডেটা মডেল ব্যবহারকারীকে দেখাচ্ছেন, খেয়াল করে থাকবেন আপনার মডেল যদি সঠিকভাবে তৈরি হয়ে থাকে, আপনার UI (এবং আপনার কম্পোনেন্ট এর গঠনবিন্যাস) সুন্দরমত মিলে যাবে। এর কারণ হচ্ছে UI এবং ডেটা মডেল একই *ইনফরমেশন আর্কিটেকচার* অনুসরণ করে। আপনার UI কে কম্পোনেন্ট গুলোকে এমনভাবে আলাদা করুণ, যেন আপনার প্রতিটি কম্পোনেন্ট একটি ডেটার মডেলের সাথে মিলে যায়।

![Component diagram](../images/blog/thinking-in-react-components.png)

এখানে আপনি দেখবেন যে আমাদের অ্যাপ এ পাঁচটি কম্পোনেন্ট রয়েছে। আমরা প্রতিটা কম্পোনেন্ট এর উপস্থাপিত ডেটা ইটালিক করে দিয়েছি।

  1. **`FilterableProductTable` (কমলা):** সম্পূর্ণ উদাহরণটি এর ভিতরে আছে
  2. **`SearchBar` (নীল):** সকল *ইউজার ইনপুট* গ্রহণ করে
  3. **`ProductTable` (সবুজ):** *ইউজার ইনপুট*  এর ভিত্তিতে *ডেটা কালেকশন* দেখায় এবং ফিল্টার করে
  4. **`ProductCategoryRow` (ফিরোজা):** প্রতিটি *ক্যাটাগরী* এর জন্য শিরোনাম দেখায়
  5. **`ProductRow` (লাল):** প্রতিটি *পণ্যর* জন্য একটি সারি দেখায়

যদি `ProductTable` এর দিকে লক্ষ্য করেন, আপনি দেখবেন প্রতিটি টেবিলের শিরোনাম (যাতে "Name" এবং "Price" লেবেল আছে) এর নিজের কম্পোনেন্ট নয়। এটা ব্যক্তিগত পছন্দ এবং যেকোনো দিকেই যুক্তি প্রদর্শন সম্ভব। উদাহরণস্বরূপ, আমারা এটাকে `ProductTable`এ রেখেছি কারণ এটি *ডেটা কালেকশন* দেখানোর অংশ, যা `ProductTable` এর কাজ। কিন্তু, যদি এই শিরোনামটি আরো জটিল আকার ধারণ করে (যেমন: সর্ট করার আরো কিছু উপায় করে), তখন শিরোনাম এর জন্য আলাদা `ProductTableHeader` কম্পোনেন্ট তৈরি যুক্তিসংগত হবে।

এখন যেহেতু আমরা মক এর জন্য কম্পোনেন্ট চিহ্নিত করে ফেলেছি, এদেরকে একটি hierarchy তে সাজিয়ে ফেলি। মকে যেই কম্পোনেন্ট অন্য কম্পোনেন্ট এর মধ্যে দেখা যায়, সেগুলো hierarchy তে চাইল্ড হিসেবে দেখানো উচিৎ:

  * `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
      * `ProductCategoryRow`
      * `ProductRow`

## ধাপ ২ঃ React ব্যবহার করে একটি স্ট্যাটিক ভার্শন তৈরি {#step-2-build-a-static-version-in-react}

<p data-height="600" data-theme-id="0" data-slug-hash="BwWzwm" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">Pen দেখুন<a href="https://codepen.io/gaearon/pen/BwWzwm">React এ চিন্তা করা: ধাপ ২</a> এটি <a href="https://codepen.io">CodePen</a> এ।</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

যেহেতু কম্পোনেন্টের Hierarchy তৈরী হয়ে গিয়েছে, এখন অ্যাপ তৈরির জন্য আপনি প্রস্তুত। সবথেকে সহজ উপায় হচ্ছে অ্যাপটির এমন একটি ভার্শন তৈরী, যেটি আপনার ডেটা মডেল গ্রহণ করে UI এ দেখাবে, কিন্তু এতে Interactivity থাকবে  না। এই দুইটা কাজ আলাদা করাটা সবচেয়ে ভাল বুদ্ধি কারণ, স্ট্যাটিক ভার্শন তৈরীর সময় প্রচুর কোড লিখতে হয়, অল্প চিন্তা করতে হয়।  অন্যদিকে অ্যাপটি Interactive করতে অনেক চিন্তা করতে হয়, কিন্তু লিখতে হয় কম। এখনি আমরা এটা দেখবো।

আপনার অ্যাপ এর স্ট্যাটিক ভার্শন, যেটি শুধু ডেটা মডেল দেখায়, তা তৈরি করর জন্য এমন কম্পোনেন্ট বানানো উচিৎ যা অন্য কম্পোনেন্ট পুনঃব্যবহার করে এবং *Props* এর মাধ্যমে ডেটা আদান প্রদান করে। *Props* হচ্ছে প্যারেন্ট কম্পোনেন্ট থেকে চাইল্ড  কম্পোনেন্ট এ ডেটা আদান প্রদানের একটি মাধ্যম। আপনার যদি *State* এর বিষয়ে ধারণা থেকে থাকে তাহলে স্ট্যাটিক ভার্শন তৈরীর সময় অবশ্যই **State ব্যবহার করবেন না**। শুধুমাত্র অ্যাপটি Interactive করার জন্য State ব্যবহার করা যাবে। অর্থাৎ, সেই ডেটার জন্য ব্যবহার করা যাবে যা সময়ের সাথে পরিবর্তিত হতে পারে। যেহেতু এটি একটি স্ট্যাটিক ভার্শন, এতে State এর দরকার নেই।

আপনি উপর থেকে নিচে (টপ-ডাউন) বা নিচে থেকে উপরে (বটম আপ) পদ্ধতিতে অ্যাপটি তৈরি করতে পারেন। অর্থাৎ আপনি শুরুতে hierarchy এর উপরের দিকের কম্পোনেন্টগুলো আগে তৈরি করতে পারেন (যেমন `FilterableProductTable` দিয়ে শুরু করা), অথবা নিচের দিকের কম্পোনেন্ট দিয়ে শুরু করতে পারেন (`ProductRow`)। অপেক্ষাকৃত সহজ কোন উদাহরণে সাধারণত টপ-ডাউন তৈরি করাই ভাল, অন্যদিকে একটু বড় ধরণের কোন অ্যাপ এ বটম-আপ তৈরি করলে ভালো কারণ এতে আপনি শুরু থেকেই টেস্ট লিখে আগাতে পারবেন।

এই ধাপের শেষে আপনার কাছে পুনর্ব্যবহার যোগ্য কিছু কম্পোনেন্ট এর লাইব্রেরী থাকবে। এই কম্পোনেন্ট গুলার শুধু একটি মাত্র মেথড ই থাকবে, তা হচ্ছে `render()` কারণ, এটি অ্যাপ এর স্ট্যাটিক ভার্শন। Hierarchy এর শীর্ষে অবস্থান করা কম্পোনেন্ট (`FilterableProductTable`) আপনার ডেটা মডেলকে একটি Prop হিসেবে গ্রহণ করবে। আপনি যদি ডেটা মডেলে অভ্যন্তরীণ কোন পরিবর্তন করে `ReactDOM.render()` মেথডটিকে আবার কল করেন, UI সেই হিসেবে আপডেটেড হয়ে যাবে। আপনি এখন দেখতে পাচ্ছেন, আপনার UI তে পরিবর্তন আনতে কোডের কোন যায়গায় পরিবর্তন আনতে হবে। React এর **One-way data flow** বা একমুখী তথ্য প্রবাহ (যাকে *one-way binding* ও বলা হয়) সবকিছু কে মডুলার এবং দ্রুতগতির রাখে।

এই ধাপটি সম্পন্ন করতে যদি সাহায্য প্রয়োজন হয়, [React ডকুমেন্টেশন](/docs/) এ দেখতে পারেন।

### অল্প একটু বিরতি: Prop বনাম State {#a-brief-interlude-props-vs-state}

React এ দু'ধরণের "model" বা আদর্শ ডেটা আছেঃ Prop এবং State। এদের মধ্যকার পার্থক্য জানা খুবই দরকারি। যদি আপনি এই পার্থক্য সম্বন্ধে নিশ্চিত না হয়ে থাকেন, তাহলে [অফিশিয়াল রিয়েক্ট ডকে](/docs/state-and-lifecycle.html) এ চোখ বুলিয়ে দেখতে পারেন। আরো দেখতে পারেন [FAQ: What is the difference between state and props?](/docs/faq-state.html#what-is-the-difference-between-state-and-props)

## Step 3: Identify The Minimal (but complete) Representation Of UI State {#step-3-identify-the-minimal-but-complete-representation-of-ui-state}

To make your UI interactive, you need to be able to trigger changes to your underlying data model. React achieves this with **state**.

To build your app correctly, you first need to think of the minimal set of mutable state that your app needs. The key here is [DRY: *Don't Repeat Yourself*](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). Figure out the absolute minimal representation of the state your application needs and compute everything else you need on-demand. For example, if you're building a TODO list, keep an array of the TODO items around; don't keep a separate state variable for the count. Instead, when you want to render the TODO count, take the length of the TODO items array.

Think of all of the pieces of data in our example application. We have:

  * The original list of products
  * The search text the user has entered
  * The value of the checkbox
  * The filtered list of products

Let's go through each one and figure out which one is state. Ask three questions about each piece of data:

  1. Is it passed in from a parent via props? If so, it probably isn't state.
  2. Does it remain unchanged over time? If so, it probably isn't state.
  3. Can you compute it based on any other state or props in your component? If so, it isn't state.

The original list of products is passed in as props, so that's not state. The search text and the checkbox seem to be state since they change over time and can't be computed from anything. And finally, the filtered list of products isn't state because it can be computed by combining the original list of products with the search text and value of the checkbox.

So finally, our state is:

  * The search text the user has entered
  * The value of the checkbox

## Step 4: Identify Where Your State Should Live {#step-4-identify-where-your-state-should-live}

<p data-height="600" data-theme-id="0" data-slug-hash="qPrNQZ" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">See the Pen <a href="https://codepen.io/gaearon/pen/qPrNQZ">Thinking In React: Step 4</a> on <a href="https://codepen.io">CodePen</a>.</p>

OK, so we've identified what the minimal set of app state is. Next, we need to identify which component mutates, or *owns*, this state.

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. **This is often the most challenging part for newcomers to understand,** so follow these steps to figure it out:

For each piece of state in your application:

  * Identify every component that renders something based on that state.
  * Find a common owner component (a single component above all the components that need the state in the hierarchy).
  * Either the common owner or another component higher up in the hierarchy should own the state.
  * If you can't find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common owner component.

Let's run through this strategy for our application:

  * `ProductTable` needs to filter the product list based on state and `SearchBar` needs to display the search text and checked state.
  * The common owner component is `FilterableProductTable`.
  * It conceptually makes sense for the filter text and checked value to live in `FilterableProductTable`

Cool, so we've decided that our state lives in `FilterableProductTable`. First, add an instance property `this.state = {filterText: '', inStockOnly: false}` to `FilterableProductTable`'s `constructor` to reflect the initial state of your application. Then, pass `filterText` and `inStockOnly` to `ProductTable` and `SearchBar` as a prop. Finally, use these props to filter the rows in `ProductTable` and set the values of the form fields in `SearchBar`.

You can start seeing how your application will behave: set `filterText` to `"ball"` and refresh your app. You'll see that the data table is updated correctly.

## Step 5: Add Inverse Data Flow {#step-5-add-inverse-data-flow}

<p data-height="600" data-theme-id="0" data-slug-hash="LzWZvb" data-default-tab="js,result" data-user="rohan10" data-embed-version="2" data-pen-title="Thinking In React: Step 5" class="codepen">See the Pen <a href="https://codepen.io/gaearon/pen/LzWZvb">Thinking In React: Step 5</a> on <a href="https://codepen.io">CodePen</a>.</p>

So far, we've built an app that renders correctly as a function of props and state flowing down the hierarchy. Now it's time to support data flowing the other way: the form components deep in the hierarchy need to update the state in `FilterableProductTable`.

React makes this data flow explicit to help you understand how your program works, but it does require a little more typing than traditional two-way data binding.

If you try to type or check the box in the current version of the example, you'll see that React ignores your input. This is intentional, as we've set the `value` prop of the `input` to always be equal to the `state` passed in from `FilterableProductTable`.

Let's think about what we want to happen. We want to make sure that whenever the user changes the form, we update the state to reflect the user input. Since components should only update their own state, `FilterableProductTable` will pass callbacks to `SearchBar` that will fire whenever the state should be updated. We can use the `onChange` event on the inputs to be notified of it. The callbacks passed by `FilterableProductTable` will call `setState()`, and the app will be updated.

## And That's It {#and-thats-it}

Hopefully, this gives you an idea of how to think about building components and applications with React. While it may be a little more typing than you're used to, remember that code is read far more than it's written, and it's less difficult to read this modular, explicit code. As you start to build large libraries of components, you'll appreciate this explicitness and modularity, and with code reuse, your lines of code will start to shrink. :)
