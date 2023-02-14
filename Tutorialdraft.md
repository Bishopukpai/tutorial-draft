# How To Prevent Broken Authentication in React.js with Password Complexity Check Method

## Introduction

Broken Authentication Attack is one of the most popular and viral web application vulnerability attacks .

Broken authentication attacks can occur in different ways. Also, there are different factors and loopholes in a web application that can make it  vulnerable to this attack. 
And allowing users to make use of weak and easy-to-guess passwords is a big factor. 
This makes it possible for attackers to easily gain access to accounts with weak password credentials by guessing and using a credential stuffing approach. 
Therefore, it is important to put a password checker in place, that will make sure that the password provided by users during registration is strong enough.

In this tutorial, we will build a React application that uses a password checker to prevent users from creating accounts with weak passwords. 
We’ll also integrate a progress bar into our application that will indicate the strength of the password to the user.

At the end of this tutorial, you will understand the requirements for building a secured React application that protects against broken authentication attacks.

## Prerequisites

To follow this tutorial, you will need the following:

1. Install a stable version of [Node.js](https://nodejs.org/en/) on your computer.  If you don’t have Node.Js on your machine, you can follow the instructions in this tutorial, [How To Install Node.js and Create Local Development environment on macOs](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-and-create-a-local-development-environment-on-macos) or the installation using a  PPA section in [How To Install Node.js in Ubuntu 18.0](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04) to install Node.js on macOS or Ubuntu 18.0.

1. Basic knowledge of React.Js, like creating a new React project, modifying the React project, and creating components. There is a tutorial on [How To SetUp React Project With Create React App](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-react-project-with-create-react-app#prerequisites) that will be very helpful in getting this knowledge.

## Step 1 - Installing Necessary Packages and Dependencies

In this step you are going to install all the packages and dependencies you need for building and styling your application. 
You will need to install and use Bootstrap for styling the password indicator progress bar. You also need to install react-hook-form. This particular package will help implement the password complexity check rules that will prevent account creation if the password of the user is weak. Then you also need tailwindcss for styling the application and then finally, you will have to install react-icons. The react-icons package will basically add icons to your form and make it look nicer and formal.

You can start by installing react-icons and react-hook-form first with the command below:

```bash

npm install react-icons react-hook-form

#OR

yarn add react-icons react-hook-form
```

The command above will install react-icons and react-hook-form packages in your application.
Once the installation is successfully completed, you should see something like this in your terminal screen:

```bash

added 2 packages, and audited 1474 packages in 6s

232 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

You can now install the tailwindcss package. To install tailwinds to react.js, you will need to follow these steps:
First, you have to install the package from npm with the command below:

```bash
npm install -D tailwindcss
```

The command above will install tailwindcss as a devDependency.  Once the installation process is done you should see something like this on your terminal screen:

```bash

up to date, audited 1474 packages in 3s

232 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

You can now go ahead and generate a tailwind.config.js file with the command below:

```bash
npx tailwindcss init
```

The code above will give the following response on the terminal screen:

```bash
Created Tailwind CSS config file: tailwind.config.js
```

It will also create a `tailwind.config.js` file for you. If you check the root of your project you will see the tailwind.config.js file. 

The next step you can take is to add the paths of all your template files in the tailwind.config.js file you just created with the command, by adding the code below inside the tailwind.config.js file:

````javaScript

/** tailwind.config.js*/
module.exports = {
```
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  
```
  theme: {
    extend: {},
  },
  plugins: [],
}
````

The code above will configure tailwind to implement all changes that occur inside any file in the `src` folder with a .js, .jsx, .ts or .tsx extension.

Then finally, you need to add  `@tailwind` directives inside your index.css file with the codes below:

```css

/** index.css*/

@tailwind base;
@tailwind components;
@tailwind utilities;
```

With these steps, you have successfully installed and  set up tailwindcss for your application. 
Now you need to install and set bootstrap up for your application as well. 
Run the command below to install bootstrap:

```bash

npm install bootstrap
#OR
yarn add bootstrap
```

The command above will install the most recent version of Bootstrap from npm into your project. 
Once you have a successful and complete installation, you should get a response similar to the one below on your terminal screen:

```bash

added 2 packages, and audited 1476 packages in 4s

234 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

You can now import Bootstrap into your index.js file which is the entry file of your project. 
Add the code below to the index.js file of your project:

```JavaScript

/** index.js */

import "bootstrap/dist/css/bootstrap.min.css";
```

The code above will import the Bootstrap CSS package into your application and you can make use of Bootstrap anywhere in your application.
With this, you have completed step 1 and installed all the packages required for this project. 

To make sure everything that was installed correctly, you can open the package.json file of your project. Under the `dependencies` section, you will `see react-icons`, `react-hook-forms` and `bootstrap`

````javaScript

/** package.json*/

"dependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
   
   ```
   "bootstrap": "^5.2.3",
   ```
   
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
   
   
   ```
    "react-hook-form": "^7.43.0",
    "react-icons": "^4.7.1",
    ```
    
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },
  ````

If you check under the devDependencies section you will see tailwindcss:

```javaScript

/**package.json*/

 "devDependencies": {
    "tailwindcss": "^3.2.4"
  }
  ```

## Step 2 - Creating a User Registration Form 

In this step, you will create a signup form that will have two input fields. The first field will collect user’s password then the second field will ask the user to re-enter the password they created, to confirm what they entered in the first field.

So, create a folder in the `src` folder of your project and call it `Components`. Inside the `Components` folder, you are going to create a file and call it `LoginForm.js`. The `LoginForm.js` file will contain the Registration form.

You can start by creating a `LoginForm` component inside the `LoginForm.js` file with the code below:

```javascript

/**LoginForm.js */

import React from 'react'

const LoginForm = () => {
  return (
    <div>
        
    </div>
  )
}
export default LoginForm
```

You now have to import the `LoginForm` component inside your `App.js` file.  Use the code below to import and return the `LoginForm` component inside the `App.js` file:

````javascript

/**App.js*/
```
import LoginForm from "./Components/LoginForm";
```
function App() {
  return (
  ```
     <LoginForm/>
   ```
    
  );
}
export default App;
````

Then, you will create a form with an input field to collect users' passwords inside the LoginForm.js file, with the code below:

