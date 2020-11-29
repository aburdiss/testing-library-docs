---
id: example-react-formik
title: Formik
---

Example based in this
[Async Submission Formik example](https://formik.org/docs/examples/async-submission)

```jsx
// myForm.js
import React from 'react'
import { Formik, Field, Form } from 'formik'

const sleep = ms => new Promise(r => setTimeout(r, ms))

export const MyForm = ({ onSubmit }) => {
  const handleSubmit = async values => {
    await sleep(500)
    onSubmit(values)
  }

  return (
    <div>
      <h1>Sign Up</h1>
      <Formik
        initialValues={{
          firstName: '',
          lastName: '',
          email: '',
        }}
        onSubmit={handleSubmit}
      >
        <Form>
          <label htmlFor="firstName">First Name</label>
          <Field id="firstName" name="firstName" placeholder="Jane" />

          <label htmlFor="lastName">Last Name</label>
          <Field id="lastName" name="lastName" placeholder="Doe" />

          <label htmlFor="email">Email</label>
          <Field
            id="email"
            name="email"
            placeholder="jane@acme.com"
            type="email"
          />
          <button type="submit">Submit</button>
        </Form>
      </Formik>
    </div>
  )
}
```

```jsx
// myForm.test.js
import React from 'react'
import { render, screen, waitFor } from '@testing-library/react'
import userEvent from '@testing-library/user-event'

import { MyForm } from './myForm.js'

test('rendering and submiting a basic Formik form', async () => {
  const handleSubmit = jest.fn()
  render(<MyForm onSubmit={handleSubmit} />)

  userEvent.type(screen.getByLabelText(/first name/i), 'Jhon')
  userEvent.type(screen.getByLabelText(/last name/i), 'Dee')
  userEvent.type(screen.getByLabelText(/email/i), 'jhon.dee@someemail.com')

  userEvent.click(screen.getByRole('button', { name: /submit/i }))

  await waitFor(() =>
    expect(handleSubmit).toHaveBeenCalledWith({
      email: 'jhon.dee@someemail.com',
      firstName: 'Jhon',
      lastName: 'Dee',
    }, expect.anything())
  )
})
```