## JavaScript Concepts

### Closure
A **closure** is a function defined inside another function that:
- Remembers its outer variables
- Can access those variables even after the outer function has finished execution

### Callback Hell
Also known as the **Pyramid of Doom**  
Occurs when callbacks are nested deeply, making code:
- Hard to read
- Hard to maintain

---

## VS Code Productivity

### Duplicate a Line
- **Windows**: `Shift + Alt + Down` (or `Up`)
- **macOS**: `Shift + Option + Down` (or `Up`)

### Emoji Keyboard Shortcut (Windows / VS Code)
- `Windows + .` (period)
- `Windows + ;` (semicolon)

---

## Practice Resources

### Frontend Practice
- https://www.frontendmentor.io/challenges
- https://www.youtube.com/@WebDevSimplified

---

## Fullstack JavaScript

### JavaScript – Part 4
- https://www.youtube.com/watch?v=8aGhZQkoFbQ
- https://codesandbox.io/s/js4-or14nn?file=/src/index.js:18-727

Read about: callback hell / pyramid of doom

### JavaScript – Part 5
- https://codesandbox.io/s/js-5-799nq4?file=/src/index.js

### Async / Await
- An `async` function **always returns a Promise**
- `await` can only be used **inside an async function**

### Currying
**Currying** is useful when:
- All parameters are not available at once
- You want to pass parameters one at a time

---

## React - 1

### React Overview
- React is a **Library**
- React framework example: https://nextjs.org/

### JavaScript to Know for React
- https://kentcdodds.com/blog/javascript-to-know-for-react

### Style Guides & Blogs
- https://airbnb.io/javascript/react/
- https://blog.isquaredsoftware.com/

---

## React Tools & Playgrounds
- React Storybook: https://www.youtube.com/watch?v=FUKpWgRyPlU
- React Compiler Playground: https://playground.react.dev/
- React Code Sandbox: https://react.new

---

## Recommended Extensions
- VS Code Extension: **Prettier**
  - Settings → Enable **Format on Save**
- Google Chrome Extension: **React Developer Tools**

---

## React Project Setup

### Create React App using Vite
```bash
npm create vite@latest react-project -- --template react
```

### Dependencies
```json
"dependencies": {
  "react": "^19.1.0",
  "react-dom": "^19.1.0"
}
```

### Install React Release Candidate
```bash
npm install --save-exact react@rc react-dom@rc
npm install
npm run dev
```

---

### Traditional CRA Setup
```bash
cd my-app
npm start
```

http://localhost:3000

---

## JSX & Compilation
- JSX = JavaScript and XML
- Holds dynamic values
- Babel converts JSX syntax to JavaScript

---

### React Components
- Component is the building block of a React App
- `App.jsx` is the first/root component

### Virtual DOM
- The `key` property helps to differentiate between two snapshots of the Virtual DOM

---

2:21:45
