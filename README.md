# react-validate-framework

A lightweight and extensible React validation component

[![npm](https://img.shields.io/npm/v/react-validate-framework.svg?style=flat-square)](https://www.npmjs.com/package/react-validate-framework)
[![npm](https://img.shields.io/npm/dt/react-validate-framework.svg?style=flat-square)](https://github.com/MinJieLiu/react-validate-framework)

## How to use?

    npm i react-validate-framework --save

This component relies on `react`.

Demo: [https://minjieliu.github.io/react-validate-framework](https://minjieliu.github.io/react-validate-framework)

You can check out the code to see examples.

> BasicForm.jsx

```js
import React, { Component, PropTypes } from 'react';
import formConnect from 'react-validate-framework';

// Rules and messages
const schemas = {
  email: {
    rules: 'required | isEmail | maxLength(32)',
    messages: 'Can not be empty! | Please enter a valid email address. | Can not exceed {{param}} characters.',
  },
  phone: {
    rules: 'isPhone',
    messages: 'Mobile: {{value}} is not valid.',
  },
  hobby: {
    rules: 'required | selectLimit(2)',
    messages: 'Can not be empty! | Select at least {{param}}.',
  },
};

// Custom methods
const methods = {
  selectLimit(field, param) {
    if (Array.isArray(field.value)) {
      return field.value.length >= param;
    }
    return false;
  },
};

// A basic form
class BasicForm extends Component {

  static propTypes = {
    fields: PropTypes.object,
    formValues: PropTypes.object,
    isAllValid: PropTypes.bool,
    onChange: PropTypes.func,
    validate: PropTypes.func,
    validateByNames: PropTypes.func,
  };

  render() {
    const {
      fields,
      onChange,
      formValues,
      isAllValid,
    } = this.props;
    
    return (
      <div className="container">
        <h3>BasicForm</h3>
        <div className="form-group">
          <label htmlFor="email">Email:</label>
          <input
            className={fields.email.className}
            id="email"
            name="email"
            type="email"
            onChange={onChange}
            value={fields.email.value}
            placeholder="Please input your email"
          />
          <em className="valid-error-message">{fields.email.message}</em>
        </div>
        <div className="form-group">
          <label htmlFor="phone">Phone:</label>
          <input
            className={fields.phone.className}
            id="phone"
            name="phone"
            type="text"
            onChange={onChange}
            value={fields.phone.value}
            placeholder="Please enter phone number"
          />
          <em className="valid-error-message">{fields.phone.message}</em>
        </div>
        <div className="form-group">
          <label htmlFor="hobby">Hobby:</label>
          <div className="checkbox">
            <label htmlFor="hobby1">
              <input
                id="hobby1"
                name="hobby"
                type="checkbox"
                onChange={onChange}
                checked={fields.hobby.value.includes('1')}
                value="1"
              />
              hobby1
            </label>
            <label htmlFor="hobby2">
              <input
                id="hobby2"
                name="hobby"
                type="checkbox"
                onChange={onChange}
                checked={fields.hobby.value.includes('2')}
                value="2"
              />
              hobby2
            </label>
            <label htmlFor="hobby3">
              <input
                id="hobby3"
                name="hobby"
                type="checkbox"
                onChange={onChange}
                checked={fields.hobby.value.includes('3')}
                value="3"
              />
              hobby3
            </label>
          </div>
          <em className="valid-error-message">{fields.hobby.message}</em>
        </div>
        <input
          className="btn btn-primary"
          id="submit"
          type="button"
          onClick={this.handleSubmitClick}
          value={isAllValid ? 'Success' : 'Commit'}
        />
        <div className="well-sm">
          <p>Form Values:</p>
          {JSON.stringify(formValues)}
        </div>
      </div>
    );
  }
}

export default formConnect(schemas, methods)(BasicForm);

```

> App.jsx

```js
import React, { Component } from 'react';
import BasicForm from './BasicForm';

export default class extends Component {

  state = {
    formValues: {
      email: '',
      phone: '',
      hobby: ['2'],
    },
  };
  
  render() {
    return (
      <BasicForm
        ref={(ref) => {
          this.basicForm = ref;
        }}
        classNames={{
          static: 'form-control',
          success: 'valid-success',
          error: 'valid-error',
        }}
        values={this.state.formValues}
      />
    );
  }
}

```

### API

#### FormControl params

| name | type | required | default | description |
| :--- | :--- | :--- | :--- | :--- |
| values | Object | true | | Key-value pairs for `name` and` value` |
| classNames | Object | false | {} | Its `key` value contains` static`, `success`,` error` |

#### Form params

| name | type | default | description |
| :--- | :--- | :--- | :--- |
| fields | Object | | The collection of fields |
| isAllValid | Boolean | | Gets the global validation status |
| formValues | Object | | Gets a list of form values |
| onChange | function | | Form change event listener |
| operateChange | function | | Customize to change the field |
| validate | function | | Validate all fields |
| validateByNames | function | | Validate the component through names |
| addFields | function | | Add one or more fields |
| removeFields | function | | Deletes one or more fields |
| addSchemas | function | | Add one or more validation rules |
| removeSchemas | function | | Delete one or more validation rules |
