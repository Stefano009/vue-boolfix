<template>
  <div id="app">
    <Nav
    />
    <VideoList
    :videosArray="queryResults"
     />
  </div>
</template>

<script>
import Nav from './components/Nav.vue'
import VideoList from './components/VideoList.vue'
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      flag: false,
      queryDigested: '',
      queryResults: [],
      APIUrl : 'https://api.themoviedb.org/3/search/',
        typeOfVideos: {
            film:'movie',
            tvSeries: ''
        },
        APILanguage:'&language=it',
        APIKey : '?api_key=45ceb3fa9940738343c6705b54fdb57f',
        query: '&query='
        }
  },
  components: {
    Nav,
    VideoList,
  },
  methods: {
    queryStorer(text) {
      this.queryDigested = text
    },
    APICall(queryStorer) {
                axios.get(this.APIUrl+this.typeOfVideos.film+this.APIKey+this.APILanguage+this.query+queryStorer)
                    .then(res => {
                    this.queryResults = res.data.results
                    })
                    .catch(err => {
                        console.log(err)
                    })
            }   
  }
}
</script>

<style lang="scss">

</style>
