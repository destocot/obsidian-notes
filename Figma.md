## Basics

Move - V
Scale - K
Frames - F
Rectangles - R

Design
Prototype
Inspect (`DevMode`)

<kbd> Ctrl </kbd> + 🖱️ (Select frame)
<kbd> Alt </kbd>  + 🖱️ (Make duplicate of a layer)
<kbd> Shift </kbd> + <kbd> + </kbd> (Zoom in)
<kbd> Shift </kbd> + <kbd> - </kbd> (Zoom out)
<kbd> Shift </kbd> + <kbd> 2 </kbd> (Zoom to current selection)

<kbd> Ctrl </kbd> + <kbd> D </kbd> (Make a copy, directly on top)
<kbd> Shift </kbd> + <kbd> R </kbd> (Show ruler guides)
- can drag from sides to create guides that can be snapped on to

1. [[#1. The Basics]]
	1. Aligning Objects
	2. Working with Layers
	3. Selecting and Inspecting
2. [[#2. Layout]]
	1. Constraints
	2. Layout Grids
	3. Auto Layouts
3. [[#3. Styling]]
	1. Styles / Typography
	2. Variables
	3. Generating a Color Palette / Semantic Colors
4. [[#4. Components]]
	1. Components

---
# 1. The Basics
## Aligning Objects

- Hold <kbd>Alt</kbd> key - measure difference between two objects

- (right pane) alignment tools
	- left | center (horizontal) | right | top | middle (vertical) | bottom
	- tidy up | distribute (vertical | horizontal) spacing

> if you highlight multiple objects and they have even spacing, you will have access in the (right pane) to the spacing properties

## Working with Layers
- **Groups** - ideal for quick, temporary arrangements and applying uniform styling across multiple elements (temporary grouping*)
	- <kbd> Ctrl </kbd> + <kbd> G </kbd> (Create a group)

- **Frames** - serve as the fundamental structure for your design, accommodating everything from screen layouts to components
	- you can swap items by there center (pink circle)
	- <kbd> Ctrl </kbd> + <kbd> Alt </kbd> + <kbd> G </kbd> (Create a frame)

- **Sections** - offer a new, top-level organizational layer, perfect for enhancing collaboration and streamlining navigation and presentation.
	- <kbd> Ctrl </kbd> + <kbd> S </kbd> (Create a section)

- <kbd> Ctrl </kbd> + <kbd> Shift </kbd> + <kbd> G </kbd> (Remove a group / frame)

## Selecting and Inspecting
- (within a frame) you can select all objects with same properties
---
# 2. Layout

## Constraints
- by default objects are constrained to the **top** and to the **left**

- constraint options
	- horizontal: | left | right | left and right | center | scale |
	- vertical: | top | bottom | top and bottom | center | scale |

## Layout Grids
- layout grids can only be applied to **frames**

- | grid | rows | columns |

> use 1 column with 1 row to visualize padding

- Show / Hide Grids <kbd> Ctrl </kbd> + <kbd> Shift </kbd> + <kbd> 4 </kbd> 
- Layout Grids get really powerful when combined with Constraints
- Frame > (Select Devices) | (Select Orientation)

## Auto Layout
- auto layout can only **frames**

> auto layout has a similar paradigm to flex box in CSS
 
- create auto layout <kbd> Shift </kbd> + <kbd> A </kbd>

- auto layout's can be nested

- properties of auto layouts
	- gap, horizontal / vertical padding, alignment, direction, wrapping

> auto layouts can be used to represent responsive design (e.g. switching between flex directions)

# 3. Styling

## Styles / Typography

- can create variables for colors, **typography**, **shadows**, borders, and **layout grids**
- <kbd> Ctrl </kbd> + <kbd> R </kbd> will let you bulk rename
- Using a `/` in the frame name with categorize styles
	- e.g. text-9xl/font-thin, text-9xl-font-extra-light, ...

## Variables
- still in beta
- can create - color, number, String, boolean
- can be scoped
	- e.g. only for text, only for stroke

## Generating a Color Palette / Semantic Colors

# 4. Components

## Components
- can be treated like JavaScript prototypal inheritance
- create a component from an existing frame
	- <kbd> Ctrl </kbd> + <kbd> Alt </kbd> + <kbd> K </kbd>
- What cannot be overridden
	- the order of things
	- the position of things
	- constraints
	- the bounds of text layers
	- adding new stuff*
- can right click to reset all overrides

## Component Properties
- can create properties
	- e.g. toggle elements

## Variants
- create component sets
- e.g. hover, active states
- using `checked=true, checked=false` gives your variants a toggle instead of a drop down

- naming conventions
	- id you have components bunched up in the same frame, figma will batch them up
	- you can add a **/** in the name to create folders
- create a single component specifically for the purpose of changing base styles (not for using as itself)

- modes:
	- can be used for themes or view ports
		- e.g. light vs dark mode

Component Recipes
- you switch a component for another component (such as icons)
- create a slot component as a placeholder to swap out with out components (a frame with 1px height to swap out)
	- such as for drop down items

## Prototyping
- in prototype mode can create flows to show interactivity
	- button states
	- screen transitions

# Plugins

- **content reel**
	- filler content (text, images, etc.)
- **`styler` plugin**
	- creates styles in bulk (named after frame name)
- `CSSGen`
	- generate variables to export to CSS / SASS files
- `Typescales`
	- generate bulk typography
- `Tailwind CSS Color Generator` - https://uicolors.app/create 
	- creates colors guide ( styles or variables )
- `Styles to Variables Plugin`
- (screen shot / photo to current UI)
- `variables2css`
	- generates css variables from figma variables







