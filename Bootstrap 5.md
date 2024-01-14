#### CDN
```
https://getbootstrap.com/docs/5.3/getting-started/download/#cdn-via-jsdelivr
```

---
### Typography
#### Heading Styles
- font styling of a heading
```
h1 h2 h3 h4 h5 h6
```

#### Display Headings
- a larger, slightly more opinionated heading style 
```
display-1 display-2 display-3 display-4 display-5 display-6
```

#### Lead
- make a paragraph stand out
```
lead
```

---
### Text

#### Alignment
```
text-start text-center text-end
```

### Decoration
```
text-decoration-underline text-decoration-line-through 
```

#### Weight
```
fw-semibold fw-bold fw-bolder fw-light fst-italic
```

#### Small
- side-comments and small print
```
<small></small> small
```

---
### Colors

#### text-, bg-
```
primary secondary success danger warning info
light dark
white black
muted
```

---
### Button

#### Base
```
btn
```

#### Variants
```
btn-<color> btn-outline-<color> btn-link
```

#### Sizes
```
btn-sm btn-lg
```

#### Group
```
<div class="btn-group" role="group" aria-label="Basic example">
  <button type="button" class="btn btn-primary">Left</button>
  <button type="button" class="btn btn-primary">Middle</button>
  <button type="button" class="btn btn-primary">Right</button>
</div>
```

---
### Utility Classes

### Margin
```
m- mt- mb- ms- me- mx- my-

0 1 2 3 4 5 auto
```

#### Padding
```
p- pt- pb- ps- pe- px- py-

0 1 2 3 4 5 auto
```

### Borders

#### Base
```
border border-top border-end border-start border-end
```

#### Color
```
border-<color>
```

#### Sizes
```
border-1 border-2 border-3 border-4 border-5
```

#### Radius
```
rounded rounded-circle rounded-pill
```

#### Box Shadow
```
shadow shadow-sm shadow-lg
```

---
### Layout

#### Containers
```
container container-sm container-md container-lg container-xl container-xxl

container-fluid (100% width)
```

#### Grid
- bootstrap uses a **twelve** column system

| col 1a | col 1b | col 1c | col 1d |
| ---- | ---- | ---- | ---- |
| **col 2a** |  |  | **col 2b** |

```
<div class="container">
  <div class="row">
    <div class="col">col 1a</div>
    <div class="col">col 1b</div>
    <div class="col">col 1c</div>
    <div class="col">col 1d</div>
  </div>
  <div class="row">
    <div class="col-8">col 2a</div>
    <div class="col-4">col 2b</div>
  </div>
</div>
```

#### Row
- rows are column wrappers for a grid
```
row
```

#### Column
- default behavior is that each column takes up equal space
```
col
```

#### Specify Width
```
col-1 col-2 col-3 col-4 col-5 col-6 col-7 col-8 col-9 col-10 col-11 col-12
```

#### Break Points
```
col-xs-<width> col-sm-<width> col-md-<width> 
col-lg-<width> col-xl-<width> col-xxl-<width>
```

#### Gap
```
g-* gx-* gy-* g-0
```
#### Flex
```
d-flex d-inline-flex
```

#### Flex Direction
```
flex-row flex-row-reverse flex-column flex-column-reverse
```

#### Justify Columns
- applies to flex and grid
```
justify-content-start justify-content-end justify-content-center
justify-content-between justify-content-around justify-content-evenly
```

#### Align Items
- applies to flex and grid
```
align-items-start align-items-end align-items-center
align-items-baseline align-items-stretch
```

---
# Using the grid system to control width
- here the form will take up 6 columns on large screens and full width (12 columns) on any size smaller
```
<div class="row justify-content-center>
	<div clas="col-lg-6">
		<form>
			...
		</form>
	</div>
</div>
```

---
### Form Example
- this form utilizes input groups and tool tips
```
<form>
  <!-- email -->
  <label for="email" class="form-label">Email address:</label>
  <div class="mb-4 input-group">
	<span class="input-group-text">
		<i class="bi bi-envelope-fill"></i>
	</span>
	<input
	  type="email"
	  class="form-control"
	  id="email"
	  placeholder="e.g. mario@example.com"
	/>
	<!-- tooltip -->
	<span class="input-group-text">
	  <span
		class="tt"
		data-bs-placement="bottom"
		title="Enter an email address we can reply to.">
		<i class="bi bi-question-circle text-muted"></i>
	  </span>
	</span>
  </div>
  <!-- subject -->
  <label for="subject" class="form-label">What is your question about?</label>
  <div class="input-group mb-4">
	<span class="input-group-text">
	  <i class="bi bi-chat-right-dots-fill"></i>
	</span>
	<select id="subject" class="form-select">
	  <option value="pricing" selected>Pricing query</option>
	  <option value="content">Content query</option>
	  <option value="other">Other query</option>
	</select>
  </div>
  <!-- query -->
  <div class="form-floating mb-4 mt-5">
	<textarea class="form-control" id="query" style="height: 140px"></textarea>
	<label for="query">Your query...</label>
  </div>
  <div class="mb-4 text-center">
	<button type="submit" class="btn btn-secondary">Submit</button>
  </div>
</form>
```

### JavaScript For Tool tip
- here `tt` is a class of our choosing
```
 <script>
      const tooltips = document.querySelectorAll(".tt");
      tooltips.forEach((t) => {
        new bootstrap.Tooltip(t);
      });
</script>
```