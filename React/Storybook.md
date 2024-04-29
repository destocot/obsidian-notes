# FEM Storybook

1. [[#1. Getting Started]]
	1. Setting Up Storybook, Storybook Sandboxes, How to Write a Story, Adding Variants to a Story, Adding Controls to a Story, Adding Button Sizes, Setting Default Arguments
2. [[#2. Styling]]
	1. Importing Styles, Using Tailwind, and Using Themes, Customizing Tailwind Colors, Importing Fonts from Fontsource
3. [[#3. Documenting Components]]
	1. Using Storybook with MDX,Improving `argTypes` with Metadata.,Color Palette, Icon Gallery, Type Set, Organizing Stories and Documentation
4. [[#4. Component Styling Techniques]]
	1. Class Variance Authority
	2. Adding a Size Variant with CVA
	3. Supporting Dark Mode
	4. Forcing Dark Mode
	5. An Alternative Approach to Dark Mode
	6. Implementing a Callout Component
5. [[#5. Testing and Interactions]]
	1. Play Functions
	2. Testing Components
	3. Setting Up a Test Runner
	4. Visual Test
	5. Accessibility Testing
6. [[#6. APIs, Context, and External Dependencies]]
	1. Using Decorators for Context
	2. Using Loaders to Fetch Data
7. Appendix
	1. Integration with Mock Service Worker
	2. Storybook in Figma
	3. Figma in Storybook

# 1. Getting Started

- Setting Up Storybook
```
npx storybook@latest init
```
- Storybook Sandboxes
```
npx storybook@latest sandbox
```
- How to Write a Story
```tsx
/* button.stories.tsx */
import type { Meta, StoryObj } from '@storybook/react';

import { Button } from './button';

type Story = StoryObj<typeof Button>;

const meta: Meta<typeof Button> = {
	title: 'Button',
	component: Button,
};

export default meta;

export const Primary: Story = {
	args: {
		children: 'Button',
	},
};
```
- Adding Variants to a Story
-  Adding Controls to a Story
- Adding Button Sizes
- Setting Default Arguments
```tsx
/* button.tsx */
import clsx from 'clsx';
import styles from './button.module.css';

type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: 'primary' | 'secondary' | 'destructive';
  size?: 'small' | 'medium' | 'large';
};

export const Button = ({
  variant = 'primary',
  size = 'medium',
  className,
  ...props
}: ButtonProps) => {
  let classes = clsx(
    styles.button,
    {
      [styles.secondary]: variant === 'secondary',
      [styles.destructive]: variant === 'destructive',
      [styles.small]: size === 'small',
      [styles.large]: size === 'large',
    },
    className,
  );

  return <button className={classes} {...props}></button>;
};
```

```tsx
/* button.stories.tsx */
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './button';

const meta = {
  title: 'Button',
  component: Button,
  args: { 
	  children: 'Button', 
	  disabled: false, 
	  variant: 'primary', 
	  size: 'medium' 
  },
  argTypes: {
    disabled: { control: 'boolean' },
    variant: { control: 'select' },
    size: { control: 'select' },
  },
} satisfies Meta;

export default meta;

type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: { variant: 'primary' },
};

export const Secondary: Story = {
  args: { variant: 'secondary' },
};

export const Destructive: Story = {
  args: { variant: 'destructive' },
};

export const Small: Story = {
  args: { size: 'small' },
};

export const Large: Story = {
  args: { size: 'large' },
};
```

# 2. Styling

- Importing Styles, Using Tailwind, and Using Themes
```
@storybook/addon-themes
```
- Customizing Tailwind Colors
```ts
/* src/tokens/colors.ts */
export const colors = {
  primary: {
    '50': '#faf7fd',
    '100': '#f2ecfb',
    '200': '#e7ddf7',
    '300': '#d5c2f0',
    '400': '#bc9be5',
    '500': '#9967d5',
    '600': '#8a55c8',
    '700': '#7642ae',
    '800': '#643a8f',
    '900': '#523073',
    '950': '#361952',
  },
  secondary: {
    '50': '#f0fdfa',
    '100': '#ccfbf0',
    '200': '#98f7e2',
    '300': '#5debd2',
    '400': '#2cd5bc',
    '500': '#13b9a3',
    '600': '#0c9586',
    '700': '#0e776d',
    '800': '#115e57',
    '900': '#134e49',
    '950': '#042f2d',
  },
  accent: {
    '50': '#fff7ed',
    '100': '#ffecd5',
    '200': '#fed6aa',
    '300': '#feb873',
    '400': '#fc903b',
    '500': '#fa7015',
    '600': '#eb550b',
    '700': '#c33f0b',
    '800': '#9b3211',
    '900': '#7c2c12',
    '950': '#431307',
  },
  information: {
    '50': '#f3f6fc',
    '100': '#e6eef8',
    '200': '#c7daf0',
    '300': '#95bbe4',
    '400': '#5d99d3',
    '500': '#387cbf',
    '600': '#2862a1',
    '700': '#214e83',
    '800': '#1f446d',
    '900': '#1f3a5b',
    '950': '#14253d',
  },
  success: {
    '50': '#f1fcf2',
    '100': '#defae4',
    '200': '#bff3ca',
    '300': '#8de8a1',
    '400': '#54d471',
    '500': '#2cb84c',
    '600': '#1f9a3b',
    '700': '#1c7932',
    '800': '#1b602b',
    '900': '#184f26',
    '950': '#082b11',
  },
  warning: {
    '50': '#fffde7',
    '100': '#fffbc1',
    '200': '#fff286',
    '300': '#ffe341',
    '400': '#ffcf0d',
    '500': '#ecb100',
    '600': '#d18b00',
    '700': '#a66202',
    '800': '#894c0a',
    '900': '#743e0f',
    '950': '#442004',
  },
  danger: {
    '50': '#fef3f2',
    '100': '#fee5e5',
    '200': '#fccfd1',
    '300': '#faa7ac',
    '400': '#f6767f',
    '500': '#ed4656',
    '600': '#d9253e',
    '700': '#b71933',
    '800': '#9a1731',
    '900': '#831831',
    '950': '#490815',
  },
  slate: {
    '50': '#f6f7f9',
    '100': '#eceef2',
    '200': '#d5d9e2',
    '300': '#b1bbc8',
    '400': '#8695aa',
    '500': '#64748b',
    '600': '#526077',
    '700': '#434e61',
    '800': '#3a4252',
    '900': '#343a46',
    '950': '#23272e',
  },
};

export const white = '#ffffff';
export const black = '#010209';
export const transparent = 'transparent';
export const currentColor = 'currentColor';
```
- Importing Fonts from Fontsource
```ts
/* .storybook/preview.ts */
import type { Preview } from '@storybook/react';
import { withThemeByDataAttribute } from '@storybook/addon-themes';
import '../src/index.css';

const preview: Preview = {
  parameters: {
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/i,
      },
    },
  },
  decorators: [
    withThemeByDataAttribute({
      defaultTheme: 'light',
      themes: {
        light: 'light',
        dark: 'dark',
      },
      attributeName: 'data-mode',
    }),
  ],
};

export default preview;
```

```ts
/* tailwind.config.ts */
import type { Config } from 'tailwindcss';
import { colors, white, black, currentColor, transparent } from './src/tokens/colors';

export default {
  content: ['./src/**/*.tsx', './src/**/*.ts', './src/**/*.mdx'],
  darkMode: ['class', "[data-mode='dark']"],
  theme: {
    colors: {
      ...colors,
      white,
      black,
      currentColor,
      transparent,
    },
    extend: {
      fontFamily: {
        sans: ['Inter Variable', 'sans-serif'],
      },
    },
  },
  plugins: [],
} satisfies Config;
```

```css
/* index.css */
@import '@fontsource-variable/inter';

@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply dark:bg-slate-950 dark:text-white;
}
```

```tsx
/* button.stories.tsx */

/* ... */

export const Dark: Story = {
  parameters: {
    themes: {
      themeOverride: 'dark',
    },
  },
};

export const Mobile: Story = {
  parameters: {
    viewport: {
      defaultViewport: 'mobile1',
    },
  },
};
```

# 3. Documenting Components
- Using Storybook with MDX
- Improving `argTypes` with Metadata.
- Color Palette
- Icon Gallery
- Type Set
- Organizing Stories and Documentation
```mdx
// button.mdx

import { Meta, Title, Primary, Stories, Controls } from '@storybook/blocks';
import ButtonStories from './button.stories';
import { Button } from './button';

<Title>Button</Title>

<Meta of={ButtonStories} />

Here is some notes on how to use the button.

<Primary />

<Controls />

<Stories />
```

```mdx
// colors.mdx

import { Meta, ColorPalette, ColorItem } from '@storybook/blocks';
import { colors } from './tokens/colors';

<Meta title="Tokens/Colors" />

<ColorPalette>
  {Object.entries(colors).map(([name, value]) => (
    <ColorItem key={name} title={name} colors={value} />
  ))}
</ColorPalette>
```

```tsx
// button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './button';

const meta = {
  title: 'Components/Button',
  component: Button,
  args: {
    children: 'Button',
    variant: 'primary',
    size: 'medium',
    disabled: false,
  },
  argTypes: {
    children: {
      name: 'Label',
      control: 'text',
      description: 'The text to display on the button',
      table: { disable: false },
    },
    variant: { control: 'select' },
    size: { control: 'select' },
    disabled: { control: 'boolean' },
  },
} satisfies Meta;

export default meta;
```
# 4. Component Styling Techniques
- Class Variance Authority
- Adding a Size Variant with CVA
- Supporting Dark Mode
- Forcing Dark Mode
- An Alternative Approach to Dark Mode
- Implementing a Callout Component
```ts
// button-variants.ts
import { cva, type VariantProps } from 'class-variance-authority';

export type ButtonVariants = VariantProps<typeof variants>;

export const variants = cva(
  [
    'font-semibold',
    'border',
    'rounded',
    'shadow-sm',
    'inline-flex',
    'items-center',
    'cursor-pointer',
    'gap-1.5',
    'focus-visible:outline',
    'focus-visible:outline-2',
    'focus-visible:outline-offset-2',
    'transition-colors',
    'disabled:opacity-50',
    'disabled:cursor-not-allowed',
    'disabled:pointer-events-none',
  ],
  {
    variants: {
      variant: {
        primary: [
          'bg-primary-600',
          'text-white',
          'border-transparent',
          'hover:bg-primary-500',
          'active:bg-primary-400',
        ],
        secondary: [
          'bg-white',
          'text-slate-900',
          'border-slate-300',
          'hover:bg-slate-50',
          'active:bg-slate-100',
        ],
        destructive: [
          'bg-danger-600',
          'text-white',
          'border-transparent',
          'hover:bg-danger-500',
          'active:bg-danger-400',
        ],
      },
      size: {
        small: ['px-2.5', 'py-1.5', 'text-xs'],
        medium: ['px-3', 'py-2', 'text-sm'],
        large: ['px-4', 'py-2.5', 'text-base'],
      },
    },
    defaultVariants: {
      variant: 'secondary',
      size: 'medium',
    },
  },
);
```

```tsx
// button.tsx
import { variants, type ButtonVariants } from './button-variants';

type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> &
  ButtonVariants & {
    variant?: 'primary' | 'secondary' | 'destructive';
    size?: 'small' | 'medium' | 'large';
  };

export const Button = ({
  variant = 'primary',
  size = 'medium',
  className,
  ...props
}: ButtonProps) => {
  return <button className={variants({ variant, size })} {...props}></button>;
};
```

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

## callout example
```ts
// callout-variants.ts
import { cva, type VariantProps } from 'class-variance-authority';

const variations = ['primary', 'information', 'success', 'danger', 'warning'] as const;
type Variation = (typeof variations)[number];

const variant = {
  primary: [
    'bg-primary-200',
    'text-primary-900',
    'border-primary-500',
    'dark:bg-primary-800',
    'dark:border-primary-900',
    'dark:text-primary-50',
  ],
  information: [
    'bg-information-200',
    'text-information-900',
    'border-information-500',
    'dark:bg-information-800',
    'dark:border-information-900',
    'dark:text-information-50',
  ],
  success: [
    'bg-success-200',
    'text-success-900',
    'border-success-500',
    'dark:bg-success-800',
    'dark:border-success-900',
    'dark:text-success-50',
  ],
  danger: [
    'bg-danger-200',
    'text-danger-900',
    'border-danger-500',
    'dark:bg-danger-800',
    'dark:border-danger-900',
    'dark:text-danger-50',
  ],
  warning: [
    'bg-warning-200',
    'text-warning-900',
    'border-warning-500',
    'dark:bg-warning-800',
    'dark:border-warning-900',
    'dark:text-warning-50',
  ],
};  

export const variants = cva(['p-4', 'rounded-lg', 'border', 'shadow-md', 'space-y-4'], {
  variants: { variant },
});

export type CalloutVariants = VariantProps<typeof variants>;

```

```tsx
// callout.tsx
import type { PropsWithChildren } from 'react';
import { variants, type CalloutVariants } from './callout-variants';

type CalloutProps = PropsWithChildren<CalloutVariants> & {
  title: string;
};

export const Callout = ({ variant, title, children }: CalloutProps) => {
  return (
    <div className={variants({ variant })}>
      <h2 className="font-semibold">{title}</h2>
      <p>{children}</p>
    </div>
  );
};
```

```tsx
// callout.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Callout } from './callout';

const meta = {
  title: 'Components/Callout',
  component: Callout,
  args: {
    title: 'An Important Title',
    children: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit.',
  },
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'success', 'danger', 'information', 'warning'],
    },
  },
} satisfies Meta;

export default meta;

type Story = StoryObj<typeof Callout>;

export const Primary: Story = {
  args: { variant: 'primary' },
};

export const Success: Story = {
  args: { variant: 'success' },
};

export const Danger: Story = {
  args: { variant: 'danger' },
};

export const Information: Story = {
  args: { variant: 'information' },
};

export const Warning: Story = {
  args: { variant: 'warning' },
};

```

# 5. Testing and Interactions
- Play Functions
- Testing Components
- Setting Up a Test Runner
- Visual Test
- Accessibility Testing
```ts
// get-length.s
import { ComponentProps } from 'react';

export const getLength = (value: ComponentProps<'textarea'>['value']): number => {
  if (typeof value !== 'string') return 0;
  return value.length;
};

export const isTooLong = (
  value: ComponentProps<'textarea'>['value'],
  maxLength: ComponentProps<'textarea'>['maxLength'],
): boolean => {
  if (typeof value !== 'string') return false;
  if (!maxLength) return false;
  return value.length > maxLength;
};
```

```tsx
// text-area.tsx
import clsx from 'clsx';
import { useMemo, useState, type ComponentProps } from 'react';
import { isTooLong, getLength } from './get-length';

type TextAreaProps = ComponentProps<'textarea'> & { label: string };

export const TextArea = ({ label, required, maxLength, ...props }: TextAreaProps) => {
  const [value, setValue] = useState(props.value ?? '');
  const tooLong = useMemo(() => isTooLong(value, maxLength), [value, maxLength]);
  const length = useMemo(() => getLength(value), [value]);

  console.log({ label, required, maxLength, value, ...props });

  return (
    <label className="flex flex-col gap-1.5">
      <span
        className={clsx(
          'inline-flex items-center gap-1 text-sm font-medium',
          required && 'after:h-1.5 after:w-1.5 after:rounded-full after:bg-accent-500',
        )}
      >
        {label}
      </span>

      <textarea
        className={clsx(
          'w-full gap-2 rounded-md bg-transparent bg-white p-4 text-sm placeholder-slate-400 shadow-sm ring-1 ring-inset ring-slate-500 invalid:bg-danger-50 focus:bg-primary-50 focus:outline-none focus:ring-2 focus:ring-primary-600 disabled:cursor-not-allowed disabled:bg-slate-50 dark:bg-slate-800 dark:placeholder-slate-300',
          tooLong && 'ring-2 ring-danger-500 dark:ring-danger-500',
        )}
        {...props}
        onChange={(e) => {
          setValue(e.target.value);
          if (typeof props.onChange === 'function') props.onChange(e);
        }}
        value={value}
        required={required}
        aria-invalid={tooLong}
      />

      {maxLength && (
        <div className="gap-1.4 flex justify-end text-xs">
          <p className={clsx(tooLong ? 'text-danger-500' : 'text-slate-600')}>
            <span data-testid="length">{length}</span>/{maxLength}
          </p>
        </div>
      )}
    </label>
  );
};
```

> textarea's `maxLength` attribute does not trigger HTML input validation (in this example, we use the aria-invalid property)

```tsx
/// text-area.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { TextArea } from './text-area';

import { userEvent, within, expect } from '@storybook/test';

const meta = {
  title: 'Components/TextArea',
  component: TextArea,
  args: {
    label: 'Text Area Label',
    placeholder: 'Enter some text here…',
    disabled: false,
    required: false,
  },
  argTypes: {
    label: {
      name: 'Label',
      control: 'text',
      description: 'Label of the text area',
    },
    placeholder: {
      name: 'Placeholder',
      control: 'text',
      description: 'Placeholder text of the text area',
    },
    disabled: {
      name: 'Disabled',
      control: 'boolean',
      description: 'Disables the text area',
      table: {
        defaultValue: {
          summary: false,
        },
      },
    },
    required: {
      name: 'Required',
      control: 'boolean',
      description: 'Marks the text area as required',
      table: {
        defaultValue: {
          summary: false,
        },
      },
    },
  },
} as Meta<typeof TextArea>;

export default meta;
type Story = StoryObj<typeof TextArea>;

export const Default: Story = {
  args: {},
};

export const Disabled: Story = {
  args: { disabled: true },
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    const textArea = canvas.getByRole('textbox');

    expect(textArea).toBeDisabled();

    const inputValue = 'Hello, world!';
    await userEvent.type(textArea, inputValue);

    expect(textArea).not.toHaveValue(inputValue);
  },
};

export const WithCount: Story = {
  args: { maxLength: 140 },
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);

    const textArea = canvas.getByRole('textbox');
    const count = canvas.getByTestId('length');

    const inputValue = 'Hello, world!';

    await userEvent.type(textArea, inputValue);
    expect(count).toHaveTextContent(inputValue.length.toString());
  },
};

export const LengthTooLong: Story = {
  args: { maxLength: 140 },
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);

    const textArea = canvas.getByRole('textbox');
    const count = canvas.getByTestId('length');

    const inputValue = 'He' + 'l'.repeat(140) + 'o, world!';

    await userEvent.type(textArea, inputValue);
    expect(count).toHaveTextContent(inputValue.length.toString());
    expect(textArea).toHaveAttribute('aria-invalid', 'true');
    expect(textArea).toHaveClass('ring-danger-500');

    expect(count).toHaveStyle({ color: 'rgb(237, 70, 86)' });
  },
};
```

**Test Runner**
```
npm install -D @storybook/test-runner
```

Storybook uses Playwright
```
npx playwright install
```

**Run tests** (Storybook must be running)
```
npx test-storybook
```

**Chromatic for visual regression tests**
```
npx storybook@latest add @chromatic-com/storybook
```

**Accessibility Testing in Storybook** (uses axe-core under the hood)
```
npx storybook@latest add @storybook/addon-a11y
```

**Running Accessibility Audits as Part of Your Test Suite**
>You’ll need to install axe-playwright if it’s not already installed. You can take care of that by running `npm install axe-playwright --save-dev`.

```ts
// .storybook/test-runner.ts

import type { TestRunnerConfig } from '@storybook/test-runner';
import { injectAxe, checkA11y } from 'axe-playwright';

const config: TestRunnerConfig = {
	async preVisit(page) {
		await injectAxe(page);
	},
	async postVisit(page) {
		await checkA11y(page, '#storybook-root', {
			detailedReport: true,
			detailedReportOptions: {
				html: true,
			},
		});
	},
};

export default config;
```
# 6. APIs, Context, and External Dependencies
- Using Decorators for Context
- Using Loaders to Fetch Data
```tsx
import type { Meta, StoryObj } from '@storybook/react';
import { TaskList } from './task-list';
import { TaskListProvider } from './task-list-context';

const meta = {
  title: 'Components/TaskList',
  component: TaskList,
  loaders: [
    async () => {
      const tasks = await fetch('https://jsonplaceholder.typicode.com/todos').then((res) =>
        res.json(),
      );

      return { tasks };
    },
  ],
  decorators: [
    (Story, context) => {
      return (
        <TaskListProvider tasks={context.loaded.tasks}>
          <Story {...context} />
        </TaskListProvider>
      );
    },
  ],
} as Meta<typeof TaskList>;

export default meta;
type Story = StoryObj<typeof TaskList>;

export const Default: Story = {
  args: {},
};
```

---
---

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

