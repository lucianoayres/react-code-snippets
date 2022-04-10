# UncontrolledForm Component

Basic example of an uncontrolled form without using the useRef hook.

## JavaScript

[View it on CodeSandbox](https://codesandbox.io/s/react-uncontrolled-form-example-ooyyn9?file=/src/UncontrolledForm.js:0-1782 'React uncontrolled form code snippet')

```javascript
import { useState } from 'react'

export default function UncontrolledForm() {
  const [hasUserTyped, setHasUserTyped] = useState(false)
  const [shouldDisableSubmit, setShouldDisableSubmit] = useState(true)

  let shouldHideError = !hasUserTyped

  const handleSubmit = (event) => {
    event.preventDefault()
    const email = event.target.elements.email.value

    // replace with your HTTP POST request
    alert(`Success! Request sent with email: ${email}`)

    clearForm(event.target, 'email')
  }

  const handleValidate = (event) => {
    const email = event.target.value

    if (validateEmail(email)) {
      setShouldDisableSubmit(false)
      setHasUserTyped(false)
    } else {
      setShouldDisableSubmit(true)
      setHasUserTyped(true)
    }
  }

  // this is a contrived example of email validation,
  // do not use on production
  const validateEmail = (email) => {
    var reg = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/
    return email.match(reg)
  }

  const clearForm = (form, field) => {
    form.elements[field].value = ''
    setShouldDisableSubmit(true)
    setHasUserTyped(false)
  }

  return (
    <div
      style={{
        maxWidth: 800,
        margin: 'auto',
        width: '90vw',
        padding: '24px 0 0 80px'
      }}
      className="App"
    >
      <form onSubmit={handleSubmit}>
        <h1>Uncontrolled Form</h1>
        <p>Subscribe to our newsletter</p>
        <div style={{ display: 'flex', gap: '10px' }}>
          <input
            type="email"
            name="email"
            placeholder="E-mail"
            onChange={handleValidate}
            required
          />
          <button type="submit" disabled={shouldDisableSubmit}>
            Subscribe
          </button>
        </div>
        <p style={{ color: 'red', margin: '0' }} hidden={shouldHideError}>
          <small>Insert a valid e-mail address</small>
        </p>
      </form>
    </div>
  )
}
```
