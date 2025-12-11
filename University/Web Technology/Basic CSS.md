---
Tags: 
Created: 2023-01-11 20:12:36
---
(Links:: [[Web Technology]])
# Using CSS in HTML
- A CSS **selector** specifies the HTML elements to which the specific style rule applies
- A **declaration block** contains one or more declarations separated by semicolons
- A CSS styling **declaration** is a CSS property followed by a colon and the property value
- Applying CSS
	- **inline style**: places CSS decalrations inside a tag's *style attribute*
	- **embedded stylesheet**: places CSS rules in an HTML document's head using `<style>` tags
	- **external stylesheet**: places CSS rules in separate file, imported with `<link>` tag
- child's declaration overrides the parent's declaration when a conflict occurs; inline style overrides embedded or external stylesheet's declaration
# Basic selectors
| Selector         | Specified with | properties                                                                     | Example               |
| ---------------- | -------------- | ------------------------------------------------------------------------------ | --------------------- |
| element          |                |                                                                                | `p{}`                 |
| class            | `.`            |                                                                                | `.name{}`             |
| ID               | `#`            |                                                                                | `#name{}`             |
| descendant       | ` `            | elements contained in other elements                                           | `a img{}`             |
| pseudo-class     | `:`            | elements based on user behavior or element metainformation                     | `p:hover{}`           |
| universal        | `*`            | implied when element name is not specified                                     | `*p{}`                |
| multiple         | `,`            | sparates selectors                                                             | `h1, h2{}`            |
| child            | `>`            | second element is a direct child of the first element                          | `p > strong{}`        |
| general sibling  | `~`            | elements that share the same parent                                            | `div ~ p{}`           |
| adjacent sibling | `+`            | elements share same parent and second element follows directly after first     | ` h1 + p{}`           |
| attribute        | `[atr_name]`   | elements with attribute specified and optional value                           | `img [width="400"]{}` |
| pseudo-element   | `::`           | applies to specific part of element or allow additional content to be inserted | `p::first-line{}`     |
- **element selector**: elements with the specified elements name
- **class selector**: specified with a period character and the class name
- **ID selector**: specified with a hash character and the ID name
- **descendant selector**: specified with a selector, a space and another selector (elements contained in other elements)
- **pseudo-class selector**: specified with a colon character and a pseudo-class name (elements based on user behavior or element metainformation)
  Examples: `:enabled`, `:hover`, `:empty`, `:lang`
# Advanced selectors
- **universal selector**: specified using asterisk character; is implied when element name is not specified
- **multiple selector**: specified using a comma; separates selectors
- **child selector**: specified using greater than character; second element is a direct child of the first element
- **general sibling selector**: specified using a tilde character; elements that share the same parent
- **adjacent sibling selector**: specified using a plus character; elements share same parent and second element follows directly after first
- **attribute selector**: specified with attribute name and optional value comparison enclosed in square brackets
  Common attribute selector comparators:
	- `=` attribute has exact value
	- `~=` attribute contains whole word
	- `^=` attribute begins with value
- **pseudo-element selector**: specified with two colon characters and pseudo-element
  Common pseudo-element selectors:
	- `::After` add content after the matched element
	- `::before` add content before the matched element
	- `::first-line` first line of text in a block element
	- `::first-letter` first letter of text in a block element
	- `::selection` text selected by user
# Common properties
- **Color property**: `#RRGGBB` hexadecimal, `rgb(0, 0, 0)` RGB color value, `hsl(0, 0%, 0%)`, RGBA color value, HSLA color value
- **Background properties**: background-color, background-image, background
- **Float** and **clear property**: control how text flows around HTML elements -> magazine or newspaper article
	- *float*: specifies whether the element will float to the right or left of the element's parent container
	  values: `left`, `right`, `none`
	- *clear*: stops elements from floating
	  value: `both`, `left`, `right`, `none`
- **Display**: controls the layout of the element on a webpage
	- `inline`: Displays as an inline element, like `span`
	- `block`: displays as a block element, like `p`, `h1`
	- `none`: Hides element from being displayed
	- `inline-block`: content like `block`, but formats as an inline element
	- `list-item`: content displayed as list item element
- **CSS Variable**: global scope why declared in `:root` selector; defined with two dashes preceding name; accessed with `var()` function
# Font and text properties
## Font properties
- [[Generic family names]]
## Font sizes
- [[Font sizes]]
## Text properties
- [[Text alignment examples]]

# Box model
## Box model components
## Box sizing
## Content width and height
## Border styles
- [[Common border styles]]
## Margin collapsing and horizontal centering

---
References: