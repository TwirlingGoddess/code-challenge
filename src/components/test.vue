/* eslint-disable no-alert, no-console */

<template>
  <div class="test">
    <input type="text" v-model="title"><br />
    <h1>{{title}}</h1>
    <p v-if="showName">{{user.firstName}}</p>
    <p v-else>Nobody</p>
    <ul>
      <li v-for="item in items" :key="item.id">{{item.title}}</li>
    </ul>
    <button v-on:click="greet">Say Greeting</button>
    <br />
    <input type="text" v-on:keyup="pressKey" v-on:keyup.enter="enterHit">
    <hr />
    <label>First Name: </label><input type="text" v-on:keyup.enter="enterHit" v-model="user.firstName">
    <label>Last Name: </label><input type="text" v-on:keyup.enter="enterHit" v-model="user.lastName">
    <h3>{{fullName}}</h3>
    <h2>{{msg}}</h2>
  </div>

</template>

<script>
export default {
  name: 'test',
  props: {
    msg: {
      type: String,
      default: 'this is default'
    }
  },
  data () {
    return {
      title: 'Hello World',
      user: {
        firstName: 'John',
        lastName: 'Doe'
      },
      showName: true,
      items: [
        {title: 'Item One'},
        {title: 'Item Two'},
        {title: 'Item Three'}
      ],
      images: []
    }
  },
  methods: {
    greet: function () {
      this.$http.get('https://api.harvardartmuseums.org/image?apikey=f0eebd50-e119-11e8-abe4-37afcc19e5ee').then(function (data) {
        console.log(data.body.records)
      })
    },
    pressKey: function (e) {
      console.log(e.target.value)
    },
    enterHit: function () {
      console.log('You hit the enter key boo')
    }
  },
  computed: {
    fullName: function () {
      return this.user.firstName + ' ' + this.user.lastName
    }
  },
  created () {
    this.$http.get('https://api.harvardartmuseums.org/image?apikey=f0eebd50-e119-11e8-abe4-37afcc19e5ee').then(function (data) {
      this.images = data.body.records
    })
  }
}
</script>

<style scoped>

</style>
