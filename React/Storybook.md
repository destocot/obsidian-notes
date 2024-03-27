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
import type { Meta, StoryObj } from "@storybook/react";
import { within, userEvent, expect } from "@storybook/test";

import Form from "./Form";

const meta: Meta<typeof Form> = {
  component: Form,
  title: "Form",
};

export default meta;

type Story = StoryObj<typeof meta>;

export const Base: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const submitBtn = canvas.getByRole("button", { name: /Post question/i });

    const email = canvas.getByLabelText(/Email/i);
    const question = canvas.getByLabelText(/Question/i);

    await expect(submitBtn).toBeInTheDocument();
    await expect(email).toBeInTheDocument();
    await expect(question).toBeInTheDocument();
  },
};

export const EmptySubmit: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const submitBtn = canvas.getByRole("button", { name: /Post question/i });

    await userEvent.click(submitBtn);

    await expect(
      canvas.getByText(/Please enter your email/i)
    ).toBeInTheDocument();
    await expect(
      canvas.getByText(/Please enter a question/i)
    ).toBeInTheDocument();
  },
};

export const InvalidEmail: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const submitBtn = canvas.getByRole("button", { name: /Post question/i });
    const email = canvas.getByLabelText(/Email/i);

    await userEvent.type(email, "invalid-email");
    await userEvent.click(submitBtn);

    await expect(
      canvas.getByText(/PLease provide a valid email/i)
    ).toBeInTheDocument();
    await expect(
      canvas.getByText(/Please enter a question/i)
    ).toBeInTheDocument();
  },
};

export const ValidInputs: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const submitBtn = canvas.getByRole("button", { name: /Post question/i });
    const email = canvas.getByLabelText(/Email/i);
    const question = canvas.getByLabelText(/Question/i);

    await userEvent.type(email, "test@email.com");
    await userEvent.type(question, "What is the meaning of life?");
    await userEvent.click(submitBtn);

    await expect(
      canvas.queryByText(/Please enter your email/i)
    ).not.toBeInTheDocument();
    await expect(
      canvas.queryByText(/PLease provide a valid email/i)
    ).not.toBeInTheDocument();
    await expect(
      canvas.queryByText(/Please enter a question/i)
    ).not.toBeInTheDocument();

    await expect(
      canvas.getByText(/Thanks for your submission!/i)
    ).toBeInTheDocument();
  },
};
```

==example== Button Story
```tsx
import type { Meta, StoryObj } from "@storybook/react";

import Light from "./Light";

const meta: Meta<typeof Light> = {
  title: "Example/Light",
  component: Light,
  argTypes: {
    variant: {
      control: {
        type: "select",
        options: ["green", "yellow", "red"],
      },
    },
  },
  tags: ["autodocs"],
  // parameters: {
  //   layout: "fullscreen",
  // },
};

export default meta;

type Story = StoryObj<typeof Light>;

/** this is how it looks by default */
export const Base: Story = {
  args: {
    variant: "green",
  },
  parameters: {
    layout: "centered",
  },
};

/** whatever!! */
export const Yellow: Story = {
  args: {
    variant: "yellow",
  },
};

export const Red: Story = {
  args: {
    variant: "red",
  },
};

export const Grouped: Story = {
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  render: (args) => (
    <div
      style={{
        background: "gray",
        display: "flex",
        flexDirection: "column",
        gap: 10,
        border: "1px solid black",
        width: "max-content",
        padding: 10,
      }}
    >
      <Light variant="green" />
      <Light variant="yellow" />
      <Light variant="red" />
    </div>
  ),
};
```

**addons**
> https://storybook.js.org/docs/configure/storybook-addons

