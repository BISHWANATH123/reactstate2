# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh








Clear and concise explanations for the above code behaviour in  own words. using markdown file......

Code 1: Delayed State Update

import React from 'react';

function App() {
  const [count, setCount] = React.useState(0);

  const handleClick = () => {
    setCount(count + 1);
    console.log(count); // You will see the older value of count in the console
  };

  return (
    <div>
      <p>Button is clicked {count} times</p>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;




1.useState is asynchronous:

setCount is an asynchronous function, meaning it doesn't immediately update the state. When you call setCount(count + 1), React schedules an update, but the state variable count is not immediately updated.

2.Closure in Event Handlers:

The console.log(count) statement is executed immediately after setCount(count + 1). At this point, count still holds the older value because the state update is not applied yet.

3.State updates are batched:

React batches state updates for performance reasons. Even though setCount is called three times in quick succession, the updates are grouped into a single batch.

4.Solution:

To see the updated count in the console, you should use the callback form of setCount to access the latest state value:
const handleClick = () => {
  setCount((prevCount) => {
    console.log(prevCount);
    return prevCount + 1;
  });
};



Code 2: Multiple State Updates


import React from 'react';

function App() {
  const [count, setCount] = React.useState(0);

  const handleClick = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    console.log(count);
  };

  return (
    <div>
      <p>Button is clicked {count} times</p>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;


1.State updates are asynchronous:

Similar to Code 1, state updates using setCount are asynchronous. When you call it three times in a row, React schedules three updates, but they don't happen immediately.

2.Batching of state updates:

React batches state updates, and in this case, the three consecutive setCount calls are grouped into a single batch.

3.Console.log immediately after state updates:

The console.log(count) statement is executed immediately after the three state updates, but at this point, the state variable count still holds the older value due to the asynchronous nature of state updates.

4.Solution:

To see the updated count, you should use the callback form of setCount or use the updated value directly in the next setCount call:

const handleClick = () => {
  setCount(count + 1);
  setCount(count + 2);
  setCount(count + 3);
  console.log(count);
};

