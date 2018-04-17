<template>
  <div class="product">
    <div class="remove" @click="removeProduct">
      <img src='../assets/cancel.png'>
    </div>
    <div>
      <span class="linear" v-show="!viewDetails">View Details</span>
    </div>
    <figure @click="viewDetails = !viewDetails">
      <div :style="`background: url(${product.image}) no-repeat center center`"><a :href="link"></a></div>
      <figcaption>
        <span>{{ product.name }}</span>
        <span class="price">£{{ product.price }}</span>
      </figcaption>
    </figure>
    <transition name="fade">
      <div class="product-details" v-if="viewDetails">
        <button class="buy"><a :href="product.url">BUY GIFT</a></button>
        <p>Taking you to the retailers website</p>
        <h4>Product Details</h4>
        <span>{{ shorten(product.description) }}</span>
      </div>
    </transition>
  </div>
</template>

<script>

export default {
  name: 'Product',

  props: ['product'],

  data () {
    return {
      viewDetails: false
    }
  },

  methods: {
    shorten(description) {
      return description.split('.', 2).join('.') + '!'
    },
    removeProduct() {
      this.$emit('remove', this.product)
    },
    imageClick() {
      if (!this.viewDetails) {
        this.viewDetails = true
      } else {
        window.location.href = this.product.url
      }
    }
  },

  computed: {
    link() {
      if (this.viewDetails) {
        return this.product.url
      }
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato');

p {
  text-align: center;
}

.linear {
  background: linear-gradient(to bottom right, #44c583, #37d685);
}

a {
  color: white;
  text-decoration: none;
}

.product-details {
  margin: 20px 10px;
  overflow: hidden;
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

.product {
  position: relative;
}

.product .linear {
  padding: 12px 19px;
  position: absolute;
  border-radius: 3px;
  color: white;
  font-weight: bold;
  letter-spacing: 0.1rem;
}

.remove {
  position: absolute;
  height: 25px;
  width: 25px;
  right: 2px;
  top: -5px;
}

.buy {
  background: linear-gradient(to bottom right, #44c583, #37d685);
  box-shadow: 0 3px 9px rgba(0,0,0,0.29);
}



</style>
