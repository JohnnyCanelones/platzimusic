<template lang="pug">
  #app
    img(src='https://johnnycanelones.github.io/platzimusic/dist/logo.png')
    h1 PlatziMusic
    h2 Pais
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul 
      artist( v-for="artist in artists" :artist="artist" :key="artist.mbid" ) 
</template>

<script>
import getArtist from './api'
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Venezuela', value: 'venezuela'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
      ],
      selectedCountry: 'venezuela',
      loading: true,
    }
  },
  methods: {
    refreshArtist(country) {
      const self = this
      this.loading = true
      this.artists = []
      getArtist(country)
        .then(function (artists){
          self.loading= false
          self.artists = artists
        })  
    }
  },
  mounted () {    
    this.refreshArtist(this.selectedCountry)
  },
  watch: {
    selectedCountry() {
      this.refreshArtist(this.selectedCountry)
    }
  },
  components: {
    Artist,
    Spinner
  },
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
    display flex
    flex-wrap wrap
    justify-content space-between
    padding 0px 46px
  li
    margin-top 60px !important
    padding 50px
    display inline-block
    border 3px dashed #41b883
  a 
    color #42b983
  

    
</style>
