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
 
 With this, we are done creating the sign-up form. Now we can move on to  step 2 which is creating the progress bar.
 
 
## Step 3- Set the Password Complexity Progress Bar

In this section, you are going to create a progress bar under the password field, that will indicate the strength of the user’s password to them. 
The progress bar should be able to change colors and length when the password becomes more complex. 
For instance, if the user inputs only numbers, the progress bar should take a particular color and length, if they make it more complex by adding lower case letters, then the length of the progress bar should increase and the color should change also. 

To add a progress bar to your project, create a file inside your `Components` folder and call it `ProgressBar.js`. Inside the `ProgressBar.js` file,  you have to create a progress bar with the code below:

````javascript

/**./Components/ProgressBar.js*/

import React from 'react'
const ProgressBar = () => {
  return (
    <>
        <div className='progress'>
            <div className='progress-bar' 
            style={{
                width:"70%",
                background:'red'
            }}></div>
        </div>
    </>
  )
}
export default ProgressBar
````

With the code above, you created a `ProgressBar` component.  Inside that component you returned a progress bar class from bootstrap. 
Then you gave it an inline style and you hardcoded the width of the progress bar and also set the background color to red.

Now import the ProgressBar component inside the LoginForm.js file and then render it in your LoginForm component with the code below:

````javascript

/**Components/LoginForm.js*/

.......

```
import ProgressBar from './ProgressBar'
```

const LoginForm = () => {
.......

<div className='relative'>
  <input 
    type={(showPassword === false)? 'password' : 'text'}
     placeholder='enter a strong passwod...'
     className='w-full rounded-lg h-10'
    />
    
    ```
    <ProgressBar />
    ```
    
    <p className='text-white'>Error</p>
...........
````

Save the LoginForm.js and ProgressBar.js files and open the browser, you should see a progress bar under the password field, something like the image below:

![Screenshot 2023-02-07 at 18 10 35](https://user-images.githubusercontent.com/77885988/218888354-b5044f46-02c0-4f78-acba-7e94a872131e.jpeg)

Now, you can make the width and the color of the progress dynamic, in a way that they change based on the user’s password complexity. So modify your `ProgressBar` component to have the code below

````javascript

/**ProgressBar.js*/

import React from 'react'

const ProgressBar = () => {

```
 const setProgressBar = () =>({
        width: '75%',
        background: 'red',
        height:'7px'
  })
```

 return (
    <>
      <div className='progress' 
      ```style={{height: '7px'}}```>
        <div className='progress-bar' 
          ```style={setProgressBar()}```>
          </div>
      </div>
    </>
  )
}
export default ProgressBar
````

With the code above, you created a function called `setProgressBar` and passed the width, height and background properties to it. Now the `function`, when called will give the progress bar all these properties.
So, on line 15 you called this function and used it to apply styles to the progress bar.

Now to make the width and the color of the progress bar dynamic, in a way that it will change based on the user’s password complexity, you have to define a few regular expressions and then use these regular expressions to to create a function that will change the progress bar color.

But before that, you also need to create a `password` state, in the `LoginForm` component and pass it down as props to the `ProgressBar` component, since the password input field is in the `LoginForm` component. 
Add the code below to the `LoginForm.js` file:

````javascript

/**Components/LoginForm.js*/

..........
const LoginForm = () => {
/** Show or hide Password state */
const [showPassword, setShowPassword] = useState(false)

```
/** Password state */
const [password, setPassword ]= useState('')
```

..............
<div className='mx-5 '>
  {/** Password creation field */}
  <div className='relative'>
    <input 
      type={(showPassword === false)? 'password': 'text'}
      placeholder='enter a strong password...'
      className='w-full rounded-lg h-10'
      
      ```
      onChange={e => setPassword(e.target.value)}
      ```
      
    />
    
    ```
    <ProgressBar  password={password}/>
    ```
    
    <p className='text-white'>Error</p>
...............
````

With the code above, you created a `password` state and a `setPassword` function with `useState` hook, then in line 18 you used an `onChange` event to set the value of the `password` state to the user’s input. Then you finally passed the `password` state as `prop` to the `ProgressBar` component. 

With this you can destructure the `password` props in the `ProgressBar` component and use it for your regular expression pattern. 
So modify your `ProgressBar.js` file to have the following code:

````javascript

/**Components/ProgressBar.js*/

import React from 'react'


```
const ProgressBar = ({password}) => {
    
   const pattern1 =/(?=.*[a-z])/
    const pattern2 = /(?=.*[a-z])(?=.*[A-Z])/
    const pattern3 =  /(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])/
    const pattern4 = /(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[@!_%&*])/

    const result1 = pattern1.test(password)
    const result2 = pattern2.test(password)
    const result3 = pattern3.test(password)
    const result4 = pattern4.test(password)
    let num;
  
    if(result1 === true){
        num = 25
    }if(result2 === true){
        num = 50
    }if(result3 === true){
        num = 75
    }if(result4 === true){
        num = 100
    }

 const ProgressBarColor = ()=>{
        switch(num){
            case 0:
                return "#FFFFFF"
            case 25:
                return "#FF0000"
            case 50:
                return '#ffa500'
            case 75: 
                return '#90EE90'
            case 100: 
                return "#00FF00"
            default:
              return 'none'
        }
    }
 ```
 
    const setProgressBar = () =>({
    
    ```
        width: `${num}%`,
        background: ProgressBarColor(),
     ```
        height:'7px'
    })
...............
````

Basically what you did was to destructure the `password` prop, then create 4 regular expression patterns. 
The first pattern will check if the password has a lowercase letter, the second pattern checks if the password has an uppercase letter and a lower case letter combined, the third pattern checks if the password has a lowercase letter, an uppercase letter and a number, then finally the fourth pattern will check if the password has a lowercase letter, an uppercase letter, a number and any special symbol.

Then you used the `test(  )`  method to test the `password` prop with these patterns. So in line 12 you tested the password if it has a lowercase letter, in line 13 you tested the password if it has a lowercase letter and an uppercase letter, in line 14 you tested if the password has a lowercase letter and an uppercase letter and a number then in line 15 you tested the password if it has a lowercase letter, an uppercase letter, a number, and a special symbol.

Now in line 16 you have declared a variable called `num`. The value of the `num` variable will be determined by the ability of the password to pass each test.

So from line 18 to line 26 you used the `if` conditional statement to see if the `password` passed a test and then assign a value to the num variable.

So the `if` block from line 18 -20 will assign a value of 25 to the `num` variable if the `password` has a lower case letter, the `if` block from line 20 to line 22 will assign a value of 50 to the `num` variable if the `password` has a lowercase letter and an uppercase letter, the `if` block from line 22 to line 24 will assign a value of 75 to the `num` variable if the `password` has a lowercase letter, an uppercase letter and a number, then the `if` block on line 24 to line 26 will assign the `num` variable a value of 100 if the `password` has a lowercase letter, an uppercase letter, a number and a special character.

Now with the value of this `num` variable, you created a function that will change the color of the progress bar. 
Inside this function you use a `switch` conditional statement to set the color of the progress bar based on the num value. So if the `num` variable is 25, the progress bar should have a color of red, if it’s 50,  the progress bar should have an orange color, if it’s 75 the progress bar should have a light green color then if it is 100 the progress bar should have a green color.

Then finally on line 45, you used the `num` value to set the width of the progress bar,  and on line 46 you passed the `ProgressBarColor` function as a value for the background color. This will give the progress bar a color based on the switch statement from the function.

Now if you save the `LoginForm.js` and `ProgressBar.js` files and open the browser. You should still see your sign up form, but this time the color of your progress bar will be white:

![Screenshot 2023-02-07 at 18 34 25](https://user-images.githubusercontent.com/77885988/218891961-47d2a386-1a3e-4fe6-b42f-45787caf2638.jpeg)

