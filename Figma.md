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

<kbd> Ctrl </kbd> + <kbd> G </kbd> (Create a group)
<kbd> Ctrl </kbd> + <kbd> Shift </kbd> + <kbd> G </kbd> (Remove a group / frame)
<kbd> Ctrl </kbd> + <kbd> Alt </kbd> + <kbd> G </kbd> (Create a frame)




---
## Frames
- aligning objects
	- once you are mostly aligned
		- you can move objects together
		- once dragged will make copies with same spacing
		- you can swap items by there center (pink circle)
- groups
	- helpful when you're constantly selecting multiple objects to move them
- can clip content (overflow-hidden)
- scale vs re-sizing

--- 
## Inspect
- selecting multiple items
	- <kbd> Shift </kbd> + 🖱️ each item
	- File > Edit >  Select all with same fill
	- Select with 🎯 under Selection colors
	- can inspect styles to reference in your `css`
	
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
## Constraints
- everything (for the most part) is constrained to the top and left
- for UI pieces that should remain in relatively the same place regardless of screen size
- Design > Constraints
## Layouts
- Design > Layout Grid > | GRID | COLUMNS | ROW
- You can layer grids on top of each other
	- example: columns / grids
- Show / Hide Grids <kbd> Ctrl </kbd> + <kbd> Shift </kbd> + <kbd> 4 </kbd> 
- Layout Grids get really powerful when combined with Constraints
- Frame > (Select Devices) | (Select Orientation)

Design > **Auto Layout**
- similar paradigm to flex box
- <kbd> Shift </kbd> + <kbd> A </kbd> , create auto layout
- can be nested

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