<template>
  <div class="app">
    <nav-bar :shortlist="shortlist" @home="showCriteria = true ; showCarousel = false; showShortlist = false"
    @shortlist="showCriteria = false; showCarousel = false; showShortlist = true"
    @carousel="showCriteria = false; showCarousel = true; showShortlist = false"
      />
    <div class="app-body">
      <transition name="fade">
        <criteria v-if="showCriteria" :anniversaries="anniversaries" @submittedCriteria="filterGifts" />
      </transition>
      <transition name="fade">
        <gifts v-if="showCarousel" :products="filteredProducts" :shortlist="shortlist" @shortlistedProduct="addToShortlist" @removedProduct="removeProduct" @viewShortlist="viewShortlist" />
      </transition>
      <transition name="fade">
        <shortlist v-if="showShortlist" :products="shortlist" @remove="removeFromShortlist"/>
      </transition>
    </div>
  </div>
</template>

<script>
import NavBar from './components/NavBar';
import Criteria from './components/Criteria';
import Gifts from './components/Gifts';
import Shortlist from './components/Shortlist';
import axios from 'axios';

export default {
  name: 'App',

  components: {
    NavBar,
    Criteria,
    Gifts,
    Shortlist
  },

  data () {
    return {
      allProducts: [],
      anniversaries: [],
      showCriteria: true,
      showCarousel: false,
      showShortlist: false,
      filteredProducts: [],
      shortlist: []
    }
  },

  created() {
    this.fetch()
  },

  methods: {
    fetch: function() {
      axios.get('https://mythic-plexus-194517.appspot.com/products/')
        .then(response => {
          this.allProducts = response.data;
          this.allProducts.forEach(product => {
            if (!this.anniversaries.includes(product.anniversary)) this.anniversaries.push(product.anniversary)
          })
          this.anniversaries.sort((a, b) => a - b)
          window.dispatchEvent(new Event('resize'));
        })
        .catch(e => {console.log(e)})
    },

    filterGifts: function(criteria) {
      let anniversaryProducts = this.allProducts.filter((product) => {
        if (product.anniversary == criteria.anniversary) {
          return product
        }
      })
      let pricedProducts = this.priceMap(anniversaryProducts)
      this.filteredProducts = pricedProducts.filter(product => {
        if (product.priceBracket == criteria.price) {
          return product
        }
      })
      if (criteria.price == 'dont mind') {
        this.filteredProducts = anniversaryProducts
      }
      this.showCarousel = true;
      this.showCriteria = false;
    },

    priceMap: function(products) {
      products.sort((a, b) => {
        return parseInt(a.price) - parseInt(b.price)
      })
      console.log(products)
      products.forEach((product, index) => {
        if (index < products.length / 3) {
          product['priceBracket'] = 'cheap'
        } else if (index >= (products.length*2) / 3) {
          product['priceBracket'] = 'expensive'
        } else {
          product['priceBracket'] = 'average'
        };
      })
      return products
    },

    addToShortlist(product) {
      if (product) {
        this.shortlist.push(product)
      }
      this.filteredProducts = this.filteredProducts.filter(p => p != product)
    },

    removeProduct(product) {
      if (this.filteredProducts.indexOf(product) < this.filteredProducts.length - 1) {
        this.filteredProducts.pop()
      } else {
        let lastItem = this.filteredProducts.pop()
        this.filteredProducts = this.filteredProducts.filter(p => p != product)
        window.dispatchEvent(new Event('resize'));
      }
    },

    viewShortlist() {
      this.showShortlist = true;
      this.showCarousel = false;
      this.showCriteria = false;
    },

    removeFromShortlist(product) {
      this.shortlist = this.shortlist.filter(p => p != product)
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato');

body {
  background: #EDEDED;
  font-family: 'Lato', sans-serif;
  margin: 0 2px;
}

.full-width * {
  color: black;
}

button {
  display: table;
  color: white;
  background: #44c583;
  border: none;
  padding: 15px 20px;
  width: 175px;
  margin: 10px auto;
  border-radius: 5px;
  font-size: 1em;
  outline: none;
}

.shadow {
  box-shadow: 0 3px 9px rgba(0,0,0,0.29);
}

figure {
  padding: 15px;
  margin: 0
}

figcaption {
  display: flex;
  justify-content: space-between;
  font-size: 1.2em;
  width: 100%;
}

span {
  display: inline-block;
}

figcaption > span:first-of-type {
  width: 75%;
}

.app {
  height: 100vh;
  display: flex;
  flex-flow: column;
  max-width: 600px;
  margin: 0 auto;
}

.app-body {
  flex: 1 1 0;
  display: flex;
  flex-direction: column;
}

.fade-enter-to, .fade-leave {
  max-height: 550px;
  height: 100%;
}

.fade-enter-active, .fade-leave-active {
  transition: max-height 1s linear;
}
.fade-enter, .fade-leave-to {
  height: auto;
  max-height: 0;
}

</style>
