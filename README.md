

## Error Tracking System:

### Tools and Libraries

For error tracking, we recommend integrating one of the following third-party tools or libraries:

- **Sentry**: Sentry is a popular error tracking and monitoring tool that provides real-time insights into application errors. It supports various platforms and programming languages, including JavaScript/React.

- **Rollbar**: Rollbar is another widely used error tracking solution that offers real-time error monitoring, alerting, and debugging features. It also supports JavaScript/React applications.

### Sentry is a powerful tool for error logging and monitoring. It includes features like:

- **Real-time error tracking**: Sentry provides real-time visibility into errors as they occur, enabling quick response and resolution.

- **Contextual information**: Sentry captures additional contextual information about errors, such as user data, environment details, and more.

- **Integrations**: It integrates with various platforms, frameworks, and languages, making it versatile for different tech stacks.

### Install

Sentry captures data by using an SDK within your application’s runtime.

```bash
npm install --save @sentry/react
```

![Screenshot 2024-07-03 154405](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/2e79ebb4-cb90-41f7-a2f2-a62800bd3c3b)


### Configure

Configuration should happen as early as possible in your application's lifecycle.

Sentry supports multiple versions of React Router.

`main.tsx`

```tsx

import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'
import { ThemeProvider } from "./context/theme.tsx";
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "https://d02d2189612dabae4c431ca386c77aa4@o4507526553075712.ingest.de.sentry.io/4507536849305680",
  integrations: [
    Sentry.browserTracingIntegration(),
    Sentry.replayIntegration(),
  ],
  // Performance Monitoring
  tracesSampleRate: 1.0, //  Capture 100% of the transactions
  // Set 'tracePropagationTargets' to control for which URLs distributed tracing should be enabled
  tracePropagationTargets: ["localhost", /^https:\/\/yourserver\.io\/api/],
  // Session Replay
  replaysSessionSampleRate: 0.1, // This sets the sample rate at 10%. You may want to change it to 100% while in development and then sample at a lower rate in production.
  replaysOnErrorSampleRate: 1.0, // If you're not already sampling the entire session, change the sample rate to 100% when sampling sessions where errors occur.
});


ReactDOM.createRoot(document.getElementById('root')!).render(
  <ThemeProvider>
    <App />
  </ThemeProvider>,
)
```

Once this is done, all unhandled exceptions are automatically captured by Sentry.

### Add Error Boundary

If you're using React 16 or above, you can use the Error Boundary component to automatically send Javascript errors from inside a component tree to Sentry, and set a fallback UI

### Testing

After integrating the error tracking system, thoroughly test the application to ensure that errors are being captured and logged correctly. Generate test errors by deliberately triggering exceptions or encountering edge cases within the application. Verify that alerts/notifications are triggered as expected for critical errors.

![Screenshot 2024-07-02 134340](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/6b85265d-87a1-44f6-b9cf-120f39be487c)

![Screenshot 2024-07-02 134306](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/1fed8ea3-4e62-40a5-b840-564201f47585)

![Screenshot 2024-07-02 140306](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/c551aae3-3329-4d58-8ae5-2d4c11428e67)


![Screenshot 2024-07-02 134234](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/f1abc144-82c8-4290-910a-2b5b7ac10613)



### Add Readable Stack Traces to Errors

#### Automatic Setup

The easiest way to configure uploading source maps with tsc and sentry-cli is by using the Sentry Wizard:

`npx @sentry/wizard@latest -i sourcemaps`

The wizard will guide you through the following steps:

- Logging into Sentry and selecting a project

- Installing the necessary Sentry packages

- Configuring your build tool to generate and upload source maps

- Configuring your CI to upload source maps

```tsx
import { defineConfig } from "vite";
import { sentryVitePlugin } from "@sentry/vite-plugin";

export default defineConfig({
  build: {
    sourcemap: true, // Source map generation must be turned on
  },
  plugins: [
    // Put the Sentry vite plugin after all other plugins
    sentryVitePlugin({
      authToken: process.env.SENTRY_AUTH_TOKEN,
      org: "arunava",
      project: "wd301",
    }),
  ],
});
```
![Screenshot 2024-07-02 140112](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/daf4e850-bd9b-4b69-8376-a2239d80bdf2)
![Screenshot 2024-07-02 140044](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/a5a06c6e-a811-407a-9534-5835982aad9b)
![Screenshot 2024-07-02 135650](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/626fc032-95da-4a83-8967-52602358acf2)
![Screenshot 2024-07-02 135818](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/dcd046b6-04da-4b7f-b6b0-1bace547ef15)



## Debugger Capability:

### Introduction

Debugging is a crucial skill for developers. Various approaches and tools are available to help identify and resolve issues in the code we wrote.

- **Console Logging**: We can use `console.log()` statements to output specific values, messages, or variables to the browser console. While this may be simple and easy to implement, it provides limited visibility into the state of the application at a specific moment. Also, it may clutter the codebase if not removed after debugging.

Eg:

```tsx
 const token = localStorage.getItem("authToken")
  useEffect(() => {
    fetch('https://wd301-capstone-api.pupilfirst.school/articles', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`Failed to fetch data: ${response.status} ${response.statusText}`);
        }
        return response.json();
      })
      .then(data => {
        setFetchedData(data);
      })
      .catch(error => {
        console.error(error.message);
      });
  }, [token]);

  // Now you can access 'fetchedData' outside the promise chain

console.log("emp",fetchedData);

debugger; //Debugger Statement for debugging

```

As for the Root Cause Analysis (RCA) for the bug found using `console.log` statements such that the bug is displayed in the console:

![Screenshot 2024-07-02 141426](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/c7b4a410-3756-431e-b767-ed7cab6ffb69)


The `console.log` statements are used for debugging as what data is passed where it is showing an error and when the data is showing as None or Undefined.

- **Breakpoints**: We can set breakpoints in the code using the browser's developer tools. The execution pauses at these breakpoints, allowing inspection of variables and the call stack. This provides a more interactive and dynamic debugging experience. It also allows step-by-step execution to understand the flow of the program.

- **Debugger Statements**: We can insert debugger; statements directly into the code. When the code is executed, it pauses at the debugger; line, allowing for inspection. This is quick and easy to implement. It offers a breakpoint-like experience without relying on the developer tools. But it needs to be removed after debugging, else it can be intrusive if left in the production code.

Here for breakpoints or debugger we have placed a `debugger` statement in our application code for Navbar.tsx every time we proceed to this page at the debugger point the application stops and then runs one by one so has to see where the code is getting error.

`Navbar.tsx`

```tsx
 const token = localStorage.getItem("authToken")
  useEffect(() => {
    fetch('https://wd301-capstone-api.pupilfirst.school/articles', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`Failed to fetch data: ${response.status} ${response.statusText}`);
        }
        return response.json();
      })
      .then(data => {
        setFetchedData(data);
      })
      .catch(error => {
        console.error(error.message);
      });
  }, [token]);

  // Now you can access 'fetchedData' outside the promise chain

console.log("emp",fetchedData);

debugger; //Debugger Statement for debugging

```

![Screenshot 2024-07-02 142027](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/664959d2-43c8-42e3-8af4-e4944cc06a5a)


- **Browser Developer Tools**: These are a comprehensive suite of tools provided by browsers for inspecting and debugging web applications. It includes the Elements panel, Console, Sources, Network, etc. It allows real-time inspection of the DOM, CSS, and JavaScript. But it has an initial learning curve.

![Screenshot 2024-07-02 142027](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/ac175ed8-186f-47c6-97da-5a1d5eb75a0a)
![Screenshot 2024-07-02 142258](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/c25943da-17cc-42e3-ab75-eaa725ca4ebb)
![Screenshot 2024-07-02 142348](https://github.com/i0am0arunava/wd401-l-9/assets/141677292/976387df-fd73-4c91-a902-5a16c2136f7f)


As for the Root Cause Analysis (RCA) for the bug found using React Developer Tools:

> **Bug Description**: The bug is caused due to the fetching of the projects list created.

> **Symptoms**: Every time the project is clicked it is showing as something went wrong.

> **Root Cause**: May be it is due to the backend delay or fetching of the projects before its creation.

> **Resolution**: Fetching the projects only after its creation can be the solution.

> **Impact Analysis**: It may not have much impact now but in future has we add features it may be a problem.
