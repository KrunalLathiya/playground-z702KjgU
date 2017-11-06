# Redux Form Validation Tutorial

Redux Form Validation Tutorial Example From Scratch is today’s leading topic. The underlying implementation of redux-form is simple. However, to make the most of it, it is recommended to have basic knowledge on:
1. <b>Redux</b> state container
2. <b>React</b> and <b>Higher-Order Components (HOCs)</b>.
To connect your React form components to your Redux store, you will need following pieces from the redux-form  package:
1. Redux Reducer: form reducer
2. React HOC reduxForm() and <Field /> component.
First, we need to setup the <b>Redux Store</b> and then pass to the main component. Also, we need to install some redux related libraries to up and running with this project.

# Step 1: Setup the project and install dependencies.

Hit the following commands one by one to set up the boilerplate.

```
npm install -g create-react-app

create-react-app reduxformvalidation

npm i -D redux react-redux redux-form
```
Now, type the following command.

```
npm start
```

If any error occurs, please hit the following command.
```
npm install
```
Then, again try the following command, it will open the http://localhost:3000 URL in the browser.
We need to style the form components, so here I have used Bootstrap 4 Forms.

```
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" integrity="sha384-PsH8R72JQ3SOdhVi3uxftmaW6Vc51MKb0q5P2rRUpPvrszuE4W1povHYgTpBfshb" crossorigin="anonymous">
    <title>Redux Form Validation</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

# Step 2: Create a redux store.

Create one store and pass wrap with the <b>Provider</b> component.

```javascript
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';
import { reducer as formReducer } from 'redux-form';
import App from './App';

const rootReducer = combineReducers({
    form: formReducer
});

const store = createStore(rootReducer);

ReactDOM.render(
<Provider store={store}>
 <App />
</Provider>
,document.getElementById('root'));
```

# Step 3: Create only FormCode.js file.

Inside <b>src</b> directory, make one file called <b>FormCode.js</b> file. To make your form component communicate with the store, we need to wrap it with <b>reduxForm()</b>. It will provide the props about the form state and function to handle the submission process.

```javascript
// FormCode.js

import React from 'react';
import { Field, reduxForm } from 'redux-form';

const renderField = ({ input, label, type, meta: { touched, error, warning } }) => (
    <div>
      <label className="control-label">{label}</label>
      <div>
        <input {...input} placeholder={label} type={type} className="form-control" />
        {touched && ((error && <span className="text-danger">{error}</span>) || (warning && <span>{warning}</span>))}
      </div>
    </div>
  )

let FormCode = props => {
  const { handleSubmit, pristine, submitting } = props;
  return (
    <form onSubmit={ handleSubmit }>
      <div className="form-group">
        <Field name="firstName" component={renderField} label="First Name" />
      </div>
      <div className="form-group">
        <Field name="lastName" component={renderField} label="Last Name" />
      </div>
      <div className="form-group">
        <Field name="email" component={renderField} label="Email" />
      </div>
      <div className="form-group">
        <button type="submit" className="btn btn-primary">Submit</button>
      </div>
    </form>
  )
}
FormCode = reduxForm({
  form: 'contact'
})(FormCode);

export default FormCode;
```
<b>FormCode.js</b> is just a web form component with its form field. We have designed the form and its error, but we have still not write any validation logic in it. Just plain Bootstrap Form.

# Step 4: Include this form into React component.

```javascript
// App.js

import React from 'react';
import FormCode from './FormCode';

class App extends React.Component {
  submit = (values) => {
    alert("submitted");
    console.log(values);
  }
  render() {
    return (
      <div className="container">
        <h3 className="jumbotron">Redux Form Validation</h3>
        <FormCode onSubmit={this.submit} />
      </div>
      
    )
  }
}

export default App;
```

Now, switch to the browser, you can see the bootstrap form ready.  If you try to submit the form then, it will not validate any field because we have not write any validation logic and you can see the alert that said: “submitted.”

# Step 5: Write and add validation logic.

In <b>FormCode.js</b> file, write the following code.

```javascript
// App.js

const validate = values => {
    const errors = {}
    if (!values.firstName) {
      errors.firstName = 'Required'
    } else if (values.firstName.length < 2) {
      errors.firstName = 'Minimum be 2 characters or more'
    }
    if (!values.email) {
      errors.email = 'Required'
    } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
      errors.email = 'Invalid email address'
    }
    if (!values.lastName) {
        errors.lastName = 'Required'
      } else if (values.lastName.length < 2) {
        errors.lastName = 'Minimum be 2 characters or more'
      }
    return errors
  }
```
Now, we need to pass this function into the <b>FormCode.js</b> component. Since it is HOC component, it is straightforward to do so.

```javascript
// App.js

FormCode = reduxForm({
  form: 'contact',
  validate,
})(FormCode);

export default FormCode;
```
So, our complete <b>FormCode.js</b> file looks like this.

```javascript
// FormCode.js

import React from 'react';
import { Field, reduxForm } from 'redux-form';

const validate = values => {
    const errors = {}
    if (!values.firstName) {
      errors.firstName = 'Required'
    } else if (values.firstName.length < 2) {
      errors.firstName = 'Minimum be 2 characters or more'
    }
    if (!values.email) {
      errors.email = 'Required'
    } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
      errors.email = 'Invalid email address'
    }
    if (!values.lastName) {
        errors.lastName = 'Required'
      } else if (values.lastName.length < 2) {
        errors.lastName = 'Minimum be 2 characters or more'
      }
    return errors
  }

const renderField = ({ input, label, type, meta: { touched, error, warning } }) => (
    <div>
      <label className="control-label">{label}</label>
      <div>
        <input {...input} placeholder={label} type={type} className="form-control" />
        {touched && ((error && <span className="text-danger">{error}</span>) || (warning && <span>{warning}</span>))}
      </div>
    </div>
  )

let FormCode = props => {
  const { handleSubmit, pristine, submitting } = props;
  return (
    <form onSubmit={ handleSubmit }>
      <div className="form-group">
        <Field name="firstName" component={renderField} label="First Name" />
      </div>
      <div className="form-group">
        <Field name="lastName" component={renderField} label="Last Name" />
      </div>
      <div className="form-group">
        <Field name="email" component={renderField} label="Email" />
      </div>
      <div className="form-group">
        <button type="submit" disabled={pristine || submitting} className="btn btn-primary">Submit</button>
      </div>
    </form>
  )
}
FormCode = reduxForm({
  form: 'contact',
  validate,
})(FormCode);

export default FormCode;
```
Finally, our <b>Redux Form Validation Tutorial Example</b> From Scratch is over.

@[Redux Form Validation Tutorial]({"stubs": ["src/index.tpl.html", "src/app/app.jsx", "src/main.js", "src/app/FormCode.js"], "command": "./run.sh"})

