
# idonttrustlikethat-fast-check

**idonttrustlikethat-fast-check** is a plugin designed for [fast-check](https://fast-check.dev/). It convert [idonttrustlikethat](https://github.com/AlexGalays/idonttrustlikethat) validators to **fast-check** arbitraries. Allowing the possibility to use idtlt validators with fast-check.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

## Installation

Run the following command in your terminal:

```bash
npm install idonttrustlikethat-fast-check       # npm

yarn add idonttrustlikethat-fast-check          # yarn

bun add idonttrustlikethat-fast-check           # bun

pnpm add idonttrustlikethat-fast-check          # pnpm
``````

## Basic usage

Creating a simple string fast-check arbitrary from a idtlt validator:

```typescript
import fc from 'fast-check'
import { string } from 'idonttrustlikethat'
import { inputOf } from 'idonttrustlikethat-fast-check'

const stringArbitrary = inputOf(string)

fc.assert(fc.property(stringArbitrary, (v) => {
    // Examples of generated values: "JT>\"C9k", "h]iD\"27;", "S", "n\\Ye", ""…

    return string.validate(v).ok
}))
```

## Examples
Here's a basic example demonstrating the usage of `inputOf`:

```typescript
import fc from 'fast-check'
import { object, array, string, isoDate, boolean, union, literal } from 'idonttrustlikethat'
import { inputOf } from 'idonttrustlikethat-fast-check'

// Define a validator
const validator = array(
  object({
    id: string,
    article: array(
      object({
        id: string,
        author: string,
        created: isoDate,
        tag: union(literal('bunny'), literal('pony'), literal('fox')),
        content: string,
        published: boolean
      })
    ),
  })
)

// Generate an arbitrary based on the validator
const arbitrary = inputOf(validator)

fc.assert(fc.property(arbitrary, (v) => validator.validate(v).ok))

```

Example of generated values:

```
[
  {
    "id": "",
    "article": [
      {
        "id": "5)zKi)\\",
        "author": "C_+]J/y<",
        "created": "-178555-05-21T08:34:36.335Z",
        "tag": "fox",
        "content": "}6f8z",
        "published": false
      }
    ]
  },
  {
    "id": "pr",
    "article": [
      {
        "id": ")s",
        "author": ":.p+G",
        "created": "+245858-09-13T13:56:40.856Z",
        "tag": "fox",
        "content": "19\\vYI.",
        "published": true
      },
      {
        "id": "{+<~o?$m",
        "author": " r",
        "created": "-171760-01-02T20:31:07.117Z",
        "tag": "bunny",
        "content": "dMY(sMG",
        "published": true
      },
      {
        "id": "!H:uO(",
        "author": ")]z9Z22;",
        "created": "-120713-12-31T15:25:27.309Z",
        "tag": "fox",
        "content": "j2cA,zb",
        "published": true
      },
      {
        "id": "w1+g",
        "author": "$",
        "created": "+239963-03-06T02:44:13.390Z",
        "tag": "fox",
        "content": "4p<n=V",
        "published": false
      },
      {
        "id": "jp,,/+W",
        "author": "4uj",
        "created": "-067112-09-29T19:48:43.189Z",
        "tag": "pony",
        "content": "LdH?",
        "published": false
      },
      {
        "id": "a5",
        "author": "l`( ",
        "created": "+014697-02-02T20:12:04.692Z",
        "tag": "fox",
        "content": "?Ry\"Y5CO",
        "published": true
      },
      {
        "id": "|_wJ=/{rP",
        "author": "5j{*\"`L~",
        "created": "-000763-05-03T22:01:18.003Z",
        "tag": "pony",
        "content": "<,^<8v",
        "published": true
      },
      {
        "id": "3g,v",
        "author": "3",
        "created": "+158617-10-22T18:09:03.570Z",
        "tag": "fox",
        "content": "MKt\\D@<1d",
        "published": true
      },
      {
        "id": "&@-P:",
        "author": "o{/CsZ",
        "created": "-090627-02-01T01:52:45.085Z",
        "tag": "bunny",
        "content": "DL=Y",
        "published": false
      }
    ]
  },
  {
    "id": "bind",
    "article": [
      {
        "id": "!",
        "author": "=MP_=2\\0M",
        "created": "1970-01-01T00:00:00.045Z",
        "tag": "pony",
        "content": "",
        "published": true
      },
      {
        "id": "{s'+",
        "author": "%5{#Q9y\"8",
        "created": "-128929-10-24T23:19:39.271Z",
        "tag": "pony",
        "content": "to",
        "published": false
      },
      {
        "id": "O5[:Y",
        "author": ")tj^\\*=",
        "created": "-087446-08-04T09:19:28.489Z",
        "tag": "pony",
        "content": "f(:oY-%",
        "published": true
      }
    ]
  }
]
```

In this example, arbitrary will be a randomly generated array that satisfies the specified validation rules.

## Warning

### Tuple

**Tuple** validator only support primitive validator:

`unknown, string, number, boolean, null, undefined`

### Unsupported validators

The following validators are not supported:

```
- discriminatedUnion
- default
- and
- then
- recursion
- minSize
- url
- booleanFromString
- numberFromString
- intFromString
```

## Contributing

Contributions are welcome! If you find a bug or have a feature request, please open an issue. If you'd like to contribute code, please fork the repository and submit a pull request.

# License

This project is licensed under the MIT License - see the LICENSE file for details.
