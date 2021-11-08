# Tips

- setting every `box-sizing` to `border-box` can be a good idea as it will make the rendered width/height to include padding and border
	- The border and padding values moved inside the element's box (margin will be outside)
```css
html {
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}
*, *:before, *:after {
	-webkit-box-sizing: inherit;
	-moz-box-sizing: inherit;
	box-sizing: inherit;
}
```
- If required, use an autoprefixer to add vendor prefixes to your CSS

# Flexbox

Container
- Use `display: flex;` to create a flex container.
- Use `justify-content` to define the main-axis alignment of items.
- Use `align-items` to define the cross-axis alignment of items.
- Use `flex-direction` if you need columns instead of rows, or reverse columns/rows
- Use `flex-wrap` if items should wrap onto multiple lines
- Use `align-content` to define how cross-axis extra spacing should be handled (only takes effect when `wrap`/multiple lines are involved). `align-content` is `justify-content` but for the cross-axis.

Item
- Use `order` to customize the order of individual elements.
- Use `align-self` to vertically align individual items.
- Use `flex` to create flexible boxes that can stretch and shrink (`flex-grow`, `flex-shrink`, `flex-basis`)

# CSS Modules (with webpack)

`webpack.config.js`:
```js
module.exports = {
	module: {
		rules: [
			{
				test: /\.css$/i,
				use: [
					"style-loader",
					{
						loader: "css-loader",
						options: {
							modules: true,	
						},
					},
				]
			},
		]
	},
}
```

React component:
```js
import styles from './component.module.css';

const Component = () => {
	return (
		<div className={styles.container}>
			<span></span>
		</div>
	);
}
```

