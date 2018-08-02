<template lang="pug">
    #app
      img(src='dist/logo2.png')
      h1 What's the world melomaniacs listening on lastfm?
      select(v-model="selectedCountry")  
        option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
      spinner(v-show="loading") 
      ul
        artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
      
</template>

<script>
  import artist from './components/artist.vue'
  import spinner from './components/spinner.vue'
  import getArtist from './api/index.js'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
      {name: 'Colombia', value:'colombia'},
      {name: 'Argentina', value:'argentina'},
      {name: 'Canada', value:'canada'},
      {name: 'Poland', value:'poland'},
      {name: 'Germany', value:'germany'}
      ],
      selectedCountry: 'colombia',
      loading: true
    }
  },
  components: {
    artist,
    spinner
  },
  methods: {
    refreshArtist(){
      const self = this
      this.loading = true
      this.artists = []
      getArtist(this.selectedCountry)
      .then(function (artists){
        self.loading = false
        self.artists = artists
      })
    }
  },
  mounted() {
    this.refreshArtist()
  },
  watch: {
    selectedCountry(){
      this.refreshArtist()
    }
  }
}
</script>

<style lang="stylus">
  #app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

  h1, h2
    font-weight normal

  ul
    list-style-type none
    padding 0

  li
    display inline-block
    margin 0 10px

  a
    color #42b983
</style>
