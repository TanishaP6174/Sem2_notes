
Closure:
```js
function createCounter() {
  let count = 0; // Private variable
  return function() {
    count++; // Accesses 'count' from the parent scope
    return count;
  };
}

const counter = createCounter(); 
console.log(counter()); // 1
console.log(counter()); // 2
// Even though createCounter() finished, 'counter' still remembers 'count'.
```

js dom:

+  When a web page loads, the browser creates a tree-like representation of the HTML document.

- The **DOM API** (Application Programming Interface) is a set of **Methods** and **Properties** that allow JavaScript to change the content, structure, and style of any HTML elements.

- element_.innerHTML - The HTML content of an element
- element_.textContent - The text content of an element

![[Pasted image 20260427092216.png]]