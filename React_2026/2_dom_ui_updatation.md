Excellent question. The answer is:

**DOM updates happen one by one in JavaScript memory, but UI usually updates together (batched) on the next render frame.**

Let's see it.

Suppose:

```js
document.getElementById("a").innerText = "A";
document.getElementById("b").innerText = "B";
document.getElementById("c").innerText = "C";
```

JavaScript executes line by line:

```text
Update node A
      ↓
Update node B
      ↓
Update node C
```

So yes, the **DOM objects are modified one by one**.

But the browser does not immediately repaint the screen after every line.

Instead it usually does:

```text
JS execution starts
      ↓
A changed
B changed
C changed
...
1000 changes
      ↓
JS execution finishes
      ↓
Browser renders
      ↓
User sees all changes together
```

---

## Example

```js
for(let i=0; i<1000; i++){
   document.getElementById(`item-${i}`).innerText = "Updated";
}
```

Internally:

```text
DOM update #1
DOM update #2
DOM update #3
...
DOM update #1000
```

But the user usually sees:

```text
Old UI
   ↓
New UI
```

Not:

```text
1 updated
2 updated
3 updated
...
1000 updated
```

Because rendering is deferred until JavaScript yields control back to the browser.

---

## Why?

JavaScript runs on the main thread.

While JS is executing:

```text
Main Thread
   |
   +-- JavaScript running
```

The browser generally cannot paint the screen in the middle of that synchronous execution.

So:

```js
for(let i=0; i<1000; i++){
   // DOM changes
}
```

The browser waits.

After loop completes:

```text
JS finished
    ↓
Layout
    ↓
Paint
    ↓
Display
```

---

## Then why is DOM considered expensive?

Because some operations force the browser to recalculate layout.

Example:

```js
div.style.width = "500px";
```

Browser marks layout dirty.

Now imagine:

```js
for(let i=0; i<1000; i++){
   div.style.width = `${i}px`;
}
```

Even though painting may be batched, the browser still has to track many changes.

---

## The Really Bad Case

Suppose:

```js
for(let i=0; i<1000; i++){

   div.style.width = `${i}px`;

   console.log(div.offsetWidth);
}
```

`offsetWidth` forces the browser to compute layout immediately.

Flow:

```text
Change DOM
   ↓
Calculate Layout
   ↓
Read width

Change DOM
   ↓
Calculate Layout
   ↓
Read width

...
```

This is called:

```text
Layout Thrashing
```

and is very expensive.

---

## Think of the Browser Like a Teacher

Imagine a teacher checking homework.

Bad:

```text
Student 1 change
Teacher checks

Student 2 change
Teacher checks

Student 3 change
Teacher checks
```

1000 inspections.

Good:

```text
Student 1 change
Student 2 change
...
Student 1000 change

Teacher checks once
```

This is what browser batching tries to achieve.

---

## Connection to React

Suppose 1000 state updates happen.

React doesn't immediately update the real DOM 1000 times.

Instead:

```text
State changes
      ↓
React batches updates
      ↓
Virtual DOM updated
      ↓
Diff calculated
      ↓
Minimal real DOM updates
      ↓
Browser paints once
```

That's one reason React can be much faster than manually manipulating many DOM elements.

### Mental model to remember

```text
JavaScript changes DOM one operation at a time.

Browser usually paints the UI later as a batch.

So:
DOM updates = one by one
Screen updates = usually together
```

That's why when you update 1000 elements inside one synchronous loop, the user normally sees the final result appear all at once.
