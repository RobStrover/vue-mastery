# Vue Mastery
Taking advantage of that sweet free weekend on Vue Mastery: https://www.vuemastery.com/

## Lesson 1 - Intro to Vue.js
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
## Lesson 2 - Real World VueJS
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
### Global Components
Eliminate the tedium of exporting and importing components used everywhere... things like home links, icons etc.
in your main js file do the following:
```vue
... other imports here
import ComponentName from '@/components/ComponentName.vue'

Vue.component('ComponentName', ComponentName)

```
And that's it! `ComponentName` can now be used in any component.

This is fine for one and two but we should use automatic registration if we have many we want to import many into the 
main.js file:
```vue
const requireComponent = require.context(
      './components',
      false,
      /Base[A-Z]\w+\.(vue|js)$/
    )
    
    requireComponent.keys().forEach(fileName => {
          const componentConfig = requireComponent(fileName)
        
          const componentName = upperFirst(
            camelCase(
              fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
            )
          )
        
          Vue.component(
            componentName,
            componentConfig.default || componentConfig
          )
        }) 
```
In this example: `require.context` is a Webpack function that takes the following arguments:
- Directory to search within
- Include Subdirectories
- Regex pattern

So in the example above we are searching in the components directory but not sub-directories for any .vue or .js file
with the prefix 'Base'.

The `camelCase` method is Loadash method which is being used to convert any kabob-case names to camel-case.

### Using SVG with Vue
Parts of svg files can be referenced and displayed conditionally within vue by using `<svg>` elements:
```vue
<template>
    <div class="icon-wrapper">
      <svg class="icon" width="" height="">
        <use v-bind="{'xlink:href':'/feather-sprite.svg#'+name}"/> // notice +name here
      </svg>
    </div>
</template>
    
<script>
    export default {
      props: {
        name: String // the name of the symbol we want to use
      }
    }
</script>
```

The svg must of course be prepared for use like this:
```
<symbol id="activity" viewBox="0 0 24 24">
  <polyline points="22 12 18 12 15 21 9 3 6 12 2 12"></polyline>
</symbol>
```
Notice the ID is added to the symbol in order to get this to work.

### Templates in Slots
It is possible to include a template tag inside of a slot if you need to have more than one element (can also be components) 
put into a slot:
```vue
<template>
    <MediaBox>
        <h2 slot="heading">Rob S</h2>
        <template>
            <p>Some words.</p>
            <p>Some other blahh.</p>
            <OtherComponent/>
        </template>    
    </MediaBox>
</template>
```
Notice that this nested `template` tag does not HAVE to have a single root element like the parent `template` tag.

### Services
Services are great for things like code that calls APIs where we want to keep things in one place for multiple components 
to use. Create a `services` directory and create a js file inside of it.
```js
import axios from 'axios'

const apiClient = axios.create({
    baseURL: 'http://localhost:3000',
    withCredentials: false,
    headers: {
        Accept: 'application/json',
        'Content-Type': 'application/json'
    }
});

export default {
    getEvents() {
        return apiClient.get('/events')
    }
}
```
What the above shows is that because the `apiClient` has a base url set, the `getEvents` call will have the base url
prepended to it meaning the call will go to `http://localhost:3000/events`.

This service can then be imported into the component where it needs to be used:
```js
import EventService from '@/services/EventService.js'

EventService.getEvents()
    .then(response => {
        console.log(response.data)
    })
    .catch(error => {
        console.log(error)
    })
```
## Lesson 3 - Vuex
### Accessing State Variables
Variables from the store can be accessed at any time using from within a template using:
```vue
{{ $store.state.blahh }}
```

And it can be used at any time in component script logic using:
```vue
this.$store.state.blahh
```

### Using Map State
Sometimes we have a larger number of variables in the store that we want to use in our templates. To do this, we can 
use `mapState`:
```vue
<script>
    import { mapState } from 'vuex'
    
    export default {
        computed: mapState({
            userName: state => state.user.name,
            userId: state => state.user.id
        })
    }
</script>
```
You can access entire states rather than single properties as well:
```vue
<script>
    import { mapState } from 'vuex'
    
    export default {
        computed: mapState({
            user: 'user',
            categories: 'categories'
        })
    }
</script>
```
You can then access the properties of these states using dot notation. If you don't need to rename your states, you can
shorten this to:
```vue
<script>
    import { mapState } from 'vuex'
    
    export default {
        computed: mapState([ 'user', 'categories' ])
    }
</script>
```
If you want to also include some local computed values, restructure using the spread operator like so:
```vue
<script>
    import { mapState } from 'vuex'
    
    export default {
        computed: {
            localComputedVariable() {
                return 'papoi'
            },
            ...mapState([ 'user', 'categories' ])
        }
    }
</script>
```
The same can be done with getters:
```vue
<script>
    import { mapGetters, mapState } from 'vuex'
    
    export default {
        computed: {
            localComputedVariable() {
                return 'papoi'
            },
            ...mapGetters([ 'getUserById', 'getCategories' ]),
            ...mapState([ 'user', 'categories' ])
        }
    }
</script>
```
### Pagination with Vuex
To get page number from the URL, use the following:
```vue
this.$route.query.page
```
'page' here can be any variable you want to get from the query strings in the url

### Reloading Components when Any Part of URL Changes
Sometimes you'll find clicking a link will not reload components properly. To make this happen, add the following to your 
router-view:
```vue
<router-view :key="$route.fullPath" />
```
### Using Vuex Modules
To create Nuxt style store modules, create a 'stores/modules' directory and then named store JS files within this directory.
The base store.js file should go in the 'stores' directory also.

Then when defining your Vuex store, indicate the modules that need to be included like so:
```vue
export default new Vuex.Store({
    modules: {
        storeName
    }
...
```
Then when using this inside of a component:
```vue
{{ moduleName.stateName.variableName }}
```
### Accessing the state of one module inside of another
You can access the 'rootState' inside of a state component. This is the base state that can then access any state module 
using dot notation.

### Namespaced Vuex Modules
By default, all actions, getters and mutations are registered in the Vuex global state. This is great for ease of use but 
also bad because we can end up with naming collisions. We can fix this with:
```vue
export const namespaced = true
```
This is to be added in your Vuex module files and causes an update to be required to your action dispatches:
```vue
this.$store.dispatch('storeNamespace/actionName', {
...
```
Just like in Nuxt. A great way to use this with mapActions is as follows:
```vue
methods: mapActions('storeNamespace', ['actionName1', 'actionName2'])
```
### Calling the action of one Namespaced Module inside of another
This can be done like so:
```vue
dispatch('moduleNamespace/actionName', null, { root: true })
```
- null is the payload for the action call.
- the third parameter tells Vuex to look for the action from the root of the store.
