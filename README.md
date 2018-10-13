# FrontEndMasters: Complete intro to react v4


## Index 

[1. Lessons learned](#1-lessons-learned)


## 1. Lessons learned
### Code formating
[Setup ESLint and Prettier together for react and react-native projects in Visual Studio Code](https://medium.com/appstud/setup-eslint-and-prettier-together-for-react-and-react-native-projects-in-visual-studio-code-78772d58358d)

### Module bundler
#### General
[Comparing Javascript module bundlers : Webpack , Rollup or Parcel](https://blog.imaginea.com/comparing-javascript-module-bundlers-webpack-rollup-or-parcel/)  
#### Webpack
[A Beginner’s Guide to Webpack 4 and Module Bundling](https://www.sitepoint.com/beginners-guide-webpack-module-bundling/) 
#### Parcel
```javascript
{
    //other settings
    "scripts": {
        "dev": "parcel src/index.html" //path from root to index.html
    }
}
```

[Announcing Parcel: A blazing fast, zero configuration web application bundler](https://medium.com/@devongovett/announcing-parcel-a-blazing-fast-zero-configuration-web-application-bundler-feac43aac0f1)  
[First impressions with Parcel JS](https://codeburst.io/first-impressions-with-parcel-js-eb81fdcc1282)  
[Getting Started With Parcel](https://medium.com/codingthesmartway-com-blog/getting-started-with-parcel-197eb85a2c8c)  
[Everything You Need To Know About Parcel](https://medium.freecodecamp.org/all-you-need-to-know-about-parcel-dbe151b70082)  

### Environment variables
[Using Environment Variables in JavaScript](https://medium.com/appstract/environment-variables-in-javascript-with-laravel-elixir-593df994d765)  
[Using environment variables in React](https://medium.com/@trekinbami/using-environment-variables-in-react-6b0a99d83cf5)  
[Thread: How to access the .env variables inside javascript?](https://laracasts.com/discuss/channels/general-discussion/how-to-access-the-env-variables-inside-javascript?page=1)  
```javascript
//to refer to the .env file variavles
const firstVariable = process.env.YOUR_FIRST_VARIABLE,
const secondVariable = process.env.YOUR_SECOND_VARIABLE
```

### Hack: Visualize data state
```javascript
//this hack just throws all your data to the app
<pre>
    <code>{JSON.stringify(this.state.data, null, 4)}</code>
</pre>
```
### React pattern: loading
If a component is loading (async request for example) since the mounting, state shall have a key "loading" with value "true". When sometime in the future the component got the data, this.setState shall be called with "loading: false" to indicate that the loading is done.
```javascript
state = {
    loading: true
}

//time flies by...

this.setState({
    loading: false
})
```
This can be used for showing the user that the page is loading. For example in the render method:
```javascript
//if loading is true
render() {
    if(this.state.loading) {
        return <h1>page is loading...</h1>
    }
}

//if loading is false (done), show the fetched data
<div>
    <h1>{props.title}</h1>
    <p>{props.detail}</p>
</div>
```

### Lifecycle: getDerivedStateFromProps
[Replacing ‘componentWillReceiveProps’ with ‘getDerivedStateFromProps’](https://hackernoon.com/replacing-componentwillreceiveprops-with-getderivedstatefromprops-c3956f7ce607)  
[Using Derived State in React](https://alligator.io/react/get-derived-state/)  
[Refactor componentWillReceiveProps() to getDerivedStateFromProps() in React 16.3](https://egghead.io/lessons/react-refactor-componentwillreceiveprops-to-getderivedstatefromprops-in-react-16-3)  
```javascript
class Test extends React.Component {
    //state
    static getDerivedStateFromProps() {
        //"this" keyword is not valid in here
    }
    //other code
}
```
### this.setState callback function
The this.setState has a "hidden" second function, a callback can be made trough this. The reason behind is the following:
React batches setState calls together. For that reason, the following method might not work:
```javascript
state = { loading: false };
this.setState({ loading: true });
console.log(this.state.loading);
//false
```
As said, setStates will be batched together and will not be call'd immediately. So this.setState will be invoked sometime in the future, after we call'd console.log.
To prevent this issue, a callback can be made:

```javascript
state = { loading: false };
this.setState({loading: true}, console.log(this.state.loading)); 
//true
```
[Dan Abramov on twitter about batching setState calls](https://twitter.com/dan_abramov/status/887963264335872000?lang=en)  
[Beware: React setState is asynchronous!](https://medium.com/@wereHamster/beware-react-setstate-is-asynchronous-ce87ef1a9cf3)  

### Context API
#### in render()
See SearchBox.js for an example.
```javascript
//in ProviderComponent.js
//define Provider, standard valuew and export it
const componentContext = React.createContext({
    value: 1, //standard value, might be overwritten in App.js
    loading: false //standard value, might be overwritten in App.js
});

export const Provider = componentContext.Provider;
export const Consumer = componentConext.Consumer;

//in App.js (parent/root) 
//import the provider and then wrap the components into the Provider component
this.state = {
    value: 1,
    loading: false
}

render(
    <Provider value={this.state}>
        <ComponentParent />
    </Provider>
)

//in ComponentParent.js (child/node/leaf) 
//import Consumer and wrap all components with the Consumer
<Consumer>
    {context => ( //context is the state from App.js
        <div>
            <Component1 value={context.value} />
            <Component2 loading={context.loading} />
            <Component3 />
        </div>
    )}
</Consumer>
```
Every Component can now access the Consumer-state, respectively Provider-state.

#### in LifeCycleMethods
See Results.js for an example.
```javascript
//in the components with the LifeCycleMethod export, it wraped within a function and afterwards you can use this.props for referencing to the state/context of the Provider
componentDidMount() {
    //use in here this.props
    //eg:
    this.props.variousParams.someItemFromState
}
export default function ComponentWithContext(props) {
    return(
        <Consumer>
            {context => <Component {...props} variousParams={context} />}
        </Consumer>
    )
}
```

### Portals
[Tasks and Portals in React](https://medium.com/@MoneyhubEnterpr/tasks-and-portals-in-react-1df2438cdebb)  

### refs
Shall only be used with additional libraries like jQuery or D3. If you dont use such libraries, refs are seen as bad practice.