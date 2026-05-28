
# DOM Deep Dive (Never-Forget Style)

Forget definitions for a moment.

Think about this:

When you open a webpage, browser sees:

```html id="lp5s3d"
<h1>Hello Mohit</h1>
```

But browser cannot directly understand HTML as a “visual page”.

Browser converts HTML into **objects in memory**.

That object structure is called:

# DOM

```text id="uv0r76"
DOM = Document Object Model
```

---

# Real Mental Model

Imagine browser creates a giant JavaScript object tree.

HTML:

```html id="o0cg6n"
<body>

  <h1>Hello</h1>

  <button>Click</button>

</body>
```

Browser internally creates something conceptually like:

```js id="j2f1zb"
document = {

   body: {

      children: [

         {
            tagName:"H1",
            innerText:"Hello"
         },

         {
            tagName:"BUTTON",
            innerText:"Click"
         }

      ]

   }

}
```

THIS is the DOM.

Not HTML.

DOM is the browser's JavaScript representation of HTML.

---

# Never Forget Difference

| HTML             | DOM                       |
| ---------------- | ------------------------- |
| Text file        | JavaScript objects        |
| Static           | Dynamic                   |
| Stored on disk   | Stored in memory          |
| Browser reads it | JavaScript manipulates it |

---

# Step-by-Step Browser Pipeline

When browser loads page:

```text id="05yey5"
1. Download HTML
         ↓
2. Parse HTML
         ↓
3. Create DOM Nodes
         ↓
4. Build DOM Tree
         ↓
5. Render page
```

---

# Important Concept

DOM is a TREE.

Everything becomes nodes.

Example:

```html id="7p9u8q"
<html>

<body>

   <div>

      <h1>Hello</h1>

      <p>Text</p>

   </div>

</body>

</html>
```

DOM tree:

```text id="e0yx14"
Document
   |
 html
   |
 body
   |
 div
  / \
 h1  p
```

---

# Every HTML tag becomes OBJECT

Example:

```html id="ijshjg"
<h1 id="title">
Hello
</h1>
```

Browser creates:

```js id="y3v7rk"
{
   tagName:"H1",
   id:"title",
   innerText:"Hello"
}
```

---

# The `document` Object

Browser gives you:

```js id="6d84jk"
document
```

This is entry point of DOM.

Think:

```text id="rf5r5j"
document
   ↓
entire webpage
```

Example:

```js id="4j7pw2"
console.log(document)
```

You’ll see huge DOM object tree.

---

# Selecting Elements

This is where JavaScript talks to DOM.

---

# 1. getElementById

HTML:

```html id="kmg6eu"
<h1 id="title">
Hello
</h1>
```

JavaScript:

```js id="m5lmbg"
const element =
document.getElementById("title")

console.log(element)
```

Output:

```html id="94icbo"
<h1 id="title">Hello</h1>
```

What happened internally:

```text id="nsj3tg"
document
   ↓
search DOM tree
   ↓
find node with id="title"
   ↓
return object
```

---

# DOM Manipulation

Now JavaScript can change page dynamically.

---

# Changing Text

```js id="4z9z5x"
element.innerText = "Welcome Mohit"
```

Browser updates UI instantly.

Before:

```html id="0hykvk"
<h1>Hello</h1>
```

After:

```html id="yjnm7l"
<h1>Welcome Mohit</h1>
```

---

# SUPER IMPORTANT

Changing DOM object changes REAL PAGE.

Because DOM and UI are connected.

```text id="d8rbfz"
DOM object changes
        ↓
Browser updates screen
```

---

# innerText vs innerHTML

## innerText

Treats value as text.

```js id="d8v92j"
element.innerText = "<h1>Hello</h1>"
```

Output on screen:

```html id="fszv7q"
<h1>Hello</h1>
```

Text only.

---

## innerHTML

Parses HTML.

```js id="oljsvp"
element.innerHTML = "<h1>Hello</h1>"
```

Browser creates real `<h1>` element.

---

# Why innerHTML is dangerous

```js id="8a7r0m"
element.innerHTML = userInput
```

If attacker sends:

```html id="59fj7t"
<script>alert("hack")</script>
```

script may execute.

This is called:

```text id="zc8kxz"
XSS attack
```

---

# Attributes

HTML:

```html id="l2hww9"
<img id="photo">
```

Set attribute:

```js id="4t52fk"
const img =
document.getElementById("photo")

img.setAttribute(
   "src",
   "cat.jpg"
)
```

Result:

```html id="7v7h6q"
<img src="cat.jpg">
```

---

# Styling Through DOM

HTML:

```html id="j1tnfy"
<h1 id="title">
Hello
</h1>
```

JavaScript:

```js id="cnq3rw"
const title =
document.getElementById("title")

title.style.color = "red"

title.style.fontSize = "50px"
```

Browser updates UI immediately.

---

# Creating Elements Dynamically

This is HUGE concept.

JavaScript can create HTML dynamically.

---

## createElement

```js id="k16i3z"
const p =
document.createElement("p")
```

Creates DOM object in memory.

Not visible yet.

Conceptually:

```js id="x7qmgd"
{
   tagName:"P"
}
```

---

# Add Text

```js id="8l4e0z"
p.innerText = "Hello"
```

---

# appendChild

```js id="wh3dbw"
document.body.appendChild(p)
```

Now browser inserts node into DOM tree.

Before:

```text id="7gvjot"
body
```

After:

```text id="6h5kc4"
body
  |
  p
```

UI updates automatically.

---

# REMOVE Element

```js id="xjlwmv"
p.remove()
```

Browser removes node from DOM tree.

---

# DOM Navigation

HTML:

```html id="8r9wnq"
<div id="parent">

   <p>One</p>

   <p>Two</p>

</div>
```

---

# Parent

```js id="m5gk4f"
const parent =
document.getElementById("parent")
```

---

# Children

```js id="8aq7ye"
console.log(parent.children)
```

Output:

```text id="t8d4gk"
HTMLCollection(2)
```

---

# First Child

```js id="40pwy5"
parent.firstElementChild
```

---

# Last Child

```js id="ytq4dw"
parent.lastElementChild
```

---

# Events

Without events, websites are static.

Events make websites interactive.

---

# Example

HTML:

```html id="gk4dms"
<button id="btn">
Click
</button>
```

JavaScript:

```js id="sgnq3j"
const btn =
document.getElementById("btn")

btn.addEventListener(
   "click",
   ()=>{
      alert("Clicked")
   }
)
```

---

# Internally What Happens?

```text id="9muh8u"
User clicks button
        ↓
Browser detects click
        ↓
Creates Event object
        ↓
Find listeners
        ↓
Executes callback
```

---

# Event Object

```js id="bqvrik"
btn.addEventListener(
"click",
(e)=>{

   console.log(e)

})
```

Output:

```js id="plbqwd"
PointerEvent
```

Contains:

```js id="2c4z0f"
e.target
e.clientX
e.clientY
```

---

# Event Bubbling (VERY IMPORTANT)

HTML:

```html id="vd7jmv"
<div id="parent">

   <button id="child">
      Click
   </button>

</div>
```

JavaScript:

```js id="8mx63j"
parent.addEventListener(
"click",
()=>{
   console.log("parent")
})

child.addEventListener(
"click",
()=>{
   console.log("child")
})
```

Click button:

```text id="9ec56t"
child
parent
```

WHY?

Because event moves upward.

Flow:

```text id="ybq96m"
button
   ↑
div
   ↑
body
   ↑
html
```

This is:

```text id="4wwu5z"
Event Bubbling
```

---

# Stop Bubbling

```js id="8xjrxg"
child.addEventListener(
"click",
(e)=>{

e.stopPropagation()

})
```

Now event stops at child.

---

# Memory Trick for DOM

Remember this forever:

```text id="d0zdb0"
HTML = Blueprint

DOM = Live Objects

JavaScript manipulates DOM

DOM updates UI
```

---

# Biggest Problem with DOM

Imagine manually updating 1000 elements:

```js id="6y7t4x"
title.innerText = "A"

count.innerText = "5"

list.appendChild(...)

button.style.color = "red"
```

Huge applications become difficult.

This is WHY React exists.

---

# React vs DOM

Without React:

```text id="bs7blf"
You manually update DOM
```

With React:

```text id="r8m4gm"
You update STATE
        ↓
React updates DOM
```

React internally still uses DOM.

But React automates DOM updates using:

```text id="4d67f0"
Virtual DOM
```

---

# Final Never-Forget Summary

```text id="y88rr0"
HTML file
    ↓
Browser parses HTML
    ↓
Creates DOM tree (objects in memory)
    ↓
JavaScript accesses DOM
    ↓
JavaScript modifies DOM
    ↓
Browser updates screen
```

This is the core foundation behind:

* React
* Angular
* Vue
* Browser rendering
* Events
* UI frameworks
* Virtual DOM
* Re-rendering

Understanding this deeply makes React MUCH easier.


## dom has event lister details also?

Yes — but this is an important subtle concept.

# Short Answer

DOM nodes themselves do **not directly store event listeners as visible HTML properties** like:

```js id="v2zqvq"
button.eventListeners
```

But internally, the browser keeps an **event listener registry/map** associated with DOM nodes.

Conceptually:

```text id="86vk18"
DOM Node
   +
Event Listener List
```

---

# Example

```html id="8ujzqt"
<button id="btn">
Click
</button>
```

JavaScript:

```js id="3m8xgh"
const btn =
document.getElementById("btn")

btn.addEventListener(
   "click",
   ()=>{
      console.log("clicked")
   }
)
```

Internally browser may store something conceptually like:

```js id="q8tqrx"
{
   tagName:"BUTTON",

   listeners:{
      click:[
         callbackFunction
      ]
   }
}
```

Real browser implementation is more complex and engine-specific, but mentally this is correct.

---

# Important Understanding

DOM tree itself mainly represents:

```text id="4ucv6r"
Structure
Attributes
Text
Relationships
```

like:

```html id="f6c1q8"
div
  h1
  p
```

Event listeners are usually managed by browser's:

```text id="ljm6mf"
Event System
```

and attached to DOM nodes internally.

---

# Think of It Like This

A DOM node is like:

```text id="m4p6n8"
Element Object
```

Browser attaches extra metadata:

```text id="3s3x5n"
styles
layout info
paint info
event listeners
```

---

# Deep Internal Flow

When you do:

```js id="7lrx0z"
btn.addEventListener(
   "click",
   handler
)
```

Browser internally:

```text id="n7qj0x"
1. Finds DOM node
        ↓
2. Creates listener entry
        ↓
3. Stores callback reference
        ↓
4. Associates with event type
```

Conceptually:

```text id="3gr3qg"
BUTTON NODE
   |
   +---- click → [handler1, handler2]
   |
   +---- mouseover → [handler3]
```

---

# Event Listeners Are NOT HTML

HTML:

```html id="ys4wbh"
<button>
```

DOM object:

```js id="e9dgbz"
{
   tagName:"BUTTON"
}
```

After listener:

```js id="w67xnd"
{
   tagName:"BUTTON",

   internalListeners:{
      click:[fn]
   }
}
```

But if you inspect HTML:

```html id="xqybqk"
<button>
```

you won’t see listener there.

Because listeners are runtime browser metadata.

---

# Old Style vs Modern Style

## Old style

```html id="93r8bz"
<button onclick="alert('hi')">
```

Here event exists directly in HTML attribute.

Browser converts it internally.

---

## Modern style

```js id="6sjc83"
button.addEventListener(
   "click",
   fn
)
```

Preferred because:

* multiple listeners possible
* separation of HTML & JS
* cleaner architecture

---

# Multiple Listeners

```js id="yzx2f8"
btn.addEventListener(
"click",
()=>{
 console.log("A")
})

btn.addEventListener(
"click",
()=>{
 console.log("B")
})
```

Internally:

```text id="jlwmol"
click
  |
  +--- fnA
  |
  +--- fnB
```

Click output:

```text id="1m1xl6"
A
B
```

---

# Event Listener and Garbage Collection

VERY important internally.

If DOM node removed:

```js id="l78e3n"
button.remove()
```

Browser can garbage collect:

```text id="6rjlwm"
DOM node
listeners
attached memory
```

IF nothing references them anymore.

Otherwise memory leaks happen.

---

# Real Browser Engines

Different engines implement differently:

| Browser | Engine                  |
| ------- | ----------------------- |
| Chrome  | V8 + Blink              |
| Firefox | SpiderMonkey + Gecko    |
| Safari  | JavaScriptCore + WebKit |

Internally they maintain optimized listener tables/maps.

Not plain JS objects.

But conceptually:

```text id="2fe9lt"
DOM Node
   +
Listener Registry
```

is correct.

---

# Very Important React Connection

React usually does NOT attach listeners to every element directly.

React uses:

```text id="lcbw5j"
Event Delegation
```

Example:

Instead of:

```text id="vf03di"
1000 buttons
1000 listeners
```

React may attach:

```text id="hh5w5q"
1 listener on root
```

Then internally determine:

```text id="m6zv7z"
Which child triggered event?
```

This improves performance.

---

# Final Never-Forget Mental Model

```text id="i3d7qo"
DOM
  =
Live object tree of webpage

Event listeners
  =
Browser-managed callbacks attached to DOM nodes
```

And:

```text id="x5n3bs"
User action
     ↓
Browser creates event object
     ↓
Browser checks listener registry
     ↓
Matching callbacks execute
```

