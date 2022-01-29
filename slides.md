---
theme: default
font:
  sans: 'Manrope'
lineNumbers: false
layout: statement
---

# Using Atomic CSS-in-JS to improve DX and UX
## (which I hope this is more interesting than it sounds)

---
layout: statement
---

<img src="/img/css-is-awesome.png" class="mx-auto" width="400" alt="CSS is Aweomse meme">

---

# Challenges With CSS

<v-clicks>

- 🌍 Global nature, cascade, source order
- 💥 Class collisions
- 👓 Specificity struggles
- 👯 Duplicated styles
- ➕ Append-only stylesheets
- 👻 Unused styles
- ⚔ Flexibility is a double-edge sword
- 🤡 Non-cohesive styles & magic numbers

</v-clicks>

---

# My Journey

<v-clicks>

- Plain ol' CSS 
  - 🤷‍♂️
- Frameworks (Bootstrap)
  - Prescribed HTML and CSS classes
- Conventions (BEM)
  - Follow naming rules to avoid specificity battles (`header__logo--dark`)
- Atomic/Utility CSS (Tailwind)
  - Each CSS rule has its own class
  - Classes are composed to create desired styles (`<div class="p-2 text-center bg-red">`)
- CSS-in-JS (observational)
  - Library generates unique selectors and styles (`.dwetdcgwr {...}`)
  - JavaScript injects CSS rules at runtime
  
</v-clicks>

---

# Frameworks

Pros ✅

<v-clicks>

- Extensive component options
- Consistent, nice styling
- Faster bootstrapping/prototyping

</v-clicks>

Cons ⛔

<v-clicks>

- Unused styles
- Requires strict markup 
- Knowledge tied to library
- Customization was pretty meh

</v-clicks>

---

# Conventions

Pros ✅

<v-clicks>

- Complete flexibility
- No specificity battles
- Some semblance of where the code exists

</v-clicks>

Cons ⛔

<v-clicks>

- No prescribed approach
- Knowledge tied to in-house system
- Code and styles live far apart

</v-clicks> 

---

# Atomic CSS

Pros ✅

<v-clicks>

- Limited options; fewer decisions
- Safe to add/remove styles
- No repeated CSS rules
- No specificity issues

</v-clicks>

Cons ⛔

<v-clicks>

- More looking at the docs
- Knowledge tied to library
- Unused styles OR slower builds *

</v-clicks>

---

# Tailwind Specifically

<v-clicks>

- Awesome predefined design tokens (colors, fonts, spacing)
- Removes unused style with additional build steps
- Haven't used JIT compiler yet

</v-clicks>

---

# CSS-in-JS

Pros ✅

<v-clicks>

- Theming & design token support
- Flexibility of plain CSS
- Knowledge tied to native platform
- Only generate styles you ask for
- Co-location of styles and components
- Scoped styles

</v-clicks>

Cons ⛔

<v-clicks>

- Repeated styles (blue button vs red button)
- Requires transpiler
- JavaScript runtime dependency

</v-clicks>

---
layout: image
image: /img/ariel.jpg
---

# How I feel right now

---

<div class="grid grid-cols-2">
<div>

<v-click>

## What I have

- Gadgets & gizmos: plenty
- Whozits & whatzits: galore
- Thingamabobs: 20

</v-click>

</div>
<div>

<v-click>

## What I want

More

</v-click>

</div>
</div>

---
layout: statement
---

# I'll tell you what I want

---

# What I Really, Really Want

<v-clicks>

- Knowledge tied to platform (plain ol' CSS)
- Fewer decisions & better cohesions with predefined design tokens (atomic CSS)
- Easy to customize as needed (CSS-in-JS)
- Avoid specificity battles (atomic CSS)
- Co-located styles for easier maintenance (CSS-in-JS)
- Autocomplete & type checking (TypeScript)
- Only build what I use (CSS-in-JS)
- Static assets. Better perf (plain ol' CSS)
- No repeated styles / logarithmic growth curve (atomic CSS)
- People who appreciate a good Spice Girls reference (me)

</v-clicks>

---
layout: statement
---

<img src="/img/bob.png" width="200" class="mx-auto -my-4" alt="bob the builder">

# Let's build it.

---

# How It Could Work: Development

Import a function called `css` 

Pass styles into the function 

Use results function as class names

---

# How It Could Work: Development

JavaScript:
```js {all|1|3-6|9}
import { css } from 'prtcls'

const buttonStyles = css({
  padding: '5px',
  color: 'blue'
})

export default () => {
  return <button className={buttonStyles}>Click me</button>
}
```

CSS:
```css
```

---

# How It Could Work: Transpilation

Transpiler removes the `css` import. 

Replaces the function with static list of classes.

Injects CSS rules into the build pipeline.

---

# How It Could Work: Transpilation

JavaScript:
```js {all|1|3|6}
// no more library import

const buttonStyles = 'p_5px c_blue' // string of atomic classes

export default () => {
  return <button className={buttonStyles}>Click me</button>
}

```

CSS:
```css {none|all}
.p_5px {
  padding: 5px;
}
.c_blue {
  color: blue;
}
```

---
layout: statement
---

# Anything Else?

---

# More Input Options

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

# Variants & Media Queries

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

# Reusable Design Tokens

Configuration:
```js {all|3|4-7|none}
export default {
  tokens: {
    favoriteColor: 'purple',
    btnStyles: {
      padding: '5px',
      borderRadius: '4px'
    }
  }
}
```

JavaScript:
```js {none|all|2|5}
const highlightedText = css((tokens) => `
  color: ${tokens.favoriteColor};
`)
const buttonClasses = css((tokens) => {
  return tokens.btnStyles
})
```

---

# Reuse & Extend Style Groups

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

<div class="grid grid-cols-2">

- Productivity
- Catching errors
- Discoverability 
- Onboarding

<img src="/img/intellisense.jpg" alt="intellisense autosuggestions" width="400" class="mx-auto">
</div>

---
layout: statement
---

# Benefits

---

# Developers: Less Thinking & More Time

<v-clicks>

- Looking up documentation (besides CSS)
- Coming up with the right class names
- Remembering a growing list of classes 
- Worrying about scoping or collisions
- Translating between projects (just copy/paste)

</v-clicks>

---

# Users: Better Performance & Less Data

<img src="/img/linear-vs-logarithmic.png" alt="linear vs logarithmic chart" class="mx-auto">

<p class="text-blue-700">

Without atomic CSS: $O(n)$ <br>
Application grows -> More CSS -> More duplication -> Linear growth

</p>

---

# Users: Better Performance & Less Data

<img src="/img/linear-vs-logarithmic.png" alt="linear vs logarithmic chart" class="mx-auto">

<p class="text-green-700">

With atomic CSS: $O(\log \, n)$ <br>
Application grows -> More reusability -> Fewer new -> Logarithmic growth

</p>

---
layout: statement
---

# What's The Catch?

Requires transpilation, so although it can be **framework** agnostic, it cannot be **language** agnostic.

---
layout: statement
---

# Good thing this is a <span class="p-2 bg-yellow-300">JS</span> conf!!!

---
layout: statement
---

<img src="/img/prtcls.svg" alt="Particles CSS" width="500" class="mb-8 mx-auto">

An atomic CSS-in-JS solution that only ships the styles you use with no runtime dependency.

More details at [particlescss.com](https://particlescss.com)

---
layout: statement
---

# Thanks 😘

<pepicons-internet/> [austingil.com](https://austingil.com)

<logos-twitter/> [@heyAustinGil](https://twitter.com/heyAustinGil)

<logos-twitch/> [@heyAustinGil](https://twitch.tv/heyAustinGil)

<bi-github/> [@AustinGil](https://github.com/AustinGil)
