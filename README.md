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
        //this keyword is not valid in here
    }
    //other code
}
```