# JavaScrpt Project Template
The aim of this repository is to create a ready-to-use Javascript project with Eslint, Stickler and Webpack set up on it.

## Eslint

### Create a `package.json` file
This file is needed to install all the packages needed in the project.
To create the `package.json` file, run:
```
npm init
```
This will ask some question to set up the file like the author, package name, etc. You can accept the default parameters or add custome ones.
When you complete those steps, you will see the `package.json` file in the root of your project.

### Install ESlint
You can install Eslint using `npm` or `yarn`:
```
npm install eslint --save-dev
```
```
yarn add eslint --dev
```
This will create the `node-modules` folder and `package-lock.json` file.


Now create the `.eslintrc.json` configuration file with:
```
npx eslint --init
```
This will prompt a dialog with some question to configure the file. A sample configuration could be:
```
? How would you like to use ESLint?
  To check syntax only
  To check syntax and find problems
❯ To check syntax, find problems, and enforce code style
```
```
? What type of modules does your project use? (Use arrow keys)
❯ JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these
```
```
? Which framework does your project use?
  React
  Vue.js
❯ None of these
```
```
? Does your project use TypeScript? (y/N) N
```
```
? Where does your code run?
❯◉ Browser
 ◯ Node
```
```
? How would you like to define a style for your project?
❯ Use a popular style guide
  Answer questions about your style
  Inspect your JavaScript file(s)
```
```
? Which style guide do you want to follow? (Use arrow keys)
❯ Airbnb: https://github.com/airbnb/javascript
  Standard: https://github.com/standard/standard
  Google: https://github.com/google/eslint-config-google
```
```
? What format do you want your config file to be in?
  JavaScript
  YAML
❯ JSON
```

Depending if you followed the options above, ESlint will install some additional packages like `eslint-plugin-import` and `eslint-config-airbnb-base`.

### Set up `.eslintrc` file

When Eslint is installed, you will get a `.eslintrc` like this:
```
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "airbnb-base"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
    }
}
```
In this file we need add the `eslint:recommended` option in the `extends` parameter to get ESlint check our project.

### Add `.eslintignore` file
There are some files we don't want Eslint to check like `main.js` file generated by Webpack. So we can create this file in the project's root and add all the files (paths) we don't want Eslint to check. For intance, if we don't want to lint the `dist` folder handled by Webpack, we can put the following line in the `.eslintignore` file:
```
dist
```

## Stickler
Stickler set up involves 4 main actions:
* Enable Stickler app on GitHub
* Turn on GitHub repository on Stickler
* Add `.stickler.yml` file to the project root
* Add `.eslint.config` file to the project root

You can find the step by step set up [here](https://github.com/microverseinc/linters-config/tree/master/javascript#set-up-stickler-github-app---it-will-show-that-your-app-is-free-from-style-errors)

As additional note if you want Stickler to use the `.eslintrc` ESlint created to check your project locally, you can change the `config` attribute in the `.stickler.yml` file from `./eslint.config` to `./.eslintrc`:
```
linters:
  eslint:
    # replace
    config: './eslint.config'
    # with
    config: './.eslintrc'
```
## Webpack

### Installation
We need to install the `webpack` library, and if it is the version 4 or later, we need to install the `webpack-cli` also.
```
npm install --save-dev webpack
# Using an specific version:
npm install --save-dev webpack@<x.x.x>
```
```
npm install --save-dev webpack-cli
```
It is recommended to install these libraries locally (`--save-dev` flag) because it you install them globaly and there is breaking update all the projects using those globals will be affected.

### Basic set up
We are going to setup Webpack with a `webpack.config.js` file so the structure we will need is like this:
```
js-project-template
  |- package.json
  |- webpack.config.js
  |- /dist
    |- index.html
    |- main.js
  |- /src
    |- index.js
```
- The **`/src`** folder will contain all our work, the files we need to develop the application
- The **`/dist`** folder is where Webpack will place all the processed files from the /src folder

We can use a basic `webpack.config.js` like this:
```
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

The first step is to set up our `package.json` file to make our package private to prevent it to be published accidentally. The file will look like this:
```
{
    "name": "js-project-template",
    "version": "1.0.0",
    "description": "",
    "private": true,
    "main": "index.js", // remove this line
    ...
}
```
Now we need to add an html file in the root of our `dist` folder, we can call it `index.html` or the name you prefer, and in this file we are going to add a reference to a js file Webpack will create after build the code in the `src` folder. The `index.html` file will look like this:
```
<!doctype html>
<html>
 <head>
   <title>Getting Started</title>
 </head>
 <body>
  <!-- main.js file is create by Webpack after build the src folder -->
   <script src="main.js"></script>
 </body>
</html>
```
When we have ready our project with this structure the next step is to generate our `main.js` file. For this we need to create an **entry point** this will be a file in the root of the `src` folder that Webpack will build to generate the `main.js` file. To tell Webpack what is the entry point we will add it in the `webpack.config.js` file like this:
```
const path = require('path');

module.exports = {
  entry: './src/index.js', //this will be the entry point
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```
Now we can build the project using: `npx webpack` to let Webpack makes its magic. When it is done we will see the `main.js` file inside the `dist` folder.
You will get a warning after run the command:
```
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
```
You can can add the key `mode: 'none'` to the `webpack.config.file` like this:
```
...
module.exports = {
  mode: 'none',
  entry: './src/index.js',
...
}
````

### NPM scripts set up
We can set up some shortcuts to useful Webpack commands. For this, we will modifiy the `scritps: {}` key in the `package.json` file.

To create an script to `build` the application, we can use:
```
{
....
  "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "webpack"
    },
...
}
```
If we don't want to build the project each time we make modifications we can use the `--watch` option of `webpack` command. For this we need to add the following script in the `package.json` file.
```
{
....
  "scripts": {
      "watch": "webpack --watch"
  },
...
}
```
We also need a sever to run the application, we can install the `webpack-dev-server` npm packege to run our app in a development enviroment. To install it, we can run:
```
npm install webpack-dev-server --save-dev
```
And the, we can create an script in the `package.json` to run it using `npm run server`. The script looks like this:
```
{
....
  "scripts": {
      "server": "webpack-dev-server --open-page 'dist/'"
  },
...
}
```
The `--open-page` flag is to open the path supplied in string right away after the server runs.

## Create your project from this repo
There are two ways (the only two I know right now) to create your ready-to-use project from this repo. The first one, and the most simple, is to creat a `fork` from it but if you want a fresh installation you can move through the following steps:

1. **Create a new origin in your newly created repo:** for this you need your project already cloned locally and run the following command:
```
git remote add template git@github.com:efrapp/js-project-template.git
```
`template` is an optional name, you can choose one of your preference.

2. **Pull the repo to your local project:** create first the branch where you want to pull the template and run the following command:
```
git pull template master --allow-unrelated-histories
```
The `--allow-unrelated-histories` allows to combine the histories from two completely different project. With this you will avoid errors when git tries to merge the content.

