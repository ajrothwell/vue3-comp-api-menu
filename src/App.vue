<template>
  
  <h1>{{ name }}</h1>

  <input type="text" v-model="name" />
  <!-- you don't have to write name.value because vue automatically unwraps it
  in the template or any other options api areas -->

  <button @click="placeOrder">Place order</button>
  <button @click="removeWatcher">Hide cart alerts</button>

  <br><br>
  <label for="currencySymbol">Currency</label>
  <select id="currencySymbol" v-model="currencySymbol">
    <option value="$">Dollars ($)</option>
    <option value="£">Pound (£)</option>
  </select>

  <YummyMeal
    v-for="meal in meals"
    :name="meal.name"
    :price="meal.price"
    @addToCart="addItemToCart"
  />
  <!-- :currencySymbol="currencySymbol" -->

</template>

<script>

import YummyMeal from './components/YummyMeal.vue';
import { ref, reactive, watch, watchEffect, provide } from 'vue';
// ref is short for "a reactive reference."
// Under the hood, vue takes the argument passed to ref and wraps it in an
// object with a .value property, that can be used to access or mutate
// the value of a reactive variable.
// this is necessary because primitive values in js are passed by value
// and not by reference
// having a wrapper object around any value allows us to safely use it throughout
// our component, without having to worry about losing its reactivity somewhere along the way.


export default {
  name: 'App',
  components: {
    YummyMeal,
  },
  setup() {
    const currencySymbol = ref('$');
    provide('currencySymbol', currencySymbol);
    // const currencySymbol = "$";


    // in order to make primitive data defined in the setup function reactive,
    // we should wrap it in a reactive reference by passing it to the ref function.
    // Then it is just like defining it with the options api data method
    const name = ref('The Snazzy Burger');
    console.log('in setup, name:', name, 'name.value:', name.value);
    // name.value = "Hello from the setup function";

    const cart = reactive([]);
    // const cart = ref([]);

    const meal1 = ref({ name: 'Hamburger', price: 5 });
    console.log('meal1.value.name:', meal1.value.name);

    // since objects are already stored by reference and not by value, we don't have to
    // turn them into reactive references.  Instead we just need to make them
    // reactive by passing them to vue's reactive method.
    // This eliminates the need to use the .value property to access or mutate the object's properties
    // in the setup method.

    const meal = reactive({ name: 'Hamburger', price: 5 });
    console.log('meal.name:', meal.name);

    const meals = reactive([
      { name: 'Hamburger', price: 5 },
      { name: 'Cheeseburger', price: 6 },
      { name: 'Impossible Burger', price: 7 },
      { name: 'Fries', price: 2 },
    ])

    const placeOrder = () => alert('Your order has been placed');
    // const addItemToCart = (item) => alert(`One ${item} added to cart`);
    const addItemToCart = (item) => cart.push(item);
    // const addItemToCart = (item) => cart.value.push(item);

    // Using a watcher to compare values of an array or object that are reactive requires that it has
    // a copy made of just the values
    // that's because watching a reactive object or array will always return a reference to the current value
    // of the object for both the current and previous values of the state
    // we can do that by passing an anonymous function to watch, and spreading the cart into a new array
    const removeWatcher = watch([name, () => [...cart]],
    (newValue, oldValue) => {
      alert(newValue.join('\n'));
      // console.log('watch cart, newValue:', newValue, 'oldValue:', oldValue);
    });

    watch(() => [...cart], (newCart, oldCart) => {
      console.log('watch cart alone, newCart:', newCart, 'oldCart:', oldCart);
    });

    // watch(name, (newName, oldName) => {
    //   console.log('watch name, newName:', newName, 'oldName:', oldName);
    // });

    return {
      name,
      placeOrder,
      addItemToCart,
      meal,
      meals,
      removeWatcher,
      currencySymbol
    };
  },

  created() {
    // We have to leave off the .value
    console.log('created, this.name:', this.name);
    // this.name = "Hello from the created hook";
  },

  // watch: {
  //   name(newName, oldName) {
  //     console.log('watch name, newName:', newName, 'oldName:', oldName);
  //   },
  // },

}

</script>

<style scoped>

</style>
