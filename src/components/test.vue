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
    <hr />
    <h1>Gallery</h1>
    <section>
      <aside v-for="image in images" :key="image.id">
        <h3>{{image.caption}}</h3>
        <h3>{{image.date}}</h3>
        <img v-bind:src="image.baseimageurl"  alt="Smiley face" height="image.height" width="image.width/2" />
      </aside>
      <button v-model="object" v-on:click="change (object.next)" >next</button>
    </section>
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
      images: [],
      object: {}
    }
  },
  methods: {
    greet: function () {
      this.$http.get('https://api.harvardartmuseums.org/image?apikey=f0eebd50-e119-11e8-abe4-37afcc19e5ee').then(function (data) {
        console.log(data.body)
      })
    },
    pressKey: function (e) {
      console.log(e.target.value)
    },
    enterHit: function () {
      console.log('You hit the enter key boo')
    },
    change: function (url) {
      this.$http.get(url).then(function (data) {
        this.images = data.body.records
        this.object = data.body.info
      })
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
      this.object = data.body.info
    })
  }
}
</script>

<style scoped>

</style>
