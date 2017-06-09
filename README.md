# workshopper-verify-processor

A verify processor that I've used in a few projects.

## Installation

```sh
npm install --save workshopper-verify-processor
```

## Usage

Pass the current exercise and a verification function to the output of this module.

```javascript
const exercise = require('workshopper-exercise')()
const verifyProcessor = require('workshopper-verify-processor')

exercise.addVerifyProcessor(verifyProcessor(exercise, async (test) => {
  await test.notEmpty(aList, 'translation_string', {
    key: 'value'
  })
}))
```

Your verification function will receive a `test` argument that is an object with various assertions.

Assertions can take promises as arguments and will return promises too.

The penultimate argument to an assertion is the name of a translation string.  A success message will be printed if the assertion is met otherwise an error message will be shown.

`pass.` or `fail.` will be prepended to your translation string so you should have a translation file similar to:

```javascript
{
  // ...
  "common": {
    "exercise": {
      "pass": {
        "translation_string": "This assertion passed with key {{key}}"
      },
      "fail": {
        "translation_string": "This assertion failed with key {{key}}"
      }
    }
  }
}
```

or per-exercise:

```javascript
{
  // ...
  "exercises": {
    "an_exercise": {
      "pass": {
        "translation_string": "This assertion passed with key {{key}}"
      },
      "fail": {
        "translation_string": "This assertion passed with key {{key}}"
      }
    }
  }
}
```

The final argument to the assertion is an object with key/value pairs that are used to do replacements in the translation string.

If an assertion fails, the test is aborted and an error message is shown.

### Adding custom validations

To add custom assertions, add them to `verifyProcessor.assertions`

Any promises passed as arguments to assertion functions will be automatically resolved before the assertion function is called.

Custom assertions can return either a boolean value or a promise that will eventually resolve to a boolean value.

```javascript
const verifyProcessor = require('workshopper-verify-processor')

verifyProcessor.assertions = Object.assign({}, verifyProcessor.assertions, {
  greaterThanOne: (left) => left > 1
})
```

Then later:

```javascript
exercise.addVerifyProcessor(verifyProcessor(exercise, async (test) => {
  await test.greaterThanOne(5, 'translation_string', {
    key: 'value'
  })
}))
```
