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
