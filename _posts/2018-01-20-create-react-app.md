---
layout: post
title: "How I'm starting my React apps now"
date: 2018-01-20 12:28
comments: true
tags:
- create-react-app
- react
---

tl;dr, clone https://github.com/saebyn/my-react-app

## Introduction

For this setup, I have node v6.9.1 installed. This uses create-react-app to create the base project, and then walks through various things I usually want for a React project, like my preferred editor configuration (for VS Code) for the project, Wallaby.JS, Prettier, and Storybook.

```bash
npx create-react-app my-react-app
cd my-react-app
npm i
```

## Validate that the app starts

```bash
npm start
open http://localhost:3000 # the app should do this for you
```

You should see a "Welcome to React" message, logo, etc in your web browser.

## Configure Wallaby.js

Grab the [Wallaby.js configuration file](https://wallabyjs.com/docs/integration/react-jsx.html) and save it as "wallaby.js" at the top level of the project.


## Editor configuration

1. At the top level of the project, add the following to a ".editorconfig" file:

    ```ini
    [*.js,*.json]
    indent_style = space
    indent_size = 2
    ```


2. Add extensions to the extensions.json for the workspace (e.g. in .vscode/extensions.json)

    ```json
    {
      // See http://go.microsoft.com/fwlink/?LinkId=827846
      // for the documentation about the extensions.json format
      "recommendations": [
        // Extension identifier format: ${publisher}.${name}. Example: vscode.csharp
        "EditorConfig.editorconfig",
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode"
      ]
    }
    ```

3. Then go install those if you don't already have them.

4. Make vscode apply formatting for this workspace, add this to .vscode/settings.json

    ```json
    {
      "editor.formatOnSave": true,
      "editor.formatOnType": true
    }
    ```

## Setup prettier

Install prettier and an eslint configuration:

```bash
npm i --save-dev --save-exact prettier
npm i --save-dev eslint-config-prettier
```

Add a .eslintrc.json file to the top level:

```json
{
  "extends": ["prettier", "react-app"]
}
```

## Setup [storybook](https://storybook.js.org/basics/guide-react/)

1. Install it

    ```bash
    npm i --save-dev @storybook/react
    ```


2. Add an entry to package.json scripts:

    ```json
    {
      "scripts": {
        ...,
        "storybook": "start-storybook -p 9001 -c .storybook"
      }
    }
    ```

3. Set up an initial config in .storybook/config.js

    ```js
    import { configure } from '@storybook/react';
    function loadStories() {
        require('../stories/index.js');
        // You can require as many stories as you need.
    }
    configure(loadStories, module);
    ```

4. Install Storybook add-ons

    ```bash
    npm install --save-dev @storybook/addon-storyshots @storybook/addon-actions  @storybook/addon-a11y @storybook/addon-links @storybook/addon-graphql @storybook/addon-knobs @storybook/addon-notes react-test-renderer
    ```

5. Create .storybook/addons.js

    ```js
    import "@storybook/addon-actions/register";
    import "@storybook/addon-a11y/register";
    import "@storybook/addon-links/register";
    import "@storybook/addon-knobs/register";
    import "@storybook/addon-notes/register";
    ```

6. Setup storyshots for Jest

    See https://github.com/storybooks/storybook/tree/master/addons/storyshots#configure-storyshots-for-html-snapshots for more about Storyshots and see https://facebook.github.io/jest/docs/en/snapshot-testing.html for more info about Jest's snapshot testing

    Create a new test file with the name Storyshots.test.js (I'm putting it in src/)

    ```js
    import initStoryshots from '@storybook/addon-storyshots';
    initStoryshots({
        storyNameRegex: /^((?!.*?DontTest).)*$/
    });
    ```

7.  Add some stories just to validate that Storybook is working

    Add this to stories/index.js:

    ```js
    import React from "react";
    import { storiesOf } from "@storybook/react";
    import { action } from "@storybook/addon-actions";
    import { checkA11y } from "@storybook/addon-a11y";
    import { linkTo } from "@storybook/addon-links";
    import { setupGraphiQL } from "@storybook/addon-graphql";
    import { withKnobs, text, boolean, number } from "@storybook/addon-knobs/react";
    import { withNotes } from "@storybook/addon-notes";
    // setup the graphiql helper which can be used with the add method later
    const graphiql = setupGraphiQL({ url: "http://localhost:3100/graphql" });
    storiesOf("button", module)
        .addDecorator(checkA11y)
        .addDecorator(withKnobs)
        .add(
            "with text",
            withNotes(`
    Clicking this button will take you to the next story.
    `)(() => (
        <button onClick={linkTo("button", "with some emoji")}>
            Hello Button
                </button>
            ))
        )
        .add("with some emoji", () => (
            <button onClick={action("clicked")}>😀 😎 👍 💯</button>
        ))
        .add("Inaccessible", () => (
            <button style={{ backgroundColor: "red", color: "darkRed" }}>
                Inaccessible button
            </button>
        ))
        .add(
            "get user info DontTest",
            graphiql(`{
                user(id: "1") {
                    name
                }
            }`)
        )
        .add("with a button", () => (
            <button disabled={boolean("Disabled", false)}>
                {text("Label", "Hello Button")}
            </button>
        ))
        .add("as dynamic variables", () => {
            const name = text("Name", "Arunoda Susiripala");
            const age = number("Age", 89);
            const content = `I am ${name} and I'm ${age} years old.`;
            return <div>{content}</div>;
        });
    ```

## Testing the tests

1. Run the tests
    ```bash
    npm test
    ```

2. If you get an error about "jest-cli", try deleting node_modules and running "npm install" again. I had to do this.

3. After it runs, you'll see a new file: `src/__snapshots__/Storyshots.test.js.snap`

4. Start Storybook

    ```bash
    npm run storybook
    open http://localhost:9001  # It will not open this automatically (but it will reload when stories are changed.)
    ```


Check out https://github.com/saebyn/my-react-app/tree/339d213dc2b28c3714e8a1836dddd83cde4f761c for the end result, or try it yourself.
