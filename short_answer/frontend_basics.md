# Front End Basics

## List at least 3 HTML tags which were not mentiond in class and explain the usage

[HTML Reference](https://htmlreference.io/)

* `canvas` - Defines an element where you can draw graphics. All drawing on the HTML canvas must be done with JavaScript.

* `aside` - Defines a block of content that is related to the main content. Displayed as a sidebar usually.

* `nav` - Defines a section with navigation links.

* `b` - Makes an element **bold**.

* `i` - Makes an element *italic*.

* `em` - Defines emphasis on text. Is usually rendered as *italic* text.

* `s` - Defines <s>strikethrough</s> text.

* `u` - Makes an element's text <u>underlined</u>.

* `sup` - Defines text that should be displayed higher. For example: 2<sup>n</sup>

* `sub` - Defines text that should be displayed lower. For example: CO<sub>2</sub>

* `kbd` - Defines a keyboard input.

* `span` - Defines a generic inline container of content, that does not carry any semantic value.

* `<hr />` - Defines a semantic break between blocks of text. It is often represented as a horizontal rule.

* [`<source />`](https://htmlreference.io/element/source/) - Defines a source for an `<audio>`, `<video>`, or `<picture>` element.

* [meter](https://htmlreference.io/element/meter/) - Defines a horizontal meter.

* [progess](https://htmlreference.io/element/progress/) - Defines a progress bar.

* [iframe](https://htmlreference.io/element/iframe/) - Defines a container for a nested browsing context: you can include a web page within another web page.

* [`<embed />`](https://htmlreference.io/element/embed/) - Defines a container for external application. For example,
  ```html
  <embed src="https://www.youtube.com/embed/kmk43_2dtn0" width="640" height="480">
  ```

### Form related

* [form](https://htmlreference.io/element/form/) - Defines an interactive form with controls.

* [label](https://htmlreference.io/element/label/) - Defines a label for a form control.

* [`<input />`](https://htmlreference.io/element/input/) - Defines an interactive control within a web form.

* [textarea](https://htmlreference.io/element/textarea/) - Defines a multi-line text control within a web form.

* [datalist](https://htmlreference.io/element/datalist/) - Defines a list of autocomplete options when using a text `<input>`.

* [fieldset](https://htmlreference.io/element/fieldset/) - Defines a group of controls within a form.

* [legend](https://htmlreference.io/element/legend/) - Defines a caption for a parent's content.

* [select](https://htmlreference.io/element/select/) - Defines a select dropdown with different `<option>` elements.

* [option](https://htmlreference.io/element/option/) - Defines an option within a `<select>` dropdown.

* [optgroup](https://htmlreference.io/element/optgroup/) - Defines a group of `<option>` elements.

### Table related

* [table](https://htmlreference.io/element/table/) - Defines a container for tabular data.

* [caption](https://htmlreference.io/element/caption/) - Defines the title of a `<table>`.

* [tr](https://htmlreference.io/element/tr/) - Defines a table row.

* [th](https://htmlreference.io/element/th/) - Defines a table header. Must be a direct child of a `<tr>`.

* [td](https://htmlreference.io/element/td/) - Defines a table cell. Must be a direct child of a `<tr>`.

* [thead](https://htmlreference.io/element/thead/) - Defines a group of table rows `<tr>` at the start of a `<table>`.

* [tbody](https://htmlreference.io/element/tbody/) - Defines a group of table rows `<tr>`.

* [tfoot](https://htmlreference.io/element/tfoot/) - Defines a group of table rows `<tr>` at the end of a `<table>`.

* [colgroup](https://htmlreference.io/element/colgroup/) - Defines a group of columns within a table.

* [`<col />`](https://htmlreference.io/element/col/) - Defines a column within a table.


## What is the box model?

The CSS box model is the foundation of layout on the web — each HTML element can be considered as a box, with the box's content, padding, border, and margin built up around one another like the layers of an onion.

![CSS box model](https://mdn.mozillademos.org/files/13647/box-model-standard-small.png)


## What is the difference between display: inline, block and inline-block?

```css
{ display: inline; }
```
The element behaves like simple text - like `<span>`.  
Any `height` and `width` properties will have no effect.

```css
{ display: block; }
```
The element starts on a new line, acts like a box and takes up the whole width.

```css
{ display: inline-block; }
```
The element behaves like an inline-level block container.  
It is like simple text, but you can apply `height` and `width` values.  


### Some relatively new display values:

```css
{ display: flex; }
```
Displays an element as a block-level flex container.  
Its child elements will be turned into flexbox items.

```css
{ display: grid; }
```
Displays an element as a block-level grid container.  
ts child elements will be turned into grid items.  


## What is the difference between position: static, relative, absolute and fixed?

```css
{ position: static; }
```
Default value.  
The elements render in orde, remain in the document's **natural flow**.  
It is **not** affected by the `top`, `bottom`, `left`, `right` and `z-index` properties which all the other positions are.

```css
{ position: relative; }
```
The element will remain in the natural flow of the page, but it **can** be adjust by the `top`, `bottom`, `left`, `right` and `z-index` properties.

```css
{ position: absolute; }
```
The element is positioned relative to its first positioned (not static) ancestor element.

```css
{ position: fixed; }
```
It will position itself according to the viewport. It always stays in the same place even if the page is scrolled.