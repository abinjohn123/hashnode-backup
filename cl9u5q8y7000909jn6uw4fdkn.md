# Date & Number Formatting Has Never Been Easier

## Introduction

Formatting dates and numbers is a tricky task in JavaScript. For starters, the native `Date` object doesn't have a method to obtain the month in words as `date.getMonth()` only provides a zero-based number equivalent to the month(0 - 11). Working with dates gets more cumbersome when tasked with displaying a date on a page, where the developer has to take care of the formatting - YY or YYYY, DD/MM/YY or MM/DD/YY, slashes instead of dashes, and so on.

Hard-coding date and number formats are fine if the users are from the same locality or region, but what if you get users from other regions or those using different languages? Here in India, dates are formatted as DD/MM/YY, while in the US it is MM/DD/YY. Numbers are formatted differently as well. Though subtle, these differences can make or break the user experience.

Luckily, JavaScript introduced the Internationalization API towards the end of 2012 to tackle this issue. Here's what the 'Scope' of the API specification states: ***This Standard defines the application programming interface for ECMAScript objects that support programs that need to adapt to the linguistic and cultural conventions used by different human languages and countries.***

## The Intl Object

The internationalization API (also known as i18n API, which stands for i - 18 characters - n, of the word internationalization) provides the `Intl` object that offers three constructors:
* `Intl.Collator` - for language-sensitive string comparisons. [An amazing read on Intl.Collator](https://learn.coderslang.com/javascript-interview-50-intl.collator/)
* `Intl.DateTimeFormat` - for locale-based date & time formatting
* `Int.NumberFormat` - for locale-based number formatting

We'll be dealing with the Date-Time and Number formatting constructors in this post.

## Formatting Date and Time

```js
const formattedDate = new Intl.DateTimeFormat(locale, options).format(date)
```

- **locale**(optional) is the language code in ISO format
- **options**(optional) is an object that specifies how different components of the date and time are to be formatted
- **date** is a date object or ISO date string.

Here's the constructor in action
```js
const locale1 = 'en-US'
const locale2 = 'de-DE'

const options1 = {
  hour: 'numeric',
  minute: 'numeric',
  day: 'numeric',
  month: 'long',
  year: 'numeric',
  weekday: 'short',
}

const options2 = {
  day: 'numeric',
  month: 'numeric',
  year: 'numeric',
  weekday: 'long',
}

const date = new Date();

const formatted1 = new Intl.DateTimeFormat(locale1, options1).format(date);
const formatted2 = new Intl.DateTimeFormat(locale2, options2).format(date);

console.log(formatted1); // Sat, October 29, 2022 at 11:28 AM
console.log(formatted2); // Samstag, 29.10.2022
```

Notice how passing in different locales changed the date format and the language, and how the options object modified how the different components of the dates were represented.

All that is good, but how do we know which locale or language a user is using? Formatting the dates differently for users from different locales is the point, right?

We have the `navigator.language` method for that which grabs the locale directly from the browser, which is usually its set language. A common approach is to use the logical OR operator to set a default value in case the browser locale is not obtained.
```js
  const locale = navigator.language || 'en-US'
```

Here are some commonly used **options** properties and their possible values

| property   | values |
| ---------- | -------|
| weekday | "narrow", "short", "long"|
| era | "narrow", "short", "long" |
| year | "2-digit", "numeric"|
| month| "2-digit", "numeric", "narrow", "short", "long" |
| day | "2-digit", "numeric" |
| hour | "2-digit", "numeric"  |
| minute | "2-digit", "numeric" |
| second | "2-digit", "numeric" |

## Formatting Numbers

Number and currency formatting is similar to the date formatting, with the only difference being the options that we pass in.

```js
const formattedNumber = new Intl.NumberFormat(locale, options).format(number)
```
- **locale**(optional) is the language code in ISO format
- **options**(optional) is an object that specifies how different components of the date and time are to be formatted
- **number** is the number to be formatted

Here's a basic demo
```js
const sixDigitUS = new Intl.NumberFormat('en-US').format(100000.5678);
const sixDigitIN = new Intl.NumberFormat('en-IN').format(100000.5678);

console.log(sixDigitUS) //"100,000.568"
console.log(sixDigitIN) //"1,00,000.568"
```
The options object offers further customization of the output. Here are a few commonly used properties and their possible values.

| property   | values |
| ---------- | -------|
| notation | "standard", "scientific", "engineering", "compact" |
| signDisplay | "auto", "always", "exceptZero", "negative", "never" |
| style | "decimal", "currency", "percent", "unit" |
|currency (style must be `currency `)| ISO 4217 country code ([a list of codes](https://www.xe.com/iso4217.php))|
| currencyDisplay (style must be `currency `) | "symbol", "narrowSymbol", "code", "name" |
|unit (style must be `unit` ) | single or combination of values specified [here](https://tc39.es/proposal-unified-intl-numberformat/section6/locales-currencies-tz_proposed_out.html#sec-issanctionedsimpleunitidentifier)|
| unitDisplay (style must be `unit` ) | "long", "short", "narrow" |
| roundingMode | "ceil", "floor", "trunc" |

Here's how numbers get formatted in different ways when passed in different *options* properties.

```js
const option1 = {
  style: 'unit',
  unit: 'liter'
}

const option2 = {
  notation: 'scientific',
  style: 'unit',
  unit: 'meter-per-second'
}

const option3 = {
  style: 'currency',
  currency: 'INR',
  currencyDisplay: 'narrowSymbol'
}

console.log(new Intl.NumberFormat('en-IN', option1).format(100)) // "100 l"
console.log(new Intl.NumberFormat('en-US', option2).format(1000)) // "1E3 m/s"
console.log(new Intl.NumberFormat('en-IN', option3).format(100000.34)) // "₹1,00,000.34"
```
## Support For The API

The API has good support across desktop and mobile browsers and has a 97.88% percentage of support as per [Can I Use](https://caniuse.com/internationalization) at the time of writing. So using the API is a safe bet.

## Conclusion

The internationalization API is, in my opinion,  the easiest and best way to format dates and numbers. It's hassle-free, quick, and highly customizable. Let me know what you think! Feel free to share your thoughts as comments or hit me up on [Twitter](https://twitter.com/abin_john98). I'd love to chat!
