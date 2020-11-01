# Message Format Syntax

Vue I18n can use message format syntax to localize the messages to be displayed in the UI. Vue I18n messages are interpolations and messages with various feature syntax.

Under the hood, The messages written in message format syntax are compiled into message functions with Vue I18n message compiler. Message functions and caching mechanism to maximize performance gains.

If you prefer the raw power of JavaScript, you can also [directly write message functions](../advanced/function) instead of messages.

## Interpolations

Vue I18n supports interpolation using placeholders `{}` like "Mustache".

### Named interpolation

The Named interpolation can be interpolated in the placeholder using variable names defined in JavaScript.

As an example, the following locale messages resource:

```js
const messages = {
  en: {
    message: {
      hello: '{msg} world'
    }
  }
}
```

The locale messages is the resource specified by the `messages` option of `createI18n` function. It is defined `en` locale with `{ message: { hello: '{msg} world' } } }`.

Named interpolation allows you to specify variables defined in JavaScript. In the locale message above, you can localize it by giving the JavaScript defined `msg` as a parameter to the translation function.

The following is an example of the use of `$t` in a template:

```html
<p>{{ $t('message.hello', { msg: 'hello' }) }}</p>
```

The first argument is `message.hello` as the key to locale messages resource, and the second argument is an object with `msg` property as a parameter to `$t` function.

:::tip NOTE
The locale message resource key for the translate function can be specified for a specific resource with `.` (dot), just like a JavaScript object.
::::

:::tip NOTE
`$t` function has some overloads. About these overloads, see the here.
:::

As result, the below:

```html
<p>hello world</p>
```

### List interpolation

The List interpolation can be interpolated in the placeholder using an array defined in JavaScript.

As an example, the following locale messages resource:

```js
const messages = {
  en: {
    message: {
      hello: '{0} world'
    }
  }
}
```

It is defined `en` locale with `{ message: { hello: '{0} world' } } }`.

List interpolation allows you to specify array defined in JavaScript. In the locale message above, you can be localized by giving the `0` index item of the array defined by JavaScript as a parameter to the translation function.

The following is an example of the use of `$t` in a template:

```html
<p>{{ $t('message.hello', ['hello']) }}</p>
```

The first argument is `message.hello` as the key to locale messages resource, and the second argument is an array that have `'hello'` item as a parameter to `$t` function.

As result, the below:

```html
<p>hello world</p>
```

### Literal interpolation

The Literal interpolation can be interpolated in the placeholder using literal string.

As an example, the following locale messages resource:

```js
const messages = {
  en: {
    address: "{account}{'@'}{domain}"
  }
}
```

It is defined `en` locale with `{ address: "{account}{'@'}{domain} }`.

Literal interpolation allows you to specify string literal that is quoted with single quotation `’`. The message specified with literal interpolation is localized **as the string** by the translation function.

The following is an example of the use of `$t` in a template:

```html
<p>email: {{ $t('address', { account: 'foo', domain: 'domain.com' }) }}</p>
```

The first argument is `address` as the key to locale messages resource, and the second argument is an object with `account` and `domain` property as a parameter to `$t` function.

As result, the below:

```html
<p>email: foo@domain.com</p>
```

:::tip NOTE
Literal interpolation is useful for special characters in message format syntax, such as `{` and `}`, which cannot be used directly in the message.
:::

## Linked messages

TODO:

### Modifiers

TODO:

### With interpolations

TODO:

## Special Characters

The following characters used in the message format syntax are processed by the compiler as special characters:

- `{`
- `}`
- `@`
- `$`
- `|`

If you want to use these characters, you will need to use the [Literal interpolation](#literal-interpolation).

## Rails i18n format

Vue I18n supports the message format that is compatible with [Ruby on Rails i18n](https://guides.rubyonrails.org/i18n.html).

You can interpolate message format syntax with `%` prefixing:

:::danger Important!!
In v10 and later, Rails i18n format will be deprecated. We recommended using Named interpolation.
:::

As an example, the following locale messages resource:

```js
const messages = {
  en: {
    message: {
      hello: '%{msg} world'
    }
  }
}
```

It is defined `en` locale with `{ message: { hello: '%{msg} world' } } }`.

As with [Named interpolation](#named-interpolation), you can specify variables defined in JavaScript. In the locale message above, it is possible to localize it by giving a JavaScript defined `msg` as a parameter to the translation function.

The following is an example of the use of `$t` in a template:

```html
<p>{{ $t('message.hello', { msg: 'hello' }) }}</p>
```

As result, the below:

```html
<p>hello world</p>
```

## HTML Message

You can localize it with messages that contain HTML.

:::danger Danger
:warning: Dynamically localizing arbitrary HTML on your website can be very dangerous because it can easily lead to XSS vulnerabilities. Only use HTML interpolation on trusted content and never on user-provided content.

We recommended using the [Component interpolation](../advanced/component).
:::

:::warning Notice
If the message contains HTML, Vue I18n outputs a warning to console when `process.env.NODE_ENV !== 'production'`, Vue I18n outputs  warning to console.

You can control warning output with the `warnHtmlInMessage` or `warnHtmlMessage` options in `createI18n` function or `useI18n` function.
:::

As an example, the following locale messages resource:

```js
const messages = {
  en: {
    message: {
      hello: 'hello <br> world'
    }
  }
}
```

It is defined `en` locale with `{ message: { hello: 'hello <br> world' } } }`.

The following is an example of the use of `v-html` and `$t` in a template:

```html
<p v-html="$t('message.hello')"></p>
```

As result, the below:

```html
<p>hello
<!--<br> exists but is rendered as html and not a string-->
world</p>
```