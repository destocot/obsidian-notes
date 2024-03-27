- categories for our components can be created using the title field of a story
	- in this example our Button component is under the Category Example
	- this is an **optional** field, if we remove the title, then Storybook will auto title the categories based on the file system
- documentation for a component can be automatically created with the 'autodocs' tag.
- the stories themselves map to the **named exports** inside of our module
```js
// Button.stories.js
export default {
  title: "Example/Button",
  component: Button,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: "centered",
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ["autodocs"],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    backgroundColor: { control: "color" },
  },
};

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Primary = {
  args: {
    primary: true,
    label: "Fun times with Storybook",
  },
};

export const Secondary = {
  args: {
    label: "Button",
  },
};
```

#### interaction testing
```js
import { Page } from "./Page";
import { userEvent, expect, within } from "@storybook/test";

export default {
  component: Page,
};

export const LoggedOut = {};

// More on interaction testing: https://storybook.js.org/docs/writing-tests/interaction-testing
export const LoggedIn = {
  play: async (context) => {
    const canvas = within(context.canvasElement);
    const loginButton = canvas.getByRole("button", {
      name: /Log in/i,
    });
    await userEvent.click(loginButton);

    const logoutButton = canvas.getByRole("button", {
      name: /Log out/i,
    });
    await expect(logoutButton).toBeInTheDocument();
  },
};

```

==example== Button Story
```js
import { Button } from "./Button.jsx";

export default {
  component: Button,
  argTypes: {
    onClick: { action: "onClick fired" },
  },
  // parameters: {
  //   layout: "fullscreen",
  // },
};

export const MyStory = {
  args: {
    label: "Button",
  },
  parameters: {
    layout: "centered",
  },
};

export const IAmYourFather = {
  args: {
    label: "NOOOO!",
    primary: true,
    backgroundColor: "red",
    size: "large",
  },
};
```

**addons**
> https://storybook.js.org/docs/configure/storybook-addons

