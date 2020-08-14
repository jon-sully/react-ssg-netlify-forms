# React Netlify Form

A drop-in all-in-one solution for getting Netlify Forms on React-based SSGs
(Gatsby, Next, etc..) including automatic honeypot protection 💯📝

## Premise

The `<NetlifyForm>` is a turn-key solution to enable your Gatsby, Next, etc. site
with Netlify forms without having to worry about all of the details. Manage your
form's state however you prefer (a simple method shown below) then pass the value
collection to the component and let it handle the rest.

No need to configure anything in the Netlify UI, no need to worry about fetching
or endpoints. It's all included.

## Example

Simple form usage (https://react-netlify-form.demo.jon.fm)
```jsx
import React, { useState } from "react"
import { navigate } from 'gatsby'
import NetlifyForm from 'react-netlify-form'

const IndexPage = () => {

  // Post-Submit Navigate
  const postSubmit = () => {
    navigate('/hooray')
  }

  // Simple controlled form setup (Control your own state)
  const handleChange = e => setFormValues({ ...formValues, [e.target.name]: e.target.value })
  const [formValues, setFormValues] = useState({
    name: '',
    message: ''
  })

  return (
      <NetlifyForm formName="Very Simple Form" formValues={formValues} postSubmit={postSubmit} >
        <div>
          Your Name: <input type="text" name="name" value={formValues.name} onChange={handleChange} required />
        </div>
        <div>
          Message: <textarea name="message" value={formValues.message} onChange={handleChange} required />
        </div>
        <div>
          <button type="submit">Send</button>
        </div>
      </NetlifyForm>
  )
}
```

## Install

Install is straightforward as can be. Install via Yarn or NPM:

Yarn: `yarn add react-netlify-form`

NPM: `npm i react-netlify-form`

Then import into your React code where desired (shown above): `import NetlifyForm from 'react-netlify-form'`

## Usage

The `<NetlifyForm>` component operates on the following premise and allows for a number of configuration options.

The Component will handle all of the plumbing and leg work to make sure that your
form data arrives to Netlify safe and sound, but it's up to you to manage the state
of your form controls. The Component expects you to pass an object in for `formValues`
with a flat structure, ready to be serialized. For instance:

```js
{
  name: 'Hello there, friend!',
  email: 'test@example.com',
  description: 'What a beautiful day it is'
  favoriteColor: 'red'
}
```

This is the natural progression if using the example above where form state is managed in a single object (which also makes having a general `onChange` handler possible 😁). The key reason for this is so that you can employ your own validations and other UX flow concerns with the form data at your fingertips. This is better illustrated in a [slightly more complicated example](https://github.com/jon-fm/react-netlify-form-demo/blob/master/src/pages/medium.js).

  /* 

  - Automatic Honeypot ✅
  - Pull this form in and have a functioning, awesome Netlify form. Literally
    no other plumbing required.

  API: 
    formName
    formValues
    
  Optional:
    preSubmit (async ready, return true, else no-op)
    postSubmit (no return necessary)
    automaticHoneypot

  */

  // Complicated stuff to keep things well-hidden during runtime (and from bots)
  // but exposed for netlify build bots
