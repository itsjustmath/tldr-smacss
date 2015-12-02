# [SMACSS: Scalable and Modular Architecture for CSS by Jonathan Snook](http://smacss.com/)

## [Introduction](http://smacss.com/book/)

SMACSS is an attempt to document a consistent approach to site development when using CSS. SMACSS is about identifying the patterns in your design and codifying them.

**Two core goals: increase semantics and decrease reliance on specific HTML**

## [Categorizing CSS Rules (Core)](http://smacss.com/book/categorizing)

At the very core of SMACSS is categorization. By categorizing CSS rules, we begin to see patterns and can define better practices around each of these patterns.

There are five types of categories:

1. Base
2. Layout
3. Module
4. State
5. Theme

Much of the **purpose of categorizing things is to codify patterns**—things that repeat themselves within our design. Repetition results in less code, easier maintenance, and greater consistency in the user experience.

**Base rules** are the defaults. They are almost exclusively single element selectors but it could include attribute selectors, pseudo-class selectors, child selectors or sibling selectors. Essentially, a base style says that wherever this element is on the page, it should look like \_this\_.

### Examples of Base Styles
```css
html, body, form { margin: 0; padding: 0; }
input[type=text] { border: 1px solid #999; }
a { color: #039; }
a:hover { color: #03C; }
```

**Layout rules** divide the page into sections. Layouts hold one or more modules together.

**Modules** (or components) are the reusable, modular parts of our design. They are the callouts, the sidebar sections, the product lists and so on.

**State rules** are ways to describe how our modules or layouts will look when in a particular state. Is it hidden or expanded? Is it active or inactive? They are about describing how a module or layout looks on screens that are smaller or bigger. They are also about describing how a module might look in different views like the home page or the inside page.

**Theme rules** are similar to state rules in that they describe how modules or layouts might look. Most sites don’t require a layer of theming but it is good to be aware of it.

### Naming Rules

I like to use a prefix to differentiate between Layout, State, and Module rules. For Layout, I use `l-` but `layout-` would work just as well. Using prefixes like grid- also provide enough clarity to separate layout styles from other styles. For State rules, I like `is-` as in `is-hidden` or `is-collapsed`.

## [Base Rules (Core)](http://smacss.com/book/type-base)

A Base rule is applied to an element using an element selector, a descendent selector, or a child selector, along with any pseudo-classes. **It doesn’t include any class or ID selectors**. It is defining the default styling for **how that element should look in all occurrences** on the page (or site).

Base styles include setting heading sizes, default link styles, default font styles, and body backgrounds. There should be no need to use `!important` in a Base style.

**CSS Resets**

A CSS Reset is a set of Base styles designed to strip out—or reset—the default margin, padding, and other properties. Its purpose is to define a consistent foundation across browsers to build the site on

## [Layout Rules (Core)](http://smacss.com/book/type-layout)

There is a distinction between layouts dictating the major and minor components of a page.

**Minor components** (eg. callout, or login form, or a navigation item) = **Modules** (or Components)

**Major components** = **Layout** styles

Layout styles can also be divided into major and minor styles based on reuse.

##### Layout declarations
```css
`header, article, footer {
	width: 960px;
	margin: auto;
}

article {
	border: solid #CCC;
	border-width: 1px 0 0;
}
```
`
Generally, a Layout style only has a single selector: a single ID or class name.

## [Module Rules](http://smacss.com/book/type-module)

Modules sit inside Layout components. Modules can sometimes sit within other Modules, too. **Each Module should be designed to exist as a standalone component.** When defining the rule set for a module, avoid using IDs and element selectors, sticking only to class names.

### Avoid element selectors

**Only include a selector that includes semantics.** A span or div holds none. A heading has some. A class defined on an element has plenty.


### Subclassing Modules

ie. When we have the same module in different sections. If load order is a factor in your project, watch out for specificity issues. Sub-classing the module will allow the module to be moved to other sections of the site more easily and you will avoid increasing the specificity unnecessarily.

#### Battling against specificity

*BAD* Styling with generic element

```html
`<div class="fld">
    <span>Folder Name</span>
    <span>(32 items)</span>
</div>
```
`
*GOOD* Styling with generic element
```html
<div class="fld">
    <span class="fld-name">Folder Name</span>
    <span class="fld-items">(32 items)</span>
</div>
```

## [State Rules](http://smacss.com/book/type-state)

A state is something that augments and overrides all other styles. A state is something that augments and overrides all other styles.

### State applied to an element

```html
<div id="header" class="is-collapsed">
    <form>
        <div class="msg is-error">
            There is an error!
        </div>
        <label for="searchbox" class="is-hidden">Search</label>
        <input type="search" id="searchbox">
    </form>
</div>
```

How state styles are different from module styles:

1. State styles can apply to layout and/or module styles; and
2. State styles indicate a JavaScript dependency.

Sub-module styles are applied to an element at render time and then are never changed again. State styles, however, are **applied to elements to indicate a change in state** while the page is still running on the client machine.

**Using !important**  

States should be made to stand alone and are usually built of a single class selector. If necessary, to override the style of a more complex rule set, use `!important`.

## [Theme Rules](http://smacss.com/book/type-theme)  

A theme defines colours and images that give your application or site its look and feel. Themes can affect any of the primary types. **It could override base styles** like default link colours. It could change module elements such as chrome colours and borders. It could affect layout with different arrangements. It could also alter how states look.  

### Module Theming

```css
// in module-name.css
.mod {
	border: 1px solid;
}

// in theme.css
.mod {
	border-color: blue;
}
```

**Typography**  

Create separate font files for each locale that redefine the font size for those components. Font rules will normally affect base, module and state styles. Font styles won’t normally be specified at the layout level as layouts are intended for positioning and placement, not for stylistic changes like fonts and colors. *Your site should **only** have 3 to 6 different font-sizes.*

## [Changing State](http://smacss.com/book/state)  

State changes are represented in one of three ways:  

1. **class name**:  change happens with JavaScript. Via some interaction, an element gets a new class applied and then the visual appearance changes.
2. **pseudo-class**: done via any number of pseudo-classes.
3. **media query**: describe how things should by styled under defined criteria, such as different viewport sizes.

When you actively ask yourself, **“what is the default state,”** then you’ll find yourself thinking proactively about progressive enhancement.

Thinking about your interface not only modularly but as a representation of those modules in various states will make it easier to separate styles appropriately and build sites that are easier to maintain.

## [Depth of Applicability (Aspects of SMACSS)](http://smacss.com/book/applicability)  

**Minimizing the Depth**

HTML is like a tree structure of parents and children. The depth of applicability is the number of generations that are affected by a given rule. 

```
// depth of applicability of 6 generations
body.article \> #main \> #content \> #intro \> p \> b
// the depth is still the same: 6 generations
.article #intro b
```

The problem with such a depth is that it enforces a **much greater dependency on a particular HTML structure.**   

##### Duplication of rules

```css
# sidebar div, #footer div {
	border: 1px solid #333;
}

# sidebar div h3, #footer div h3 {
	margin-top: 5px;
}

# sidebar div ul, #footer div ul {
	margin-bottom: 5px;
}
```

##### Simplification of rules

```css
.pod {
	border: 1px solid #333;
}

.pod \> h3 {
	margin-top: 5px;
}

.pod \> ul {
	margin-bottom: 5px;
}
```

##### Duplication of rules

```css
.pod \> ul, .pod \> ol, .pod \> div {
	margin-bottom: 5px;
}

##### Simplifying with a class

.pod-body {
	margin-bottom: 5px;
}
```

## [Selector Performance](http://smacss.com/book/selectors)

**How CSS gets evaluated**
Browsers are designed to handle documents like a stream. They begin to receive the document from the server and can render the document before it has completely downloaded. Each node is evaluated and rendered to the viewport as it is received ([Visualization](http://youtu.be/ZTnIxIA5KGw)).  Related: [CSS selectors performance comparison](http://stackoverflow.com/questions/12279544/which-css-selectors-or-rules-can-significantly-affect-front-end-layout-renderi)

**CSS gets evaluated from right to left**  
By working right to left, the browser can determine whether a rule applies to this particular element that it is trying to paint to the viewport much faster.

**Which rules rule?**

According to Google Page Speed recommendations there are four main rules that they consider inefficient:  

1. Rules with descendant selectors. E.g. `#content h3`
2. Rules with child or adjacent selectors. E.g. `#content > h3`  
3. Rules with overly qualified selectors. E.g. `div#content > h3`
4. Rules that apply :hover to non-link elements. E.g. `div#content:hover`

ie. The evaluation of any more than a single element to determine styling is inefficient. 

Three simple **guidelines to help limit the number of elements** that need to be evaluated:  

1. Use child selectors
2. Avoid tag selectors for common elements
3. Use class names as the right-most selector

## Other Sections
- [HTML5 and SMACSS](http://smacss.com/book/html5)
- [Prototyping](http://smacss.com/book/prototyping)
