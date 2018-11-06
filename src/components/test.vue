/* eslint-disable no-alert, no-console */

<template>
  <div>
    <h1>GALLERY</h1>
    <button v-on:click="change (object.next)" >next</button>
    <button v-on:click="showCurated" >curated</button>
    <div class="gallery" >
      <aside v-for="obj in galleryImg" :key="obj.id">
        <p contenteditable="true">add caption</p>
        <img  v-bind:src="obj.image"  alt="obj.caption" height="{image.height" width="image.width" class="glryImg" />
      </aside>
    </div>
    <hr />
    <h2>Curate Your Own Gallery</h2>
    <h3>click an image to add it to your gallery</h3>
    <section>
      <aside v-for="image in images" :key="image.id">
        <h3>{{image.caption}}</h3>
        <h3>{{image.date}}</h3>
        <img v-bind:src="image.baseimageurl"  alt="Smiley face" height="image.height" width="image.width/2" v-on:click="highlight" />
      </aside>
      <button v-on:click="change (object.next)" >next</button>
      <button v-on:click="showCurated" >curated</button>
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
      images: [],
      object: {},
      active: false,
      galleryImg: []
    }
  },
  methods: {
    showCurated: function () {
      document.querySelector('.gallery').style.height = '100vh'
      document.querySelector('.glryImg').style.width = '40vw'
      document.querySelector('section').hidden = true
    },
    change: function (url) {
      this.$http.get(url).then(function (data) {
        this.images = data.body.records
        this.object = data.body.info
      })
      document.querySelector('.gallery').style.height = '15rem'
      document.querySelector('.glryImg').style.width = '8vw'
      document.querySelector('section').hidden = false
    },
    highlight: function (e) {
      const imgObj = { image: e.target.src, caption: '', id: Date.now() }
      this.galleryImg.push(imgObj)
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
  img {
    width: 40vw;
    cursor: pointer;
  }
  button {
    box-sizing: border-box;
    appearance: none;
    background-color: transparent;
    border-radius: 0.6em;
    cursor: pointer;
    align-self: center;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1;
    margin: 20px;
    padding: 1em 1em;
    text-decoration: none;
    text-align: center;
    text-transform: uppercase;
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
    border-color: #3498db;
    color: #fff;
    box-shadow: 0 0 40px 40px #3498db inset, 0 0 0 0 green;
    transition: all 150ms ease-in-out;
    width: 12rem;
    max-height: 4rem;
  }
  button:hover {
    color: purple;
    outline: 0;
    box-shadow: 0 0 10px 0 #3498db inset, 0 0 10px 4px green;
  }
  .gallery {
    height: 15rem;
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    overflow: scroll;
  }
  .glryImg, p {
    width: 8vw;
    margin: 0 .2rem;
  }

  p {
    height: 2.5rem;
    margin-bottom: .5rem;
    overflow: scroll;
  }
</style>
