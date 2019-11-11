# Implementing Loading Indicators

Facebook did a survey and they found user perception changed because of 1 icon: the loading indicator.

![Experiment](https://mercury.io/wp-content/uploads/2014/02/facebook_loading_animation.png)

- Using their own indicator (vertical bars), people blamed loading performance on Facebook's mobile app.

- Using an iOS-styled indicator, people attributed loading performance on the Apple operating system.

User experience is easily influenced and has broad implications on your program's credibility!

## Loading Styles

#### Eager load: load everything at once
-   For pages that change quickly, i.e. social media feeds, single player games
#### Lazy load: load bits on demand
- For heavy assets and slower paced sites, i.e. image galleries, leisurely displays
#### Hybrid
- Buffers, i.e. infinite scrolling, multiplayer games

### Traffic
Eager loading is reliant on the client side, while lazy-loading is reliant on the server.

---
## Placeholders
Placeholder can be an image or any HTML element (styled div, svg)
#### Pre-set placeholder
[Example SVG](https://codepen.io/ainalem/pen/aLKxjm)
<svg xmlns="http://www.w3.org/2000/svg" width="450" height="300" viewBox="0 0 450 300"><path d="M107.37 300l2.052-17.78-1.367-3.42 7.523-21.885 2.735-9.575-1.368-6.155 3.42-3.42-2.736-2.735 1.367-6.155-4.103-2.05 2.05-2.737-3.418-2.052h6.155l-3.42-4.103h4.105l-6.84-4.103 9.575-2.052-2.05-4.103-6.156 1.368 9.574-6.84.684-2.05-10.942 4.102 12.31-11.626 3.42-4.788-7.524 2.052 6.155-4.104-1.366-2.05 5.47-4.79 3.42-8.205 4.787-8.89 2.737-11.627 2.736-4.787 2.735-4.103 5.47-3.42 3.42-3.42 4.104-11.625 5.47-8.89 8.892-9.575.684-4.103-10.26-23.936-10.94-13.678-9.575-8.89-10.943-9.575-8.206-9.575-4.788-13.677-3.42-4.788L114.724 0h5.642l7.523 16.87 6.155 11.625 8.89 9.917 7.865 5.813 5.13.342 4.102-1.71 2.394-6.497-.342-9.574-.684-11.626v-1.71L160.714 0h3.42l.342 7.295.684 7.18.342 6.156 2.735 8.207.684 6.84-.34 6.838-.343 7.865 1.368 4.445 3.077 6.497 3.42 5.813 4.787 10.26 2.735 9.573 4.787 3.078 2.393 2.735-.683 9.234 2.05 4.787 1.37 3.078 2.393.684 3.42.684 1.71 2.736-1.71 1.368-1.37.683 1.027 5.814 1.026 1.025 3.078-7.18 2.735-6.497-1.367-11.285-2.735-7.864-3.078-2.736-5.47-3.077-4.446-3.42-2.736-4.787-3.077-4.445-6.496-10.6-5.47-15.045-1.027-4.104.342-3.418h1.367l1.367 3.42 2.394 7.864 5.813 9.575 2.737 5.472 5.13 5.47 3.76 5.13 4.445 2.736 3.762.683h2.052l1.71-2.05 1.367-3.762-.342-7.864v-4.788l-1.367-4.787-1.367-3.762-.684-7.523V41.49l1.025-3.078 2.052-2.052.683 1.026-.684 3.42-.343 3.42.684 2.734 1.027 4.104 2.394 6.497 2.052 5.13 1.026 5.47.684 10.26v5.13l.684 4.786 1.71 8.207-.342 6.155-.684 5.813-2.052 6.498-2.052 3.42-1.71 1.71.685 1.025 3.078 3.76v1.71l-1.368 1.37-.684 2.734 3.762 2.052 3.76 6.154 3.08 3.76 5.812 3.08 16.755 7.863 7.18 4.788 25.99 14.02 14.36 7.18 12.994 6.155 5.13 4.788 3.076 7.18 2.052 6.155-4.446 7.523-4.103 6.84-2.395 3.42-4.445 4.102-6.84 3.077-4.786 1.026-7.523-.683h-2.734l-.342-1.367-3.762.683-1.025-1.367-3.078.684-7.182.683-5.47 2.052-3.078 3.42 11.284 3.077 1.026 2.052 23.936-6.155 5.13-2.053 1.025 1.368 6.156-1.71 1.026 1.71 8.89-2.052.342 2.052 6.497-2.052 4.104 1.71 6.84-4.103 4.445 2.05 10.94-1.367h11.627l6.497 1.71 4.445 4.445 12.65 6.497 1.027 3.078 9.233 5.813 8.89 9.233 4.104 5.47 6.155 5.813 6.154 8.89-.684 3.078 3.077 7.866 1.026 4.445-.342 5.13z" fill="#bdcdd4" fill-rule="evenodd"></path></svg>


#### Server-sided processing
<a href="https://engineering.fb.com/android/the-technology-behind-preview-photos/
"><img src="https://engineering.fb.com/wp-content/uploads/2015/08/gdaxrgc4zz8xrocbagqcne8aaaaabj0jaaab.jpg" alt="drawing" width="600"/></a>

### Summary
1. Server-sided image processing: images shrunk to 200kb
2. Blur CSS on client side to hide pixelation
3. Deliver shrunk image, pre-load full image
4. Render full image and remove blur

### <em>Caveat: Flex content</em> 
If images are dynamically loaded, varied image sizes means that additional code is needed to make the placeholder scalable, otherwise image dimension changes are jarring and disrupt the content flow of elements.

---
## IntersectionObserver

A condition to send network request is when an element, such as a placeholder, intersects the viewport.
[Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

```
new IntersectionObserver observer(callback, options);
observer.observe(element)
```
#### callback

```
function(entries, observer) {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
	// do stuff
    }
  }
}
```
 [Example: color change on scroll](https://codepen.io/tutsplus/pen/GyyWvw)


## Pre-render image in JS

```
let myImg = new Image();

myImg.onload = function() {
  // replace placeholder
  // add image element
  document.body.appendChild(myImg);
}

myImg.src = “/location/mountain.png”
```
- The `onload` event must be defined before setting `src`

Images in JavaScript have a 'complete' property:
 `myImg.complete` returns true after onload

---
## Debounce
A function that is deferred every time an input is triggered soon enough.
It sets a timer, and when called again, clears and resets the timer. The function runs when the timer finishes.

Most common exmaple is live search, where a search submits the value after the user finishes typing.

```
let helloWorld = function(){alert("Hello World)};

searchBar.onkeyup = debounce(helloWorld, 1000); //1000 ms
```

```
function debounce(callback, wait) {
  var timeout;

  return () => {

    clearTimeout(timeout);

    timeout = setTimeout(() => callback(...arguments), wait);
  };
};
```

### How does it work?
#### Closures
Variables declared inside a function are limited to the function scope and disappear after the function runs. However, you can keep an inner variable persistent by using it in a nested function (known as a closure). Because every function is stored in a reference, the variables used in a function are also referenced and not garbage collected unless the function is also discarded.

In the debounce function, we need to keep a reference of the same timer, otherwise we'd be creating new timers every time someone presses a key.
#### Spread syntax
`arguments` is a reserved keyword that contains the arguments of the function. The `...` array spread syntax from ES6 copies contents of the array _and_ applies them to the function.

`arrayClone = [...arrayOriginal];`

`myFunction.apply(null, args)` equivalent to `myFunction(...args)`


---
## Example Site
[InstaDictionary](./index.html)

1. [Semantic UI](https://semantic-ui.com/) framework to create placeholders
2. InsersectionObserver on an invisible div at the bottom of the page
3. Live search with debounce



# Future: Native HTML spec?
Google Chrome developers hinted towards adding the attribute to the browser.

`<img loading=”lazy” src=... />`

https://www.youtube.com/watch?v=ZBvvCdhLKdw

For now, we still need to implement lazy-loading for backwards compatibility.