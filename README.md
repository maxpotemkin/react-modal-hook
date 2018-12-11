# react-modal-hook

[![NPM](https://img.shields.io/npm/v/react-modal-hook.svg)](https://www.npmjs.com/package/react-modal-hook)

Syntactic sugar for handling modal windows using React Hooks.

[**Demo** (Material-UI)](https://codesandbox.io/s/v8qy4w1j77)

## Table of Contents

- [Install](#install)
- [Usage](#usage)
  - [Updating Modals](#updating-modals)
  - [Animated Modals](#animated-modals)
- [License](#license)

## Install

```bash
npm install --save react-modal-hook
```

## Usage

Use `ModalProvider` to provide modal context for your application:

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { ModalProvider } from "react-modal-hook";
import App from "./App";

ReactDOM.render(
  <ModalProvider>
    <App />
  </ModalProvider>,
  document.getElementById("root")
);
```

Call `useModal` to receive modal handles in your functional component:

```jsx
import React from "react";
import { useModal } from "react-modal-hook";

const App = () => {
  const [showModal, hideModal] = useModal(() => (
    <div role="dialog" className="modal">
      <p>Modal content</p>
      <button onClick={hideModal}>Hide modal</button>
    </div>
  ));

  return <button onClick={showModal}>Show modal</button>;
};
```

### Updating Modals

Second argument to `useModals` should contain an array of values referenced inside the modal:

```jsx
const App = () => {
  const [count, setCount] = useState(0);
  const [showModal] = useModal(
    () => (
      <div role="dialog" className="modal">
        <span>The count is {count}</span>
        <button onClick={() => setCount(count + 1)}>Increment</button>
      </div>
    ),
    [count]
  );

  return <button onClick={showModal}>Show modal</button>;
};
```

Modals are also functional components and can use react hooks themselves:

```jsx
const App = () => {
  const [showModal] = useModal(() => {
    const [count, setCount] = useState(0);

    return (
      <div role="dialog" className="modal">
        <span>The count is {count}</span>
        <button onClick={() => setCount(count + 1)}>Increment</button>
      </div>
    );
  });

  return <button onClick={showModal}>Show modal</button>;
};
```

### Animated Modals

Use [`TransitionGroup`](https://github.com/reactjs/react-transition-group) as the container for the modals:

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { ModalProvider } from "react-modal-hook";
import { TransitionGroup } from "react-transition-group";
import App from "./App";

ReactDOM.render(
  <ModalProvider container={TransitionGroup}>
    <App />
  </ModalProvider>,
  document.getElementById("root")
);
```

When `TransitionGroup` detects of one of its children removed, it will set its `in` prop to false and wait for `onExited` callback to be called before removing it from the DOM.

Those props are automatically added to all components passed to `useModal`. You can can pass them down to [`CSSTransition`](http://reactcommunity.org/react-transition-group/css-transition/) or modal component with transition support.

Here's an example using Material-UI's [`Dialog`](https://material-ui.com/demos/dialogs/):

```jsx
import React from "react";
import { useModal } from "react-modal-hook";
import { Button, Dialog, DialogActions, DialogTitle } from "@material-ui/core";

const App = () => {
  const [showModal, hideModal] = useModal(({ in: open, onExited }) => (
    <Dialog open={open} onExited={onExited}>
      <DialogTitle>Dialog Content</DialogTitle>
      <DialogActions>
        <Button onClick={hideModal}>Close</Button>
      </DialogActions>
    </Dialog>
  ));

  return <Button onClick={showModal}>Show modal</Button>;
};
```

## License

MIT © [mpontus](https://github.com/mpontus)
