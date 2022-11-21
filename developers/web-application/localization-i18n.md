---
description: >-
  The locale setting is located in Web Settings on the admin section. Currently
  English and Portuguese languages are supported.
---

# Localization (i18n)

Text localization can be done using the Live own `i18n` service.

## Import

```javascript
import i18n from 'live/services/i18n'
```

## Registering translations

```jsx
import locale from 'live/init/global-locale'
import i18n from 'live/services/i18n'
import pt_br from './i18n/pt_br'
import en_us from './i18n/en_us'

var localeFiles = {
    pt_br: pt_br,
    en_us: en_us
}

// register current locale file
i18n.add(localeFiles[locale])
```

## Locale files

{% code title="i18n/pt_br.js" %}
```jsx
export default {
    values: {
       book: "livro"
    }
}
```
{% endcode %}

## Parameters, pluralization, functions

Translations can received simple parameters that will be expanded for each locale string

{% code title="i18n/pt_br.js" %}
```javascript
export default {
    values: {
        'Hi %{user} welcome to our book store': 
            'Olá %{user} bem vindo à nossa loja de livros'
    }
}
```
{% endcode %}

The parameters can also be used for a very simple pluralization logic

{% code title="i18n/en_us.js" %}
```javascript
export default {
    values: {
        '%{count} books were added to cart': [
            [1, 1, 'a book was added to cart'],
            [2, 99, '%{count} books were added to cart'],
            [100, null, '99+ books were added to cart']
        ]
    }
}
```
{% endcode %}

Translation values can be `functions` that will be rendered as React components

{% code title="i18n/en_us.js" %}
```jsx
export default {
    values: {
       'book-count': function BookCount ({ count }) {
           if (count === 0) return <span style={{color: 'red'}}>There are no books</span>
           if (count === 1) return <span style={{color: 'green'}}>There is one book</span>

           return <span> There are {count} books </span>
       }
    }
}
```
{% endcode %}

## API

```typescript
type i18nString = (translation: string) => string

// passing props to function values
type i18nComponent = 
    (translation: string, props: Record<string, any>) => React.ReactNode

type i18n = i18nString | i18nComponent
```

## Usage

```jsx
function SimpleExample ()  { return <span>{i18n('book')}</span> }

function Greetings() { 
  return 
    <span>
      {i18n('Hi %{user} welcome to our book store', { user: 'John Doe' })}
    </span> 
}

function CartInfo() {
  return <span>{i18n('%{count} books were added to cart', { count: 123 })}</span>
}

function BookCounter () {
  const [ count, setCount ] = React.useState(0)

  return <div onClick={() => setCount(s => s + 1)} >{i18n('book-count', { count })}</div>
}
```
