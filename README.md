# React SSG Netlify Form

A drop-in all-in-one solution for getting Netlify Forms on React-based SSGs
(Gatsby, Next, etc..) including automatic honeypot protection 💯📝

**Demo: https://react-ssg-netlify-forms.demo.jon.fm/**

## Premise

The `<NetlifyForm>` is a turn-key solution to enable your Gatsby, Next, etc. site
with Netlify forms without having to worry about all of the details. Manage your
form's state however you prefer (a simple method shown below) then pass the value
collection to the component and let it handle the rest.

No need to configure anything in the Netlify UI, no need to worry about fetching
or endpoints. It's all included.

## Example

Simple form usage (https://react-ssg-netlify-forms.demo.jon.fm)
```jsx
import React, { useState } from "react"
import { navigate } from 'gatsby'
import NetlifyForm from 'react-ssg-netlify-forms'

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

Yarn: `yarn add react-ssg-netlify-forms`

NPM: `npm i react-ssg-netlify-forms`

Then import into your React code where desired (shown above): `import NetlifyForm from 'react-ssg-netlify-forms'`

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
  description: 'What a beautiful day it is',
  favoriteColor: 'red'
}
```

This is the natural progression if using the example above where form state is managed in a single object (which also makes having a generic `onChange` handler possible 😁). The key reason for this is so that you can employ your own validations and other UX flow concerns with the form data at your fingertips. This is better illustrated in a [slightly more complicated example](https://github.com/jon-fm/react-ssg-netlify-forms-demo/blob/master/src/pages/medium.js).

**formName** - a _required_ prop - You may make this whatever you'd like - this is what will show up in the Netlify UI as the 'name' label for your form. This name is also what's referenced in the Subject field of Forms notifications emails.

**formValues** - a _required_ prop - This is the object that holds the form data / the values of the fields at time of submit event; described above

**preSubmit** - an _optional_ prop - A function that the `<NetlifyForm>` component will call when the user kicks off the submit event, before the content is sent to Netlify. This function _must_ return `true` for the submit to proceed, as returning `false` (or something non-truthy) results in a short-circuit no-op for the form. This may be an Async function. It will be `await`ed before submission, but still must return something truthy. This function will not receive arguments.

_A note: **preSubmit** is where you'd want to manifest any sort of form validations or checks you'd like to run. Please see [this example](https://github.com/jon-fm/react-ssg-netlify-forms-demo/blob/master/src/pages/medium.js) for an idea of what that might look like_

**postSubmit** - an _optional_ prop - A function that the `<NetlifyForm>` component will call _after_ submitting the form data to Netlify. This may be an Async function. This function will not receive arguments.

---

## Why?

Well, the short of it is that Netlify Forms is awesome but can be a tad tricky for the React-based SSGs. This package aims to take as much complexity away as possible. It generates the right code at build time then augments it differently at runtime and handles the submission process to Netlify servers for you.

Tl;dr: it makes life easier 🙂
