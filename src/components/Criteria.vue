<template>
  <div class="criteria">
    <div class='greeting'>
      <h1>Hi there,</h1>
      <h2>What are you looking for today?</h2>
    </div>
    <div>
      <h2>I am looking for a</h2>
      <dropdown :anniversaries="anniversaries" @selected='selected' />
    </div>
    <div>
      <h2>wedding anniversary gift for</h2>
      <div class="buttons">
        <button :class="{ dulled: dulled('recipient', 'her') }" @click="criteria.recipient='her'">her</button>
        <button :class="{ dulled: dulled('recipient', 'him') }" @click="criteria.recipient='him'">him</button>
        <button :class="{ dulled: dulled('recipient', 'couples') }" @click="criteria.recipient='couples'">couples</button>
        <button :class="{ dulled: dulled('recipient', 'dont mind') }" @click="criteria.recipient='dont mind'">don't mind</button>
      </div>
    </div>
    <div>
      <h2>in the price range of</h2>
      <div class="buttons">
        <button :class="{ dulled: dulled('price', 'cheap') }" @click="criteria.price='cheap'">£</button>
        <button :class="{ dulled: dulled('price', 'average') }" @click="criteria.price='average'">££</button>
        <button :class="{ dulled: dulled('price', 'expensive') }" @click="criteria.price='expensive'">£££</button>
        <button :class="{ dulled: dulled('price', 'dont mind') }" @click="criteria.price='dont mind'">don't mind</button>
      </div>
    </div>
    <div>
      <button class="submit shadow" @click='submitCriteria'>go</button>
    </div>
  </div>
</template>

<script>

import Dropdown from './Dropdown.vue'

export default {
  name: 'Criteria',

  components: {
    Dropdown
  },

  props: ['anniversaries'],

  data () {
    return {
      criteria: {
        anniversary: '',
        recipient: '',
        price: ''
      }
    }
  },

  created() {
  },

  methods: {
    submitCriteria () {
      this.$emit('submittedCriteria', this.criteria)
      window.dispatchEvent(new Event('resize'));
    },
    dulled (field, value) {
      return (value != this.criteria[field] && this.criteria[field] != '');
    },
    selected(anniversary) {
      this.criteria.anniversary = anniversary;
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato');

h1, h2 {
  color: #d8d8d8;
}

.greeting * {
  color: #44c583;
  font-style: italic;
}

.criteria {
  width: 95%;
  margin: 0 auto;
  text-align: center;
  overflow: hidden;
  flex: 1 1 0;
}

.criteria .buttons {
  display: flex;
  width: 100%;
  justify-content: center;
}

.criteria button {
  width: 25%;
  padding: 10px 0;
  font-style: italic;
  border-radius: 0;
  border: 0;
  color: black;
  background: white;
  font-size: 12px;
  border-left: 1px solid #d8d8d8;
  cursor: pointer;
  outline: none;
}

.criteria button.dulled {
  opacity: 0.5;
}

.criteria button:first-of-type {
  border-left: 0
}

.select {
  background: url(http://i62.tinypic.com/2e3ybe1.jpg) no-repeat right center;
  border: none;
  border-bottom: 1px solid #d8d8d8;
  border-radius: 0;
  width: 200px;
  height: 30px;
  background-size: 18%;
  margin: 0 auto;
}

.select select {
    padding: 5px 8px;
    width: 100%;
    color: black;
    border: none;
    box-shadow: none;
    background: transparent;
    font-size: 16px;
}

.select-style select:focus {
    outline: none;
}

.criteria .submit {
  margin-top: 20px;
  border-radius: 50%;
  height: 80px;
  width: 80px;
  color: #EDEDED;
  font-size: 20px;
  text-transform: uppercase;
  font-style: normal;
  background: linear-gradient(to bottom right, #44c583, #37d685);
}

</style>
