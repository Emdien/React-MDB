# React Movie Database

This is a project that has been done following the [ReactJS Course present on the FreeCodeCamp youtube channel](https://www.youtube.com/watch?v=nTeuhbP7wdE), which was uploaded in July 2021. In said course, I developed a website using ReactJS and StyledComponents, and was hosted using Netlify. It was also used create-react-app as an starter point of the project.

This readme file will contain different notes I have taken from the course, so I can check and use in the future when working with React. Its divided in different sections, such as general rules or information, components, hooks, styling and so on.

## General 

### App Wrappers

When making an application with React, you will have two different kinds of wrapper. You will be presented with a file called **index.js**, which calls the ReactDOM.render method. Inside said method it will usually have a JSX object, which wraps an object called App (or similar) around a react.strictmode tag. Said tag is not necessary, but good practice.

Then aside of that file, you will be presented with a file named **App.js**. That file's purpose is handling the different "main" components of the applications, aswell as doing the routing of the different pages.

> Its important to note that to do the web routing, its necessary to import the react-router-dom library and make use of its exports, BrowseRouter, Routes(6.0.0 and up) and Route.

In App.js its also needed to import the global styles if using styledcomponents and if you declared global styles to be applied on your page.

For more information, check **src/app.js**

### Config files

Its a good practice to have some kind of configuration file which holds things like, API keys, base URLs, and overall API related things. Its always important to check the documentation of the API!.

Its also recommendable to make a file such as API.js, which has methods that handle the API requests so its way easier to use, instead of having to construct these methods each time they are used .

### ENVIROMENT Variables

In this app it was used an enviroment variable. In our case, locally we used a file named .env. The config.js file then obtains the API key value using *process.env.name_of_the_key*.

When deployed, in netlify it was specified to use an enviroment variable of the same name.

## Components

### Main components

What I call a main component is basically a component that is no trivial in size. They usually wrap a bundle of other smaller components, and it passes down states and methods down to their childs, so these can modify the state of its parent.

On these components, usually before writing the actual component, you do the following:
- Import react
- Import config related constants 
- Import the smaller components you are gonna be using
- Import any hook that is needed
- Import miscelaneous

Once thats done, then *usually* goes the component, which is written as an arrow function such as
```javascript
const component = () => {...}
```

In these components, you usually either build your states using the useState hook from react, or destructure them from a hook that has been made previously (such as useHomeFetch.js in this project). More common to do the later.

Lastly, the component returns a JSX object, on which:
- You WRAP all the other react components between <> and </>
- You use other smaller components for more specific parts of the page
- You pass props such as state values or urls or anything really, or callback methods that will be used by the childs to alter the parent's state

Don't forget to export the component with
> export default *component*

### Components

Components are best organized on their own folders, and in this case, each folder holds 2 files:
- index.js: file that holds the logic of the component. This file could be name anything else, but naming it index.js makes it easier for importing. Need to documentate this more throroughly.
- Component.styles.js: file that is specific from styled-components library to style each one of the components. It holds the styling of the component.

First of all, as usual, gotta do the imports. These are usually:
- React
- Other components
- Config variables from configuration files
- The styles from the style file
- Misc

Similarly to the main components, it returns a JSX object using an arrow function. Said arrow function can return the object implicitly, by using parenthesis instead of curly braces, or using curly braces and explicitly returning the JSX object with the return term. The second one is used when you need some logic inside of the component, otherwise its recommended to simply return the JSX object implicitly.

These components have to be thought of as "small parts of a large bundle of components". A page is usually formed by a set of bundles of components. Its also important that the components are reusable, that being, they are not very specific, unique case type of components, that only work on a very specific scenario. A good usage of props is needed for a good reusable component.

While this is more related to the styling, its important to note that its good practice to follow some kind of structuring when making a component. The common structuring followed on this project is the following:

- A Wrapper component that consists of a div. Wraps the content of the component itself.
- A Content component, which holds the content itself as the name indicates. Content means things like, text, images and other sub components
- Other kind of components, such as Text, Thumb, Image and so on, that are particular of the component itself

The structure will usually look like this as an example:

```html
<Wrapper>
    <Content>
        <Text>
            ...
        </Text>
    </Content>
</Wrapper>
```

A good, complex component to look at as an example would be MovieInfo component. For a component with curly braces, SearhBar component can be used.

In a lot of cases, these components will be small and simple, where they simply import react and their styles.

## Hooks

Hooks are VERY important in React. There are two kinds of hooks
- Default React hooks, such as useState, useEffect...
- New hooks implemented by the developer of the app. They HAVE to follow the naming convention of u *useHookName*.

### Default Hooks

For more information about the default hooks, its recommended to check the [documentation](https://reactjs.org/docs/hooks-reference.html#gatsby-focus-wrapper). The basic gist of it is:

- useState gets deestructured into a state and a setter method for that state. Can receive as parameter an initial value for the state. Can set multiple states by calling the method multiple times and saving the state and setter on different variables.
- useEffect receives a function as a parameter. This hook calls the function once React has rendered the components. Ideally you pass an arrow function as a parameter. Good for things like fetching from API, updating some states and so on. It also has a second parameter which is an array of dependencies, needed for things outside of the useEffect hook (states for example).

Examples:
```javascript
// States
const [state, setState] = useState(initialState); 
const [loading, setLoading] = useState(false);
const [error, setError] = useState(false);
const [searchTerm, setSearchTerm] = useState('');
const [isLoadingMore, setIsLoadingMore] = useState(false);


// Initial and search  
    useEffect(() => {
        if (!searchTerm) {
            const sessionState = isPersistedState(sessionStorageItemName)

            if (sessionState) {
                console.log("Grabbing from sessionStorage")
                setState(sessionState);
                return;
            }
        }

        console.log("grabbing from API")
        setState(initialState);
        fetchMovies(1, searchTerm);
    }, [searchTerm])
```

### Own Hooks

When making your own hook, its important to keep in mind that it basically wraps the usage of the other, default hooks such as useState and useEffect, for the sake of simplicity when working with them and for not repeating similar or identical code in multiple places.

First of all, as usual, need to realize a series of imports. Said imports are:

- Hooks from react such as useState, useEffect etc.
- Other methods or variables needed such as API fetching methods or helper methods.


After that, depending on the logic of the hook, might want to set up a serie of global variables, such as an initial state with default values to pass to useState when making a new state.

When making the hook, its important to first of all, declare all the states and its setters, with the initial values as needed.

After that, you can make additional methods that might contain the logic behind the hook. Once thats done if needed, it follows the useEffect calls, which will basically contain the logic and will make React refresh its components when called

Its important to not forget that the hook has to return something. What is returned is an object that contains all the states and setters that will be needed outside of the hook.

A good example to look at in this repository would be **src/hooks/useHomeFetch.js**

## Styling

The styling was done with styled-components. Its very intuitive, there is not much to comment on this. For reference just look at the style files on each component, aswell as the global file.
