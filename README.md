# Grid

Starting with grid, we need to declare `display: grid;`


```css
display: grid;
gap: 1.5rem;
grid-template-columns: 100px 100px 100px; 100x;
```

- `gap` for equal spacing for both x,y;

### Values for template-columns

```css
grid-template-columns: 100px 100px 100px 100px;
grid-template-columns: 25% 25% 25% 25%;
grid-template-columns: 1fr 1fr 1fr 1fr;
grid-template-columns: repeat(4, 1fr);
```

As of now we haven't declared rows explicitly but grid is smart enough to make new rows if it reaches the declared column.

## Span 

Creating utility class to span rows

```css
.grid-col-span-2{
	grid-column: span 2;
}

.grid-row-span-2{
	grid-row: span 2;
}
```

Here `grid-column` is a short-hand property for
 - `grid-column-start`
 - `grid-column-end`
 
*Same for rows respectively.* 

### Positioning Items in Grid

It's better for not to create utility class for this as we can't position multiple items at same position so creating an utility class for only one element/item is not worth it.

Selecting the `child item` to position in grid.

*Tip: Enable line numbers in dev tools to see where we putting it*

```css
.container-grid:last-child{
grid-column-start: 4;
grid-column-end: 5;
}
```

In the above example, we are stating that start at line number `4` and end at line number `2`.

So, when we specifiy `grid-column`
```css
grid-column: 4;
```
It actually sets the `grid-column-start` but not grid column end as we don't need `grid-column-end`.

We can do same for the rows as well.
```css
.container-grid:last-child{
	grid-column-start: 4;
	grid-row-start: 1;
	grid-row-end: 3; // OR span 2;
}
```

So, that's how we can explicitly position items in the grid by specifying line numbers. (`start` and `end`).

In the above example, we explicitly declared `grid-row-start` and `grid-row-end` 
Both the lines can be replaced with the following short hand property `grid-row`.

```css
```css
.container-grid:last-child{
	grid-column-start: 4;
	grid-row: 1 / 3;
}
```

 Possible Values for `grid-row`
- 1 / 3 (1 = where to start, 3 = where to end). So, Starts at `1` and End at line number `2`. 
- 1 / span 2; (1 = Where to start, span 2 = Spans the specified column)
- 

Conclusion:
- `span 2` basically means no matter where you start, just span 2.
- Explicitly declaring line numbers means locking where to start and end. (Not recommended unless uncessary)

***

# Responsive Design

Mobile First Responsive Design

```css
.container-grid{
	display: grid;
	gap: 1.5rem;
}

@media (min-width: 50em){
	.grid-col-span-2{
		grid-column: span 2;
	}

	.container-grid{
		grid-template-colums: repeat(4, 1fr);

	}

	.container-grid:last-child{
		grid-column-start: 4;
		grid-row: 1 / span 2;
	}
}
```

***

# Grid template areas

Grid template areas helps to work with Responsive Design much easier as we only have to work on the parent, set how the grid should be looking for each media queries.
- Just define the `grid-area` (child) where they live on, and never touch them again.
- Keep Redefining the `grid-template-areas` as needed.

Starting as usual but now we're going to add a new property `grid-template-areas`

```css
.container-grid{
	display: grid;
	gap: 1.5rem;
	grid-template-areas:
	'one'
	'two'
	'three'
	'four'
	'five';
}
```

We have set the initial areas, 1 item each row.

Now, to place something on grid area, we need to tell to which grid area they should be living on.

```css
.container-grid:nth-child(1){
	grid-area: one;
}
.container-grid:nth-child(2){
	grid-area: two;
}
.container-grid:nth-child(3){
	grid-area: three;
}
.container-grid:nth-child(4){
	grid-area: four;
}
.container-grid:nth-child(5){
	grid-area: five;
}
```
- **Don't write the values in quotation marks for grid-area**

```css
@media (min-width: 30em){
	.container-grid{
		/* Redefining the grid */
		grid-template-areas:
		'one one'
		'two five'
		'three five'
		'four four';
	}
}

@media (min-width: 50em){
	.container-grid{
		/* Redefining the grid */
		grid-template-areas:
		'one one two five'
		'three four four five'
		'
	}
}
```

**Explaination:** 

- As we set, `nth-child` to live on `grid-area: one`
- So if `one` is going across two columns. `(one one)`
- The `nth-child` will also span two columns. 


Each quoted line reprents a row in `grid-template-areas`;

***

To Ensure columns to have always the same size, and we are not going to setup any columns, we can do it at one spot and never have to worry about it again.

On the grid

```css
.container-grid{
	display: grid;
	gap: 1.5rem;
	grid-auto-columns: 1fr; /* Sets Each Column Same Size */
	grid-template-areas:
	'one'
	'two'
	'three'
	'four'
	'five';
}
```

`grid-auto-columns: <size>` - Sets the size of auto columns, Same goes for rows.
`grid-auto-rows: <size>` - Sets the size of automatic generating rows.
