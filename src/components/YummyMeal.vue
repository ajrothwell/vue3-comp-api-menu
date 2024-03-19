<template>

  <p>
    <strong>{{ name }}</strong>

    <!-- <input type="text" v-model="pricePretty"> -->
    <YummyMealPrice
      :price="price"
    />
    <!-- :currencySymbol="currencySymbol" -->

    <button @click="addToCart">Add to cart</button>
    
  </p>
</template>

<script>

import { computed } from "vue";
import YummyMealPrice from "./YummyMealPrice.vue";

export default {
  components: {
    YummyMealPrice,
  },
  props: {
    name: String,
    price: Number,
    // currencySymbol: String,
  },
  setup(props, { attrs, slots, emit }) {
    // when the setup method is called by vue, the component instance is not yet created.
    // and thus 'this' is undefined
    // you can never use the keyword 'this' to access any of the data, props, or methods
    // as you normally would with the options API
    console.log('attrs:', attrs, 'slots:', slots, 'emit:', emit, 'props.name:', props.name, 'props.price:', props.price);
    // attrs, slots, emit are the "context" - we can destructure them cause they are not reactive

    const addToCart = () => emit('addToCart', props.name);

    return { addToCart };
  },

  // computed: {
  //   pricePretty() {
  //     return `$${this.price.toFixed(2)}`;
  //   }
  // }

}

</script>

