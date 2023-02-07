:warning: **WARNING** :warning: we no longer recommend measuring TTI [in the field](https://web.dev/user-centric-performance-metrics/#in-the-field); instead, we recommend measuring [FID](https://web.dev/fid/), which can be done using the [`web-vitals`](https://github.com/GoogleChrome/web-vitals) JavaScript library. TTI will continue to be supported in [lab-measurement tools](https://web.dev/user-centric-performance-metrics/#in-the-lab) like [Lighthouse](https://github.com/GoogleChrome/lighthouse).

* * *

Time to Interactive Polyfill
============================

A polyfill for the Time to Interactive metric. See the [metric definition](https://goo.gl/OSmrPk) for in-depth implementation details.

## Installation

You can install the TTI polyfill from npm by running:

```sh
npm install tti-polyfill
```

## Usage

Import the module into your application code and invoke the `getFirstConsistentlyInteractive()` method. The `getFirstConsistentlyInteractive()` method returns a promise that resolves to the TTI metric value (in milliseconds since navigation start). If no TTI value can be found, or if the browser doesn't support all the APIs required to detect TTI, the promise resolves to `null`.

```js
import ttiPolyfill from './path/to/tti-polyfill.js';

ttiPolyfill.getFirstConsistentlyInteractive(opts).then((tti) => {
  // Use `tti` value in some way.
});
```

Note that this method can be invoked at any time, it does not need to be called prior to interactivity being reached. This allows you to load the polyfill via `<script async>`, so it doesn't block any other critical resources.

### Configuration options

The following table outlines the configuration options you can pass to the `getFirstConsistentlyInteractive()` method:

<table>
  <tr valign="top">
    <th align="left">Name</th>
    <th align="left">Type</th>
    <th align="left">Description</th>
  </tr>
  <tr valign="top">
    <td><code>minValue</code></td>
    <td><code>number|null</code></td>
    <td>
      The lower bound to start forward-searching for the quite window. If no value is set, the default is after the <code>DOMContentLoaded</code> event.
    </td>
  </tr>
  <tr valign="top">
    <td><code>useMutationObserver</code></td>
    <td><code>boolean</code></td>
    <td>
      When true (the default), a mutation observer is used to detect when added DOM elements will create additional network requests. This can be disabled to improve performance in cases where you know no additional request-creating DOM elements will be added.
    </td>
  </tr>
</table>

### The debug version

A [debug](https://github.com/GoogleChrome/tti-polyfill/blob/master/tti-polyfill-debug.js) version of the polyfill ships with this repo that includes helpful `console.log()` statements that can be used to better understand how the polyfill is working under the hood.

*__Note:__ usage for the debug version is exactly the same as the regular version.*

## Browser support

The TTI polyfill will work in any browser that supports [`PerformanceObserver`](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver) and the [`PerformanceLongTaskTiming`](https://w3c.github.io/longtasks/) entry.

At the moment this is Chrome 58+.
