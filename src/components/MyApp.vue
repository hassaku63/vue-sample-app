<template>
  <div class="my-app">
    <h1>{{ msg }}</h1>
    <h2>Menu</h2>
    <ul>
        <menu-item v-for='item in menuItems' :key='item.id' :dispaly-name='item.name'>
        </menu-item>
    </ul>
  </div>
</template>

<script>
import Vue from 'vue'

var menuItem = Vue.component('menu-item', {
  props: ['display-name'],
  template: '<li>{{ display-name }}</li>',
  data () {
    return {
      menuItems: [
        {'id': 1, 'name': 'Home', 'link-to': '/'},
        {'id': 2, 'name': 'Contact', 'link-to': '/contact'}
      ]
    }
  }
})

export default {
  name: 'MyApp',
  components: { menuItem },
  data () {
    return {
      msg: 'Welcome to Bookue'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
