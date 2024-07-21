# React Effects (Basic)

In this project, I created an application where I user is presented with some distinations for vacations sorted by the distance, and the user can then choose and remove the places it likes

The idea behind this application is to understand what are side-effects, and how to handle them gracefully. While doing so, what are dependencies, and how they impact the re-renders of the components. 


## Table of Contents

1. [What's a "Side Effect"?](#whats-a-side-effect)
2. [A Potential Problem with Side Effects: An Infinite Loop](#a-potential-problem-with-side-effects-an-infinite-loop)
3. [Using useEffect for Handling (Some) Side Effects](#using-useeffect-for-handling-some-side-effects)
4. [Not All Side Effects Need useEffect](#not-all-side-effects-need-useeffect)
5. [Understanding Effect Dependencies](#understanding-effect-dependencies)
6. [useEffect's Cleanup Function](#useeffects-cleanup-function)
7. [The Problem with Object & Function Dependencies](#the-problem-with-object--function-dependencies)
8. [The useCallback Hook](#the-usecallback-hook)
9. [Optimizing State Updates](#optimizing-state-updates)

### What's a "Side Effect"?

A side effect in React is any operation that affects something outside the scope of a function being executed. This could be:

- Fetching data from an API
- Modifying the DOM directly
- Setting up a subscription or timer

#### Example:
```javascript
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]);
```
In this example, updating the document title is a side effect of clicking a button.

### A Potential Problem with Side Effects: An Infinite Loop

Side effects can cause issues if not managed correctly, one common problem being infinite loops. This happens when an effect updates a state that is also a dependency of the effect, causing it to re-run continuously.

#### Example:
```javascript
useEffect(() => {
  setCount(count + 1);
}, [count]);
```
This code will cause an infinite loop because setCount updates count, which is a dependency, causing useEffect to re-run indefinitely.

### Using useEffect for Handling (Some) Side Effects

useEffect is a hook that allows you to perform side effects in function components. It's a powerful tool for tasks like data fetching, subscriptions, and manually changing the DOM.

#### Example:
```javascript
useEffect(() => {
  const fetchData = async () => {
    const result = await axios.get('/api/data');
    setData(result.data);
  };
  fetchData();
}, []);
```
In this example, useEffect is used to fetch data from an API when the component mounts.

### Not All Side Effects Need useEffect
Not all side effects require the use of useEffect. For example, certain side effects can be handled directly within event handlers.

#### Example:
```javascript
const handleClick = () => {
  console.log('Button clicked');
};
```
Here, logging a message to the console on button click is a side effect but does not need useEffect.

### Understanding Effect Dependencies
Dependencies in useEffect determine when the effect should re-run. The array of dependencies tells React to only re-run the effect if one of the dependencies has changed.

#### Example:
```javascript
useEffect(() => {
  // Effect logic here
}, [prop1, prop2]);
```
Best practice involves carefully specifying dependencies to avoid unnecessary re-renders or missing updates.

### useEffect's Cleanup Function
The cleanup function in useEffect is used to clean up side effects like subscriptions or timers to prevent memory leaks.

#### Example:
```javascript
useEffect(() => {
  const timer = setTimeout(() => {
    console.log('Timer executed');
  }, 1000);

  return () => {
    clearTimeout(timer);
  };
}, []);
```
In this example, the cleanup function clears the timeout when the component unmounts.


### The Problem with Object & Function Dependencies
Dependencies that are objects or functions can cause problems because they may change on every render due to reference inequality.

#### Example:
```javascript
useEffect(() => {
  // Effect logic here
}, [obj, func]);
```
To avoid unnecessary re-renders, use useMemo or useCallback to memoize objects and functions.

### The useCallback Hook
useCallback is a hook that returns a memoized version of the callback function that only changes if one of the dependencies has changed. It helps in preventing unnecessary re-renders.

#### Why We Need It:
Prevents functions from being recreated on every render.
Optimizes performance, especially when passing callbacks to child components.

#### Example:
```javascript
Copy code
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```
Here, memoizedCallback will only change if a or b changes, preventing unnecessary re-renders.

### Optimizing State Updates
Optimizing state updates in React involves strategies to ensure that state changes trigger the least number of re-renders necessary.

#### Example:
```javascript
const handleIncrement = () => {
  setCount(prevCount => prevCount + 1);
};
```
Using the functional form of setState ensures that state updates are based on the most recent state.


