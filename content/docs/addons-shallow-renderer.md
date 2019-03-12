---
id: shallow-renderer
title: Shallow Renderer
permalink: docs/shallow-renderer.html
layout: docs
category: Reference
---

**Importing**

```javascript
import ShallowRenderer from 'react-test-renderer/shallow'; // ES6
var ShallowRenderer = require('react-test-renderer/shallow'); // ES5 with npm
```

## সারমর্ম {#overview}

React এ unit test লেখার জন্য shallow rendering বেশ সুবিধাজনক। Shallow rendering আপনাকে "এক ধাপ নিচ পর্যন্ত" একটি কম্পোনেন্টকে রেন্ডার করতে সহায়তা করে এবং কম্পোনেন্টটির রেন্ডার মেথড কি রিটার্ন করে সেটি যাচাই করে, এক্ষেত্রে তার অভ্যন্তরীণ কম্পোনেন্টগুলো, যেগুলা এখনো তৈরি অথবা রেন্ডার হয়নি তাদের ধরণ আমলে নেওয়া হয় না। এই প্রক্রিয়ার জন্য কোন DOM এর প্রয়োজন হয় না।


উদাহরণ হিসেবে, যদি আপনি নিচের মতো একটি কম্পোনেন্ট লিখুন :

```javascript
function MyComponent() {
  return (
    <div>
      <span className="heading">Title</span>
      <Subcomponent foo="bar" />
    </div>
  );
}
```

তখন আপনি শর্ত আরোপ করতে পারেন:

```javascript
import ShallowRenderer from 'react-test-renderer/shallow';

// in your test:
const renderer = new ShallowRenderer();
renderer.render(<MyComponent />);
const result = renderer.getRenderOutput();

expect(result.type).toBe('div');
expect(result.props.children).toEqual([
  <span className="heading">Title</span>,
  <Subcomponent foo="bar" />
]);
```

Shallow testing এর বর্তমানে কিছু সীমাবদ্ধতা আছে, যেমন এটি ref সমর্থন করে না।

> নোট:
>
> আমরা পরামর্শ দেই, আপনি Enzyme এর  [Shallow Rendering API](https://airbnb.io/enzyme/docs/api/shallow.html) -টি দেখুন। এটি আরও সুন্দর একটি উঁচু-স্তরের API দেয়, যা দিয়ে একই কাজ করা সম্ভব। 

## রেফারেন্সে {#reference}

### `shallowRenderer.render()` {#shallowrendererrender}

আপনি shallowRenderer কে এমন একটা "জায়গা" হিসেবে চিন্তা করতে পারেন, যেখানে আপনি যে কম্পোনেন্ট নিয়ে পরীক্ষা করছেন তা ঐ  কম্পোনেন্ট টি  render করার স্থান এবং এখান থেকে আপনি আপনার পরীক্ষামূলক কম্পোনেন্ট এর আউটপুট পেতে পারেন।

`shallowRenderer.render()` অনেকটা [`ReactDOM.render()`](/docs/react-dom.html#render) এর অনুরূপ। তবে `shallowRenderer.render()` এর DOM লাগে না এবং এটি শুধুমাত্র এক স্তর গভীর পর্যন্ত render করে। এর অর্থ হলো আপনি কোনো কম্পোনেন্ট কে তার অভ্যন্তরীণ কম্পোনেন্টগুলো থেকে আলাদা করে পরীক্ষা করতে পারবেন।

### `shallowRenderer.getRenderOutput()` {#shallowrenderergetrenderoutput}

`shallowRenderer.render()` ব্যবহার করার পর আপনি `shallowRenderer.getRenderOutput()` ব্যবহার করে shallowly rendered আউটপুট পেতে পারবেন।

আপনি তারপর শর্ত আরোপ করার মাধ্যমে নির্ধারণ করতে পারবেন আউটপুট কি হবে।
