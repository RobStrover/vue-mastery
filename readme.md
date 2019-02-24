# Vue Mastery
Taking advantage of that sweet free weekend on Vue Mastery: https://www.vuemastery.com/

## Course 1 - Intro to Vue.js
I am not new to Vue so I skipped over a lot of the basics. Here are some cool things I didn't know, though:

### Short-hand v-for Key Binding
Binding the key in v-for can be done like so:
```vue
<p v-for="(person, index) in people" :key="index">{{ person.name }}</p>
```

### Modifiers
Vue allows for modifiers to be used with Events. For example `@keyup.enter` means that the keyup listener will only be for
the enter key. There's a whole host of these, take a look in the Cheat Sheet (in the root of this repo).

### Style Binding
Style binding means altering the inline style of an object through an expression bound to the style tag:
```vue
<p :style="{ color: getColourName() }">Some Fancy Colour</p>
```
Notice the curly braces inside of the style tag. We can make a call to computed properties, data, methods or even just 
use inline expressions. This is all well and Good for a single style property, however things get out of hand quickly. We 
can use a *Style Binding Object* to add multiple style options:
```vue
<p :style="styleObject">Some Fancy Colour</p>


data: {
    styleObject: {
        color: 'red',
        fontSize: '30px'
    }
}
```

### Class Binding
Usually I bind classes to a computed property which returns the class value, however you can express this inline quite nicely 
as well:
```vue
<p :class="{ fancy: fancyFactor > 100 }">Some Fancy Class</p>
```
So in the example above, if our data (could be a method or computed) property 'fancyFactor' is greater than 100, then we add the class
'fancy' is added to the <p> tag. This can be extended even further by the use of ternary operators:
```vue
<p :class="[ fancyFactor > 100 ? 'fancy' : 'not-so-fancy' ]">Some Fancy Class</p>
```
### Props Options Object
A component prop can have extra rules applied to it by passing it like so:
```vue
props: {
    textColor: {
        type: String,
        required: false,
        default: "red"
    }
}
```
## Course 2 - Real World VueJS
### Vue CLI and Vue GUI
If you have not already got it... install Vue CLI:

`npm install -g "vue/cli` If you get permissions errors, install and use NVM instead!

You can use the CLI to get a project set up like so:
`vue create project-name` You'll get asked all the options you want on installation.

WHat is cool though is the Vue GUI which can be accessed from:
`vue ui`
This allows you to create and manage applications from a sweet user interface.

### Routing to Named Routes
When using router links, it is possible to route to a 'named route':
```vue
<router-link :to="{ name: 'home' }">Home</router-link>
```
These relate directly to the 'name' property of each route object in router.js

### Redirects in Vue Router
It is possible to create redirects using named routes:
```vue
    {
      path: "about-redirect",
      redirect: { name: "about" } // could also just be redirect: "/about"
    },
    {
      path: "/about",
      name: "about",
      component: About
   }
```
By the way... although it says 'component', this is still a component being treated as view in the 'views' folder

### Dynamic Routes
It is possible to create dynamic routes in a router:
```vue
    {
        path: "/user/:username",
        name: "user",
        component: "User"
    }
```
The username variable will usable in the User view like so:
```vue
<p>this is the page for {{ $route.params.username }}</p>
```
### Linking to a Dynamic Route
Linking to a dynamic route can be done by using a params object as the second argument, like so:
```vue
<router-link :to="{ name: 'user', params: { username: 'Rob' } }">Rob</router-link>
```
We can make all this even better by adding the following to our dynamic route:
```vue
    {
        path: "/user/:username",
        name: "user",
        component: "User",
        props: true // <-----------
    }
```
This will then make the username prop available to this view (component).

### Removing the Pesky Hash
To remove the hash from Router URLs, we add configuration to our router.js:
```vue
export default new Router({
    mode: "history",
    routes: [
    ....
```
This forces the application to the browsers history push state API without reloading the page.

### Routing and Server Deployment
We need to overwrite the default behaviour of NGINX, Apache etc... Example implementations can be found here:
https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations

These implementations all have the caveat however of no longer returning a 404 page as all requests will always serve the 
index.html file. To get around this, add a route at the bottom of the routes array to act as a catchall if no other routes
match:
```vue
export default new Router({
    mode: "history",
    routes: [
        {
          path: "about-redirect",
          redirect: { name: "about" } // could also just be redirect: "/about"
        },
        {
          path: "/about",
          name: "about",
          component: About
       },
       { path: '*', component: NotFoundComponent}
    ]
})
```
