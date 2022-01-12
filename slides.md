---
theme: default
highlighter: shiki
lineNumbers: false
layout: statement
---

# Exploring The Modern CSS Landscape
# 🔭 ⛰

---
layout: statement
---

<img src="/img/css-is-awesome.png" class="mx-auto" width="400">

---

# Challenges With CSS

<v-clicks>

- 🌍 Global nature, cascade, source order
- 💥 Class collisions
- 👓 Specificity struggles
- ➕ Append-only stylesheets
- 👯 Repeated styles
- 👻 Unused styles
- ⚔ Flexibility is a double-edge sword
- 🤡 Non-cohesive styles & magic numbers

</v-clicks>

---

# Solutions Over Time

<v-clicks>

- Wild Wild West 
  - 🤷‍♂️
- Frameworks (Bootstrap, Foundation, Bulma, etc)
  - Prescribed HTML and CSS classes
- Conventions (BEM, SMACSS, etc.)
  - Class names follow a convention to avoid specificity battles
- Atomic/Utility CSS (Tailwind, Tachyons, etc.)
  - Each CSS rule has its own class
  - Classes are composed to create desired styles (`<div class="p-2 text-center bg-red">`)
- CSS-in-JS (Styled-components, CSS modules, Emotion, etc.)
  - JavaScript runtime injects CSS rules
  - Components are scoped to avoid collisions
  
</v-clicks>

---

# Frameworks

Pros ✅

<v-clicks>

- Extensive component options
- Consistent styling
- Faster bootstrapping/prototyping

</v-clicks>

Cons ⛔

<v-clicks>

- Unused styles
- Requires strict markup 
- Learning tied to library
- More difficult to customize

</v-clicks>

---

# Conventions

Pros ✅

<v-clicks>

- Complete flexibility
- Pseudo-scoping
- Some semblance of where the code exists

</v-clicks>

Cons ⛔

<v-clicks>

- No prescribed approach
- Learn tied to in-house system
- Code and styles live far apart

</v-clicks> 

---

# Atomic CSS

Pros ✅

<v-clicks>

- More flexibility in styles
- Safe to add/remove styles
- No repeated CSS rules
- No scoping needed

</v-clicks>

Cons ⛔

<v-clicks>

- Learning tied to library
- Unused styles *

</v-clicks>

---

# Atomic CSS - Tailwind *

<v-clicks>

- Offers design tokens (colors, fonts, spacing)
- Offers built in unused style removal
- Very small production builds (5kb average)
- Recently announced JIT compilation
  - Arbitrary CSS like `top: -113px` can be achieved with classes like `top-[-113px]`

</v-clicks>

---

# CSS-in-JS

Pros ✅

<v-clicks>

- Theming & design token support
- Flexibility from CSS
- Learning tied to native platform
- No unused styles
- Co-location of styles and components
- Scoped styles

</v-clicks>

Cons ⛔

<v-clicks>

- JavaScript runtime dependency
- Repeated styles (blue button vs red button)
- Only works for JavaScript projects

</v-clicks>

---

# A Fractured Landscape

<v-clicks>

🙋 Some people want the flexibility and learning curve of plain CSS.

🧕 Some people want the structure and speed of frameworks.

👲 Some people want the limited options and reduced size of atomic CSS.

👷 Some people want the safety and maintainability of CSS-in-JS.

👪 Everyone wants their lives to be easier, their applications faster, and maintenance less of a headache.

</v-clicks>

---
layout: statement
---

# What if we combine the best of each option 🤔

---

# We Might Borrow Ideas

<v-clicks>

- Plain CSS
  - Complete flexibility
  - Learning tied to platform
  - Copy/paste portability
  - Static assets
- Atomic CSS
  - No repeated styles (logarithmic growth curve)
  - Design tokens
  - Minimal specificity
- CSS-in-JS
  - Co-located styles
  - TypeScript support
  - No unused styles

</v-clicks>

---
layout: statement
---

<img src="/img/prtcls.svg" alt="Particles CSS" width="500" class="mb-8 mx-auto">

An atomic CSS-in-JS solution that only ships the styles you use with no runtime dependency.

---

# How It Works

<v-clicks>

You install a `babel.js` plugin.

It find all your CSS rules and writes them to a static file.

At the same time, it replaces the style definitions with a string of atomic class names.

You use the string of class names on an element.

The result: a smaller JavaScript footprint and static CSS with only the rules you used.

</v-clicks>

---

# Before transpilation

Development code:
```js
import { css } from 'prtcls'

const buttonStyles = css({
  padding: '5px',
  color: 'blue'
})

export default () => {
  return <button class={buttonStyles}>Click me</button>
}
```

Particles CSS stylesheet:
```css
```

---

# After transpilation

Production code:
```js
// no more library import

const buttonStyles = 'p_5px c_blue' // string of atomic classes

export default () => {
  return <button class={buttonStyles}>Click me</button>
}

```

Particles CSS stylesheet:
```css
.p_5px {
  padding: 5px;
}
.c_blue {
  color: blue;
}
```

---

# Atomic CSS' Logarithmic Growth Curve

With other CSS approaches you see a linear growth curve: $O(n)$

With atomic CSS, the growth curve looks more logarithmic: $O(\log \, n)$

<img src="/img/linear-vs-logarithmic.png" alt="linear vs logarithmic chart">

---

# In Addition

Many ways to define styles:

```js {all|1-4|5-8|9-14}
// DOM Styles object
const first = css({
  padding: '5px'
})
// CSS string or template literal
const second = css(`
  padding: 5px;
`)
// Callback function returning either of the above
const third = css(() => {
  return {
    padding: '5px'
  }
})
```

---

# In Addition

Support for variants and media queries

```js {all|3-6|7-9,14|10-13}
const classList = css({
  color: 'purple',
  // variants
  '&:hover': {
    color: 'blue',
  },
  // media queries
  '@media only screen and (min-width: 576px) and (max-width: 768px)': {
    color: 'green',
    // both
    '&:hover': {
      color: 'red',
    },
  },
})
```

---

# In Addition

We can define a configuration file with design tokens and/or predefined styles

```js {all|3|5-9}
export default {
  tokens: {
    favoriteColor: 'purple',

    btnStyles: {
      padding: '5px',
      background: 'black',
      color: 'white'
    }
  }
}
```

---

With the callback syntax, we can reference those tokens

```js {all|2-4|8}
const highlightedText = css((tokens) => {
  return {
    color: tokens.favoriteColor
  }
})

const buttonClasses = css((tokens) => {
  return tokens.btnStyles
})
```

---

We can also extend predefined styles

```js {all|3-4,9-10}
const btnClassesSmall = css((tokens) => {
  return {
    ...tokens.btnStyles,
    padding: '2px'
  }
})
const btnClassesLarge = css((tokens) => {
  return {
    ...tokens.btnStyles,
    padding: '10px'
  }
}
```

---

# TypeScript Intellisense

Great for productivity, catching errors, & onboarding

<img src="/img/intellisense.jpg" alt="intellisense autosuggestions" width="400">

---
layout: statement
---

# Nothing Is Perfect

This is a CSS-in-**JS** tool, so although it **is** framework-agnostic, it is not **language** agnostic.

---
layout: statement
---

# Thanks 😘

More details at [particlescss.com](https://particlescss.com)