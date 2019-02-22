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

 