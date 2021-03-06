<template>
  <div class="gifts">
    <carousel-3d ref="carousel" :width="400" :height="370" :minSwipeDistance="5" @after-slide-change="slideChange" :startIndex="(products.length)/2" :loop="false">
      <slide v-for="(product, i) in products" :index="i" :key="i">
        <figure>
          <div :style="`background: url(${product.image}) no-repeat center center`"></div>
          <figcaption>
            <span>{{ product.name }}</span>
            <span class="price">£{{ product.price }}</span>
          </figcaption>
        </figure>
      </slide>
    </carousel-3d>
    <div>
      <div class='container'>
        <div @click="next">
          <img src='../assets/cancel.png'>
        </div>
        <div @click="prev">
          <img src='../assets/heart.png'>
        </div>
      </div>
      <div>
        <button class="shadow" @click="viewShortlist">View Shortlist <span>({{ shortlist.length }})</span></button>
      </div>
    </div>
  </div>
</template>

<script>

import { Carousel3d, Slide } from 'vue-carousel-3d';

export default {
  name: 'Gifts',

  components: {
    Carousel3d,
    Slide
  },

  props: ["products", "shortlist"],

  data () {
    return {
      currentIndex: 0,
      previousIndex: undefined
    }
  },

  mounted() {
    this.currentIndex = this.products.length/2
  },

  computed: {

  },

  methods: {

    next() {
      this.$refs.carousel.goNext()
    },

    prev() {
      this.$refs.carousel.goPrev()
    },

    shortlistProduct() {
      let currentProduct = this.products[this.previousIndex];
      this.$emit('shortlistedProduct', currentProduct);
    },

    removeProduct() {
      let currentProduct = this.products[this.previousIndex];
      this.$emit('removedProduct', currentProduct);
    },

    slideChange() {
      this.previousIndex = this.currentIndex;
      this.currentIndex = this.$refs.carousel.currentIndex;
      if (this.swipeRight()) {
        if (this.looped()) {
          this.shortlistProduct()
          this.$refs.carousel.total = this.products.length
          this.$refs.carousel.currentIndex = this.$refs.carousel.total-1
          this.currentIndex = this.$refs.carousel.currentIndex;
        } else {
          this.shortlistProduct()
        }
      }
      if (this.swipeLeft()) {
        this.removeProduct()
      }

    },

    viewShortlist() {
      this.$emit('viewShortlist');
    },

    swipeLeft() {
      return this.currentIndex > this.previousIndex && this.previousIndex != 0
    },

    swipeRight() {
      return this.currentIndex < this.previousIndex || this.looped()
    },

    looped() {
      return this.previousIndex == 0 && this.currentIndex > 1
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato');

.gifts {
  width: 100%;
  overflow: hidden;
  flex: 1 1 0;
}

.container {
  display: flex;
  justify-content: center;
  width: 200px;
  margin: 10px auto 20px;
  height: 60px;
}

.container div {
  margin: 0 10px;
  border-radius: 50%;
  box-shadow: 0 3px 9px rgba(0,0,0,0.29);
  width: 60px;
  height: 60px;
}

.price {
  font-weight: bold;
  margin-left: 20px;
}

.carousel-3d-container {
  margin: 10px auto 0 !important;
  height: 400px !important;
}

.carousel-3d-slide {
  background: white !important;
  border-radius: 5px;
  border: 0px;
  box-shadow: 0 1px 19px rgba(0,0,0,0.29);
}

figure > div {
  height: 250px;
  overflow: hidden;
  border-radius: 3px;
  background-size: cover !important;
}

figcaption {
  margin: 5px 0;
}

img {
  max-height: 100%;
  max-width: 100%;
  object-fit: contain;
  border-radius: 3px;
  margin: 0 auto;
}

button > span {
  font-size: 12px;
}

</style>
