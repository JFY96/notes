# BEM naming convention for CSS

- use only Latin letters, digits, and dashes
- use `-` rather than camelCase
```css
.redBox /* wrong */
.red-box /* correct */
```
- use `__` (two underscores) for elements contained inside a block with class name `block`
```html
<div class="block">
	<span class="block__elem"></span>
</div>
```
```css
.block__elem { }

/* bad: don't have dependency on other blocks/elements */
.block .block__elem { }
```
- use `--` (two dashes) for modifier names (for blocks/elements) when appearance, behavior or state changes
```html
<!-- modifier is an extra class name to block/element. Should also keep the original class -->
<div class="block block--hidden">
	<span class="block__elem"></span>
	<span class="block__elem block__elem--mod"></span>
</div>
```
```css
/* use modifier class name */
.block--hidden

/* to alter elements based on block level modifier */
.block--hidden .block__elem { }
```
## Example

```html
<form class="form form--theme-xmas form--simple">
  <input class="form__input" type="text" />
  <input
		class="form__submit form__submit--disabled"
		type="submit"
	/>
</form>
```