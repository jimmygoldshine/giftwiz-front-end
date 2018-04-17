<template>
  <div class='dropdown'>
    <div class="field">
      <span @click="dropdown = !dropdown">{{ selected }}</span>
      <ul v-show="dropdown">
        <li v-for="anniversary in anniversaries" @click="select(anniversary)"><span>{{ anniversary }}</span></li>
      </ul>
    </div>
    <div class="arrow" @click="dropdown = !dropdown">
    </div>
  </div>

</template>

<script>

export default {

  name: 'Dropdown',

  props: ['anniversaries'],

  data() {
    return {
      dropdown: false,
      selected: 'Choose anniversary',
    }
  },

  methods: {
    select(anniversary) {
      this.selected = anniversary;
      this.dropdown = false;
      this.$emit('selected', anniversary)
    }
  }

}

</script>

<style>

.dropdown {
  position: relative;
  height: 40px;
  width: 200px;
  margin: 0 auto;
  cursor: pointer;
  display: flex;
  border-bottom: 2px solid #afafaf;
}

.field {
  display: inline-block;
  height: 40px;
  width: 160px;
}

ul {
  position: absolute;
  background: white;
  list-style: none;
  padding: 0;
  margin: 0 auto;
  text-align: center;
  width: 160px;
  box-shadow: 0 0 0.7rem rgba(0, 0, 0, .1);
  z-index: 2;
}

.field > span {
  width: 100%;
  padding: 10px 0;
  color: #afafaf;
}

li {
  height: 30px;
  background: white;
}

li:hover {
  background: #f7f7f7;
  color: #44c583;
}

li span {
  padding: 6px 0;
}

.arrow {
  display: inline-block;
  height: 100%;
  width: 40px;
  background-image: url('../assets/arrow.png');
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
}

</style>
