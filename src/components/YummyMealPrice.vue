<template>
  <span>{{ pricePretty }}</span>
</template>

<script>

import { computed, inject, ref } from "vue";

export default {
  props:{
    price: Number,
    // currencySymbol: String,
  },

  setup(props) {

    const currencySymbol = inject('currencySymbol', ref('$'));

    // using computed props with the composition api doesn't really demonstrate any clear
    // advantage over using them with the options api
    // the real power comes into play when you extract your logic into composables
    // for reuse in multiple components
    // const pricePretty = computed(() => `$${props.price.toFixed(2)}`);

    const pricePretty = computed({
      get() {
        return `${currencySymbol.value}${props.price.toFixed(2)}`;
      },
      set(price) {
        alert(`Sorry, we can not accept ${price}. Nice try though!`);
      }
    })

    // like refs, you should use .value for computeds in the setup method
    const pricePrettySentence = computed(
      () => `The price of this item is ${pricePretty.value}`
    );

    return { pricePretty, pricePrettySentence };
  },

};

</script>