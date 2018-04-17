<template>
  <div class="shortlist">
    <div v-for="product in products">
      <product :product="product" @remove="remove"/>
    </div>
  </div>
</template>

<script>

import Product from './Product';

export default {
  name: 'Shortlist',

  props: ['products'],

  components: {
    Product
  },

  data () {
    return {

    }
  },

  methods: {
    remove(product) {
      this.$emit('remove', product)
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato');

.shortlist {
  display: flex;
  flex-direction: column;
}

.shortlist figure {
  background: white;
  border-radius: 5px;
  box-shadow: 0 1px 19px rgba(0,0,0,0.29);
  margin: 10px auto;
}

</style>
