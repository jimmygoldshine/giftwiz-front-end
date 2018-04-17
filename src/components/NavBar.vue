<template>
  <div class='full-width'>
    <h3 class="left" @click="gifts">Gifts</h3>
    <h1 @click="home">Giftwiz</h1>
    <h3 class="right" @click="viewShortlist">Shortlist <span>({{ shortlist.length }})</span></h3>
  </div>
</template>

<script>

export default {

  name: 'NavBar',

  props: ['shortlist'],

  methods: {
    home() {
      this.$emit('home')
    },
    viewShortlist() {
      this.$emit('shortlist')
    },
    gifts() {
      this.$emit('carousel')
    }
  }

}

</script>

<style>

  h1, h2 {
    margin: 10px 0;
  }

  h3 > span {
    font-size: 12px;
  }

  .left {
    position: absolute;
    left: 0;
  }

  .right {
    position: absolute;
    right: 0;
  }

  .full-width {
    position: relative;
    z-index: 1;
    display: flex;
    justify-content: center;
    margin: 0 auto;
    width: 95%;
    text-align: center;
  }

  .full-width * {
    text-align: center;
  }




</style>
