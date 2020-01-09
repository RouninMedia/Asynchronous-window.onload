# Asynchronous window.onload for Ashiva
A function which mimics `window.onload` and `window.onDOMContentLoaded` in post-onload environments where these event listeners would not normally work.

In an asynchronous script which is dynamically generated and appended to the `document.body` only at the point where the window has loaded:

```
const loadDynamicScript = () => {

  const dynamicScript = document.createElement('script');
  dynamicScript.src = '/my-dynamic-script.js';
  document.body.appendChild(dynamicScript);
}

window.addEventListener('load', loadDynamicScript);
```

any `window.onload` or `window.onDOMContentLoaded`event listener will never fire, because `my-dynamic-script.js` itself did not exist prior to `window.onload` firing.

The following function will mimic both `window.onload` or `window.onDOMContentLoaded`event listener in such an environment:

```
const window_addEventListener = (eventName, callback, useCapture = false) => {

  if ((document.readyState === 'interactive') || (document.readyState === 'complete'))   {

    callback();
  }
}
```

this means that in place of:

 - `window.addEventListener('load', activateMyFunction);` and
 - `window.addEventListener('DOMContentLoaded', activateMyFunction);`
 
 we can now call:

 - `window_addEventListener('load', activateMyFunction);` and
 - `window_addEventListener('DOMContentLoaded', activateMyFunction);`



