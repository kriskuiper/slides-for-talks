---
theme: apple-basic
highlighter: prism
lineNumbers: true
info: |
  ## We've built a component library,
  why you should too
drawings:
  enabled: false
---

---
layout: intro
---
# We've built a component library

Why you should too

<span class="text-sm">Component-driven development in Vue en/of React</span>

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: intro-image-right
image: '/images/kris-pf.jpeg'
---

# Over mij
- 👨‍💻 Chef component library @ LOAVIES
- 🎓 Cum laude afgestudeerd aan CMD Amsterdam in 2021

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: quote
---

# Waar deze talk over gaat
- Wat een component library is
- Hoe je het kan implementeren
- Waarom het een meerwaarde kan vormen binnen je tech stack
- Hoe je als agency een component-library kan adopteren
- Learnings, zodat jij niet dezelfde fuck-ups hoeft te maken

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: quote
---

# Deze talk is NIET
- Een vergelijk tussen Vue en React
- Alleen maar gericht is op JavaScript frameworks
- Een dwingend praatje over dat je een component library **moet** gebruiken

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: quote
---

# Wie heeft er wel eens een component library gebruikt?
Zo ja, welke dan?

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: quote
---
# Het start allemaal met een design system
Of een bestaande site / bestaande sites

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: quote
---
# Die zet je om naar herbruikbare componenten
Zodat je het op elk project kan gebruiken

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: intro-image-right
image: /images/ikea-billy.jpg
---
# De onderdelen van een component library
- De componenten in code (Vue/React etc.)
- Tests voor je componenten
- Een live playground (bijv. Storybook)
- Documentatie (liefst bij de playground)

<footer class="absolute bottom-12">
  <span class="text-sm font-300 flex items-center">🛍️ loavies.com/nl</span>
  <span class="text-sm font-300 flex items-center my-1"><logos-github-octocat /> github.com/kriskuiper</span>
</footer>

---
layout: intro-image-right
image: 'https://media.giphy.com/media/4JVTF9zR9BicshFAb7/giphy.gif'
---
# Hoe bouw je dat dan?
- Het opsplitsen van je UI
- De anatomie van de library en een component
- Omgaan met vormgeving en CSS
- Testing tips
- Gebruik en distributie van je library

---
layout: image
image: /images/loavies-ui.png
---

---
layout: image
image: /images/componenten-voorbeeld.png
---

---
layout: quote
---

# De anatomie van de LOAVIES component library en een component

```bash
/src
  /assets
    /css # Alle 'globale' CSS, dit gaat naar een aparte CSS file samen met alle CSS van de componenten
      -> utilities.css # Dingen als een .sr-only class
      -> variables.css # Alle CSS variabelen
    /scss
      -> breakpoints.scss # Om te gebruiken bij alle projecten
  /components
    /component-name
      -> component-name.stories.js # Storybook stories
      -> component-name.test.js # Tests
      -> component-name.vue # Alle src code voor het component
```

---
layout: quote
---
# Omgaan met vormgeving van je component
- Probeer outer margins niet te definiëren, dat wil je later bepalen als je de context weet
- Gebruik eens CSS variabelen, dit maakt de vormgeving van je component een stuk flexibeler

---
layout: quote
---
```css
/* assets/css/variables.css */
:root {
  --spacing-small: 12px;
  --spacing-default: 16px;
  --spacing-medium: 24px;
  --spacing-large: 32px;
}
```

```js
// nuxt.config.js
export default {
  // ..
  css: [
    '@loavies/component-library/dist/loavies-component-library.css',
    '~/assets/css/index.scss',
  ],
  // ..
}
```
```css
/* ~/assets/css/index.scss */
:root {
  --spacing-small: 16px;
  --spacing-default: 24px;
  --spacing-medium: 32px;
  --spacing-large: 40px;
}
```

---
layout: quote
---

```css
/* library -> app-button.vue */
.app-button {
  width: 100%;
  max-width: var(--app-button-width, 244px);
}
```

---
layout: quote
---
```html
<!-- project -> ander componentje -->
<app-button class="my-awesome-button">
  Klik!
</app-button>
```

```css
.my-awesome-button {
  --app-button-width: 500px;
}
```

---
layout: quote
---

# 🧪 Testing tips
- Schrijf je tests met de gebruiker in het achterhoofd, focus je dus niet teveel op implementatie details maar op wat een gebruiker hoort te zien
- Normaliseer zoveel mogelijk in constants wanneer je bijv. props validatie hebt zodat je dit indien nodig weer in je test kan gebruiken

---

```js {1-7|9|11}
// beforeEach, mounten van component etc.
it('shows details after clicking on summary', async () => {
  const button = wrapper.find('button')

  await button.trigger('click')

  const content = wrapper.find('.app-accordion__content')

  expect(content.classes()).toContain('app-accordion__content--show')

  expect(content.isVisible()).toBe(true)
})
```

---
layout: image
image: /images/storybook.png
---

---
layout: quote
---

```js {2,5,7,10,12,15}
// ..
it('can contain multiple child elements', () => {
  expect(wrapper).toContainChildElement('.first-child')
  expect(wrapper).toContainChildElement('.second-child')
})

it('renders the same amount of pagination dots as there are child nodes', () => {
  const pagination = wrapper.find('.app-swiper__pagination')
  expect(pagination.element.children.length).toEqual(2)
})

it('first dot has active styling by default', () => {
  const firstPaginationItem = wrapper.find('.app-swiper__pagination-item:first-child')
  expect(firstPaginationItem).toContainClassName('app-swiper__pagination-item--active')
})
// ..
```

---
layout: intro-image-right
image: 'https://media.giphy.com/media/135zStc5hvtaMg/giphy-downsized-large.gif'
---
# Distributie en gebruik

---
layout: section
---

# Waar zou je logischerwijs een component library publiceren?

<v-click>

`npm` natuurlijk 💡 

</v-click>

---
layout: image
image: /images/npm.png
---

---
layout: image
image: /images/github-actions.png
---

---
layout: quote
---

```js
import { AppButton } from '@loavies/component-library'

// En we gebruiken 'm zoals normaal
export default {
  components: {
    AppButton,
  }
}
```

---
layout: intro-image-right
image: /images/component-library-little-view.png
---

# De meerwaarde in je tech stack

<v-click>

- Bouwen van nieuwe projecten is op de lange termijn veel minder werk

</v-click>

<v-click>

- Bugs hoef je maar op één plek te fixen

</v-click>

<v-click>

- Je bouwt je componenten een stuk simpeler zonder vrijwel enige business logica

</v-click>

---
layout: section
---

# Hoe kan je dit toepassen als agency?

---
layout: image
image: /images/volkswagen-logo.jpeg
---

---

# Eén base platform, meerdere merken / modellen
<v-click>

- De Volkswagen Golf, Seat Leon, Audi A3 en Škoda Octavia

</v-click>


<v-click>

- Vormgeving compleet anders

</v-click>

<v-click>

- Ze zijn namelijk gebouwd op hetzelfde (MQB 🤓) platform

</v-click>

---

# Eén library met base componenten
- Kale UX van componenten is vaak hetzelfde, ongeacht de vormgeving

<v-click>

- Vormgeving kan je achteraf bepalen, standaard UX out-of-the-box

</v-click>

<v-click>

- Voorbeeld: een Dialog/Pop-up/Modal

</v-click>

---

<video src="/videos/logiq-video.mp4" controls></video>

---

# Idealiter is een component in delen beschikbaar

```js
export {
  DialogRoot,
  DialogContent,
  DialogTrigger,
  DialogCloseButton,
}
```

<v-click>

- Vergemakkelijkt flexibiliteit van layout

</v-click>

---

# Recap
<v-click>

- Breek je front-end(s) op in kleinere stukken/componenten
- Agency? Strip zoveel mogelijk vormgeving eruit

</v-click>

<v-click>

- Stop ze in een losse component library die op NPM staat
```js
import { MyComponent } from 'my-component-library'
```

</v-click>

<v-click>

- Profit! 💸

</v-click>

---
layout: section
---

# Joejoe! 👋

🖥️ [kris-kuiper.nl](https://www.kris-kuiper.nl)

🦸 [careers.loavies.com](https://careers.loavies.com/)

🛍️ [LOAVIES component library](https://loavies-component-library.herokuapp.com/)

🧱 [Logiq component library source code](https://github.com/kriskuiper/graduation-project)

---