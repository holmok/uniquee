# Uniquey

Uniquey _(you-KNEE-key)_ is a node library that generates random unique strings **(or keys)**.

## Why?

Yeah, there are UUIDs, and we mostly use v4 (random) ones. But we usually store these 128bit keys as TEXT in a database, which is 32 characters which are larger than 128 bits. So rather than hex, why not base62 (A-Za-z0-9) or even any alphabet? How about always eight characters? A Base62 alphabet and eight characters will give you a total of 2.18\*10^14 unique keys, pretty good.

## Are Collisions Possible?

Yes, but you can make it less possible by adjusting the length to 22, and you will get a slightly less chance of a collision as UUID v4. With the bonus of only being 22 characters (or even fewer with a larger alphabet).

## How random?

Yeah, there are a bunch of less than good ways to generate random stuff in node/javascript. The base of this library uses `Crypto.randomFillSync` to allocate a bunch of random bytes. This process is similar to how the `UUID` library generates v4 UUIDs.

## Is it blocking? Is it fast?

Yes, and it depends. On my M1 MacBook Air, I can generate 1,000,000 keys in less than 300ms. That seems fast, and was with the default settings. Your performance may be different.

## Multi-Byte Characters?

Ugh, well this was fixed in version 1.1, sorry. So a multi-byte character (like 🏢) will be split as a whole character as it's self and not each byte. Support for multi-byte characters needs to be turned on explicitly with the `multiByteCharacters` option in the constructor.

---

## How to Use

**Install it with**

`yarn add uniquey`

or

`npm i uniquey`

**Sample code:**

```javascript
import Uniqee from 'uniquey'
// or const Uniqee = require('uniquey').default

const uniquey = new Uniquey()

console.log(uniquey.create())

// outputs random string like `HqiId0Sj`
```

---

## Options

You can create a **Uniquey** instance with the following optional options:

- `length` [number]: _(default: 8)_ the length of string to create. Must be greater than 0 and less than `allocation`.
- `characters` [string]: _(default: '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ')_ the characters to use in the random output. Must be at least 2 characters and no more than 256 and not have any duplicates.
- `allocate` [number]: _(default: 256)_ number of random bytes to allocate. This allocation helps with performance. Must be greater than `length` and greater than 0.
- `multiByteCharacters` [boolean]: _(default: false)_ allow multi-byte characters, if true they will be allowed, if not the creation will throw an error if `characters` contains multi-byte characters.

**Example using options:**

```javascript
import Uniquey from 'uniquey'
// or const Uniquey = require('uniquey').default

const uniquey = new Uniquey({
  length: 4,
  characters: '🐈🏢🐕🐎',
  allocate: 4000,
  multiByteCharacters: true
})

console.log(uniquey.create())

// outputs random string like `🐎🐈🏢🐕`
// not very random but fun!
// also this will allocate new random bytes
// after generating 1000 random keys (4000/4)
```

## New in Version 1.1.0

Better handling of multi-byte characters.
