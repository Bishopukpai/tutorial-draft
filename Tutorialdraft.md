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

````javascript

/**LoginForm.js*/

......
const LoginForm = () => {
  return (
   <React.Fragment>
     <section>
       <form>
         <div 
          className='bg-black w-auto h-96 mt-20 rounded-lg mx-5'
          >
          {/** Header */}
            <div 
              className='flex items-center justify-center h-32'
            >
            <p 
  className='text-white uppercase text-4xl font-bold text-center'
             >
              Sign-Up To Create an Account
            </p>
            </div>
            {/** Body */}
            <div>
              <div className='mx-5 '>
                  {/** Password creation field */}
                    <div className='relative'>
                       <input 
                        type="password"
                        placeholder='enter a strong passwod...'
                        className='w-full rounded-lg h-10'
                        />
                      <p className='text-white'>Error</p>
                      </div>
                  </div>
                </div>
              </div>
            </form>
        </section>
   </React.Fragment>
  )
}
export default LoginForm
````

From line 6 to line 39 of the code above, you created a form with a single input field. On line 19 you added a header for your form and then you used tailwindcss to style it, the code from line 16 to line 20 will create an input  field with a placeholder and a type of password and also styled using tailwindcss.  
Then on line 32 you added an error message that will notify the user when they provide weak credentials that goes against your password creation rules. 
For now this error message is still hardcoded.

Now, you need to make the user’s experience better, by adding a feature that will either show the users their password while they type or hide their passwords, depending on their choices. 
To do this, you need to import two icons from from from the react-icons package you installed earlier. 

The first icon will appear if the user wants to hide their password while the second icon will appear if they want to show their password. So import these icons from the react-icons by adding the code below inside the LoginForm component:

````javaScript

```
import {AiFillEyeInvisible, AiFillEye} from 'react-icons/ai'
```

const LoginForm = () => {
  return (
   <React.Fragment>
      .......
     <p className='text-white'>Error</p>
     
     
     ```
          {/** Password view or hide */}
    <div className='text-2xl absolute top-2 right-5'>
       <AiFillEyeInvisible/>
       <AiFillEye/>
    </div>
    ```
    ........
   </form>
 </React.Fragment>
  )
}
export default LoginForm
````

With the code above, you imported `AiFilledEyeInvisible` and `AiFillEye` icons from `react-icons/ai` on line 3.
Then add the icons to your form field with the code from line 11 to line 14. 

The next feature you need to add to the form field is to make the password visible or hidden, to do this you have to create a state with `useState`, then use this `state` to create a condition to either display or hide the password. 
Add the following code to your `LoginForm` component to add this feature to your the input field:

````javascript

/**LoginForm.js*/

```
import React, {useState} from 'react'
```

import {AiFillEyeInvisible, AiFillEye} from 'react-icons/ai'


const LoginForm = () => {

```
{/** Show or hide Password state */}
const [showPassword, setShowPassword] = useState(false)
{/** Show Password onClick events */}
const handleShowPassword = () => {
    setShowPassword(!showPassword)
}
```
.......
   <div className='relative'>
     <input 
     
     ```
       type={(showPassword === false)? 'password' : 'text'}
      ```
      
        placeholder='enter a strong passwod...'
        className='w-full rounded-lg h-10'
       />
      <p className='text-white'>Error</p>
        {/** Password view or hide */}
      <div className='text-2xl absolute top-2 right-5'>
      
      ```
        {
          (showPassword == false)? 
           <AiFillEyeInvisible onClick={handleShowPassword}/>:  
           <AiFillEye onClick={handleShowPassword}/>
         }
      ```
      
      </div>
    </div>
    ..........
  )
}
export default LoginForm
````

With the code above, you imported the `useState` hook from `react` and then used it to create a `showPassword` state with an initial value of `false` on line 8. and also created a `setShowPassword` function that will reset the value of the `showPassword` state. 

Then from line 10 to line 12, you create a `handleShowPassword` function that will set the `showPassword` state to the previous value. 
That is, if the state was set to `true` before this function will set it to `false` and if it was `false` then it will set it to `true`.

Now on line  16, you used the value of the `showPassword` state to set the type of the input field. So, if the value of the `showPassword` state is `false`, then the input field type will be `password`, which means users won’t see what they are typing, but if the value is true, then the input type will be `text`.

Then finally, from line 23 to line 27, you use a `ternary` operator to display either the `AiFilledEyeInvisible` icon or the `AiFilledEye` icon depending on the value of the `showPassword` state. 
If the `showPassword` state is `true` your code will show the `AiFilledEye` icon, but if it is `false` your code will show the `AiFilledEyeInvisible` icon. To toggle the value of the `showPassword` state, you passed the `handleShowPassword` function to an `onClick` event in the `AiFilledEye` and `AiFilledEyeInvisible` icons

To wrap things up for this section, let’s create another input field. This input field will be used to confirm the password. 
You can add the following code to your `LoginForm` component in the `LoginForm.js` file:

````javascript

/**LoginForm.js*/

........
  {/** Password view or hide */}
    <div className='text-2xl absolute top-2 right-5'>
      {
      (showPassword == false)? 
        <AiFillEyeInvisible onClick={handleShowPassword}/>: 
        <AiFillEye onClick={handleShowPassword}/>
      }
     </div>
    </div>
    
    
```
    {/** Confirm Password creation field */}
    <div className='relative'>
      <input 
        type='password'
         placeholder='confirm your password'
         className='w-full rounded-lg h-10'
       />
       <p className='text-white'>Error</p>
    </div>
  {/** Submit Button */}
  <div className='flex items-center justify-center'>
    <input
      type="submit"
      value="submit"
      className='h-10 w-2/5 text-white rounded-lg font-bold bg-red-900'
    />
  </div>
```
..........
````

With the code above, you created a field for password confirmation. 
It’s actually the same code as the `password` collection field, it's just that in this case you are hard coding the input type as `password`. 
Then from line 23 to line 29 you created the `submit` button.  
Save the `LoginForm.js` file and run your application with:

```bash

#command Line 

npm start 
```

 Open your app in any browser of your choice and you should see a login form that looks like the one below.
 
 
 https://user-images.githubusercontent.com/77885988/218808316-3215016f-f318-42f8-abe4-a6335d553bcb.mov
