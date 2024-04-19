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


# Plugins

- **content reel**
	- filler content (text, images, etc.)

--- 
v1

---
## Styles

- can create variables for colors, texts, shadows, borders, and layouts
- <kbd> Ctrl </kbd> + <kbd> R </kbd> will let you rename colors
```
Grayscale / $n00

Grayscale / 100
Grayscale / 200
```

 **plugins**
 - `Styler` -  to create variables in bulk
 - `CSSGen` - generate variables to export to `CSS / SASS` files
 - many other plugins (to generate color palettes, etc.)


---

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


## Variants
- create component sets
- using `checked=true, checked=false` gives your variants a toggle instead of a drop down

## Swapping Components
- naming conventions
	- id you have components bunched up in the same frame, figma will batch them up
	- you can add a **/** in the name to create folders
- create a single component specifically for the purpose of changing base styles (not for using as itself)


## Component Recipes
- you switch a component for another component (such as icons)
- create a slot component as a placeholder to swap out with out components (a frame with 1px height to swap out)
	- such as for drop down items

## Prototyping
- in prototype mode can create flows to show interactivity
	- button states
	- screen transitions

## Recommended Plugins
- Content Reel
- Unsplash
- Coda
- Design Lint
- Batch Styler
- Figma to HTML, CSS, React & more!
- Typescale
- Autoflow