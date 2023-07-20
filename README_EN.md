Create an application that will be compiled into two files, plugin.js and plugin.css, and can be embedded into any application through the window object. Create a project on GitHub and provide us with the link upon completing the task.

Example of application invocation:


```js
document.addEventListener("DOMContentLoaded", () => {
  window.MY_APP &&
    window.MY_APP.init({
      selector: "#root"
    });
});
```

The application should be built using the following stack:

**React, Typescript, Effector, Vite**

and should have the following options:

```js
document.addEventListener("DOMContentLoaded", () => {
  window.MY_APP &&
    window.MY_APP.init({
      selector: "#root",
      options: {
        initializedOptions: ['A1', 'C2', 'B4', 'A2'], // array of active positions
        onPositionChange(positions) {
          // called when positions change and returns an array of active positions
          console.log(postions);
        },
        onComplete(positions) {}, // called when events are confirmed
        onInit() {} // called on plugin initialization
      }
    });
});
```

This can be implemented using a wrapper function around the render function:

```js
if (import.meta.env.MODE  ===  'development') {
  const  renderElement  =  document.getElementById('root');
  createRoot(renderElement  as  HTMLElement).render(
    <App
      options={{
         ...someOPtionsOnlyForDev
      }}
    />
  );
} else {
	window.DAMAGE_SELECTOR_API  = {
		init: (options: PluginOptions) => {
			const { selector } = options;
			if (selector) {
				const  renderElement = document.querySelector(selector);
				if (renderElement) {
					createRoot(renderElement as HTMLElement).render(
						<ErrorBoundary>
							<App  options={options}  />
						</ErrorBoundary>
					);
				}
			}
		}
	};
}
```

The application should look like this::
![enter image description here](https://i.ibb.co/gPjQxxY/Screenshot-2023-07-10-141447.png)

- The image of the car and the dots on it represent buttons indicating the car damages.
- Each dot has its own letter, and when a button is clicked, the onPositionChange callback is triggered, passing an array of active positions to the callback.
- If a dot is active, it becomes orange.
- Clicking on a dot triggers the onComplete callback, which receives an array of positions.

Position schema:
![enter image description here](https://i.ibb.co/j41F55z/Screenshot-2023-07-10-142359.png)

Effector:
During the plugin initialization using Effector, we need to make a request to a mini API to fetch the current list of positions:

GET: https://myfailemtions.npkn.net/b944ff/

When a dot is clicked, we need to send a request to the API with an array of active positions:

POST: https://myfailemtions.npkn.net/b944ff/
`["A1", "B2"]`

All of this should be implemented using Effector.
