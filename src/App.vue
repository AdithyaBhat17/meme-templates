<template>
  <div id="app">
    <div class="container">
      <div class="row masonry-grid">
        <div class="col-md-4 col-sm-12 masonry-column" v-bind:key="meme.id" v-for="meme in templates">
          <div>
            <a @click="downloadTemplate(meme.url, meme.name)" href="javascript:void(0)" class="thumbnail" download>
              <img :src="meme.url" v-bind:alt="meme.name">
              <span>{{meme.name}}</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <!-- <router-view/> -->
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      loading: true,
      templates: []
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      fetch('https://api.imgflip.com/get_memes')
      .then(response => response.json())
      .then(templates => this.templates = templates.data.memes)
      .then(this.loading = false)
    },
    downloadTemplate(image, name) {
      fetch(image)
      .then(response => response.blob())
      .then((blob) => {
        if (window.navigator && window.navigator.msSaveOrOpenBlob) // for IE
           window.navigator.msSaveOrOpenBlob(blob, name)
        else {
          let a = document.createElement('a')
          a.href = URL.createObjectURL(blob)
          a.download = name
          a.click()
        }
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
img {
  width: 100%;
  height: 300px;
  object-fit: contain;
  margin-top: 25px;
  box-shadow: 5px 5px 25px 0 rgba(0, 0, 0, 0.04);
  border-radius: 5px;
}
</style>
