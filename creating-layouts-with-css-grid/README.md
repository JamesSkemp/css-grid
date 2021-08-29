# Creating Layouts with CSS Grid
Playing around while following [Matt Henry's course on Pluralsight](https://app.pluralsight.com/library/courses/css-grid-creating-layouts/table-of-contents).

## Notes

### Basics
- grid-column-start
- grid-column-end
- grid-template-columns
- grid-template-rows

```css
html, body {
	height: 100%;
}

body {
	/* Changes all direct children into grid items. */
	/* By default 1 column 1 row grid. */
	display: grid;
	/* Number of columns and width. */
	grid-template-columns: 15em auto 15em;
	/* Number of rows and heights. */
	/* min-content is minimum amount to fit content, and no more. */
	/* auto only works here because of 100% height on html and body. */
	grid-template-rows: min-content auto min-content;
	/* Above is the same as the following. rows / columns. */
	/*
	grid-template: min-content auto min-content / 15em auto 15em;
	*/
	/* Can also use the more powerful grid, with just what we want. */
	/* grid is also recommended, while grid-template is not. */
	/*
	grid: min-content auto min-content / 15em auto 15em;
	*/
}

header, footer {
	/* Grid line to start at. */
	grid-column-start: 1;
	/* Grid line to end at. */
	grid-column-end: 4;
}
```

### Sizing

Viewport-responsive:
- Percentage
- fr
	- These will be min-content until all other tracks reach their growth limit (for example, `minmax()`).
- auto
	- Growth limit is `max-content`.
	- This means if a `1fr` is used it won't get any larger than `max-content`.

Content-responsive:
- min-content
- max-content
- fit-content()


- `fr` fractional unit.
- `minmax(max-content, 50%)` define minimum and maximum values for a track.
- `fit-content(30em)` try to fit the content, but don't go any larger than an amount.
- `repeat(3, 20em)` repeats x times a track size of a certain amount.
	- `repeat(6, 1fr 2fr)`
- `repeat(auto-fill, 10em)` fills up available space with tracks of set size.
	- `repeat(auto-fill, minmax(15em, 1fr))` is an example of combining for a responsive design.
- `auto-fit` tries to fill empty columns, versus `auto-fill`.
	/* Default. Fills it row-by-row. */
- `grid-auto-flow: row;` is the default and tries to fill the grid row-by-row.
	- `grid-auto-flow: column;` fills column-by-column instead.
	- Alternative is to put it in the `grid` definition. For example `grid: auto-flow 10em / 10em 10em`.
- `writing-mode: vertical-rl;` and `writing-mode: horizontal-tb` are examples of ways to get `grid-auto-flow` to start a certain way, but may also make columns look like rows.
- `grid-auto-rows` can impact implicit rows created if the grid definition doesn't account for all grid items.
	- Supports more than one size, in which case it will alternate.
	- `grid-auto-columns` is the other.
	- Depends upon `grid-auto-flow` definition.

#### Determing track size (slide)
1. All tracks start at their base size.
2. Extra space is allocated evenly to tracks which haven't reached their growth limit.
3. Additional remaining space is given to fractional unit (`fr`) tracks.
4. If there are no fractional unit tracks, additional space is given to `auto` tracks.

### Gaps
- `column-gap`, `row-gap`, and `gap` (shorthand) are the recommended properties.
	- Makes it consistent with CSS Grid, Flexbox, and Multi-column Layout.
	- `grid-` prefix is also acceptable.
- `margin` on grid container for outer margin.

### Positioning
- Can use -1 for end position (or any negative numbers) instead of a positive number. Negative numbers count from the end of the row to the start.
- However, avoid micromanaging.
- On a grid item, can use span x to span a certain number of tracks.
	- `grid-column-end: span 2;`
- `grid-column: start / end;`
	- `grid-column: 3 / 5;`
	- `grid-column: 3 / span 2;`
	- `grid-column: span 2;`
	- `grid-row`
- `grid-area: 2 / 1 / 5 / 6;` is grid row start, column start, row end, column end.
	- May want to just use `grid-column` and `grid-row` for readability.

### Alignment
- Can align a grid on a web page or the content within a grid cell, and there must be extra space to align within.
- Properties:
	- `justify-content` default is start.
		- Other options are center, end, space-around, space-between, and space-evenly. Controls entire grid.
	- `align-content` default is start.
		- Other options are center, end, space-around, space-between, and space-evenly. Controls entire grid.
	- `justify-items` default is stretch.
		- Other options (for this and next three properties) are start, center, and end. Controls content within grid item.
	- `align-items` default is stretch. Controls content within grid item.
	- `justify-self` default is stretch. Controls an individual item.
	- `align-self` default is stretch. Controls an individual item.
- Justify = left to right.
- Align = top to bottom.

### Accessibility
- Can use `dense` in `grid-auto-flow` and it will try to fill in empty gaps (in the case of spanning grid items).
- Can also use `order` to force items before/after items. Default is `order: 0;`.
- However, try to keep the source order = display order.
- Can also overlaps grid items by overlapping row/column placements. Combine with `z-index` as needed.

### Naming
- When defining track sizes, you can name them.
	- `grid-template-columns: [left-edge] 1fr 1fr [midpoint] 1fr 1fr [right-edge];`
	- Then use like `grid-column: left-edge / right-edge;`.
- Something like `body { display: grid; grid-template-rows: [header-start] 2em 5em [header-end body-start] 10em 10em [body-end]; }` is a bit more conventional.
- Can also use names within `repeat()` which can cause multiple named lines.
	- When used for grid items, this will cause them to jump to the closest named line.
	- Can put a number afte the name to use that instance.
	- Can also use `span x` before to span over multiple, but be careful when repeating.

#### Simple layouts
```
body {
	display: grid;
	grid-template-areas: "header header header"
						"nav main aside"
						"footer footer footer";
	grid-template-rows: min-content auto min-content;
	grid-template-columns: 15em 1fr 1fr;
}

header { grid-area: header; }
nav { grid-area: nav; }
main { grid-area: main; }
aside { grid-area: aside; }
footer { grid-area: footer; }
```

Good for simple layouts.

- Must be rectangular shape.
- Must have same number of cells.
- `...` to define empty areas.
- Lines automatically get names based upon area names.
- Named lines automatically create named areas.

Shorter version of the above:

```
body {
	display: grid;
	grid: "header header header" min-content "nav main aside" auto "footer footer footer" min-content / 15em 1fr 1fr;
}
```

Or to improve readability:

```
body {
	display: grid;
	grid: "header header header" min-content
		"nav main aside" auto
		"footer footer footer" min-content /
		15em 1fr 1fr;
}
```

### Responsive design
- Start small.
- Use media queries. :|

### Subgrids
- May not be well supported (as of me typing this, still only Firefox supported).
- You can of course put a grid within a grid.
