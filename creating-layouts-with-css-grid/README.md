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
- auto

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

### Gaps
- `column-gap`, `row-gap`, and `gap` (shorthand) are the recommended properties.
	- Makes it consistent with CSS Grid, Flexbox, and Multi-column Layout.
	- `grid-` prefix is also acceptable.
- `margin` on grid container for outer margin.




