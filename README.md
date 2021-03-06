# React Integration

Converts web components into React components so that you can use them as first class citizens in your React components.

- Web components become lexically scoped so you can use them as tag names in JSX.
- Listen for custom events triggered from web components declaratively using the standard `on*` syntax.
- Passes React `props` to web components as properties instead of attributes.
- Works with any web component library that uses a standard native custom element constructor, not just Skate or native web components.

## Usage

Web components are converted to React components simply by passing them to the main react-integration function:

```js
import reactify from 'skatejs-react-integration';

// Create your constructor.
const MyComponent = class MyComponent extends HTMLElement {};

// Define your custom element.
const CustomElement = window.customElements.define('my-component', MyComponent);

// Reactify it!
export default reactify(CustomElement);
```

Usage with [SkateJS](https://github.com/skatejs/skatejs) is pretty much the same, Skate just makes defining your custom element easier:

```js
import reactify from 'skatejs-react-integration';

export default reactify(skate('my-component', {}));
```

### Lexical scoping

When you convert a web component to a React component, it returns the React component. Therefore you can use it in your JSX just like any other React component.

```js
const ReactComponent = reactify(WebComponent);
ReactDOM.render(<ReactComponent />, container);
```

### Custom events

Out of the box, React only works with built-in events. By using this integration layer, you can listen for custom events on a web component.

```js
<MyComponent oncustomevent={handler} />
```

Now when `customevent` is emitted from the web component, your `handler` will get triggered.

### Web component properties

When you pass down props to the web component, instead of setting attributes like React normally does for DOM elements, it will set all `props` as properties on your web component. This is useful because you can now pass complex data to your web components.

```js
<MyComponent items={[ 'item1', 'item2' ]} callback={function() {}} />
```

### Children

If your web component renders content to itself, make sure you're using Shadow DOM and that you render it to the shadow root. If you do this `children` and props get passed down as normal and React won't see your content in the shadow root.

### Injecting React and ReactDOM

By default, the React integration will look for `React` and `ReactDOM` on the window. However, this isn't the case for all apps. If you're using ES2015 modules or CommonJS, you'll have to inject them into the reactify function as options:

```js
import reactify from 'skatejs-react-integration';
import React from 'react';
import ReactDOM from 'react-dom';

export default reactify(..., {
  React,
  ReactDOM
});
```

### Multiple React versions

The integration sets a peer-dependency on React so you know what it's compatible with. That said, you still need to be mindful that the version of React you provide to the integration layer is correct.
