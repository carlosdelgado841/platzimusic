<template lang="pug">
#app
  img(src='https://carlosdelgado841.github.io/platzimusic/dist/logo.png')
  h1 {{ title }}
  select(v-model="selectedCountry")
    option(v-for="country in countries" :value="country.value") {{ country.name }}
  spinner(v-show="loading")
  ul
    //- li(v-for="artist in artists") {{ artist.name }}
    artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid") 

</template>

<script>
import getArtists from './api';
import Artist from './components/Artist'
import Spinner from './components/Spinner'
export default {
  name: 'app',
  data () {
    return {
      title: 'Platzi Music',
      artists: [],
      countries: [
        { name: "Colombia", value: "colombia" },
        { name: "España", value: "spain" },
        { name: "Argentina", value: "argentina" },
        { name: "Chile", value: "chile" },
        { name: "China", value: "china" }
      ],
      selectedCountry: "colombia",
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists: function(){
      const _self = this;
      _self.artists = [];
      _self.loading = true;
      getArtists(this.selectedCountry)
        .then(function (artists)  {
          _self.loading = false;
          _self.artists = artists;
        })
    }
  },
  mounted(){
    this.refreshArtists();
  },
  watch: {
    selectedCountry: function(){
      this.refreshArtists();
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
