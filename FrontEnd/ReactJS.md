# ASCENDING React/JSX Style Guide

This style guide is mostly based on [Airbnb](https://github.com/airbnb/javascript/blob/master/react/README.md) react coding style. We kept key points as part of our standard.

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Project Scaffold](#project-scaffold)
  1. [Dependency Management](#dependency-management)
  1. [Naming](#naming)
  1. [Variables](#variables)
  1. [Environments](#environments)
  1. [Branching Strategy](#branching-strategy)
  1. [Analytics](#analytics)
  1. [Project ReadMe](#project-readme)

## Basic Rules

- Copy&Paste with your mind, never just copy paste, make sure change the name and content according to each project. Remove unecessary code.
- Always use JSX syntax.
- Fix all the console error at the end of PR

## Project Scaffold
- We made a [scaffold project](https://github.com/ascending-llc/create-event-app) to bootstrap a standard reactjs project according to ASCENDING standard. Please use that to inialize a project, provide feedback for improvement.
- Use scss to orgnize all the styling work. Bootstrap is our preference on ADM project. In the event of using material UI, require approval from team. Component scss has to be define along with the *component*.jsx file
project
```
├── src  
│   ├── pages  
│   │   ├── Career  Components
│   │       ├── index.jsx  *jsx file*
            ├── index.scss *styling file*
            ├── data.jsx *static data file*
```

## Dependency Management
- We should use **package.json** to manage all runtime/dev dependency and build target
- **Runtime dependency** which is essential to run the front-end application could be defined in `dependencies` node
- **Dev dependency** which is only required to build the front-end application could be defined in `devDependencies` node

 ```json
    //good
    "dependencies": {
        "react": "YOUR VERSION",
        ...
    },
    "devDependencies": {
        "env-cmd": "YOUR VERSION",
        ...
    }

    //working but bad
    "dependencies": {
        "react": "YOUR VERSION",
        "env-cmd": "YOUR VERSION"
    }
 ```

## Naming

 - **Extensions**: Use `.jsx` extension for React components. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

  - **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

    > Why? A component’s `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

    ```jsx
    // bad
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // bad
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

## Variables

- **No Hardcoding**: It's universal rule in ASCENDING that doesn't allow hard code. 
    ```jsx
    // bad
    region: "us-east-1",

    // good
    region: process.env.REGION,

    // good
    let region = "us-east-1"
    ...
    region: region,
    ```
- **environment variable** setup environment variable .env.{environment}
- **component variable** use react `useState`
- **static page data** sometime we have static front-end data that is not fetched from API, we define them in the data.jsx

## Environments
We normally setup four environment in each application, depending on the complexity we may add or remove environment
- **local** *feature/{jira_ticket}* branch for local development
- **qa** *test* branch for testing purpose pretty much similar to local environment
- **staging** *staging* branch all features will be staged in the staging environment for user acceptance testing(UAT)
- **prod** *master* branch production environment


## Branching Strategy
- **feature** for any tickets or feature development, please create branch **feature/{jira_ticket}** off *test* branch and make PR for review when it is ready
- **pull request** pull request please request one of the developer in your team to run peer review prior to merge.
- **hotfix** please create branch **hotfix/{jira_ticket}** off *staging* branch and make PR for review when it is ready. Please make sure merge back to *test* at the end of life cycle.

## Analytics