<template>
  <div>
      <h1>FILM</h1>
      <ul>
          <li v-for="(video, index) in videosArray" :key="index">
            <span>il titolo è {{video.title}}</span>
            <img :src="poster(video.poster_path)" :alt="video.title">
            <span> il titolo originale è {{video.original_title}}</span>
            <country-flag :country='getFlag(video.original_language)' size='small'/>
            <span  v-show="(stars(video.vote_average) != 0)" v-for="index in stars(video.vote_average)" :key="index"><i class="fas fa-star"></i></span> <span v-show="(5 - (stars(video.vote_average)) != 0)" v-for="index in (5-(stars(video.vote_average)))" :key="index"><i class="far fa-star"></i></span>
          </li>
      </ul>
      <h1>TV SERIES</h1>
      <ul>
          <li v-for="(tv, index) in tvArray" :key="index">
            <span>il titolo è {{tv.name}}</span>
            <!-- <span>{{imgUrl+tv.poster_path}}</span> -->
            <img :src="poster(tv.poster_path)" :alt="tv.title">
            <span> il titolo originale è {{tv.original_name}}</span>
            <!-- <span> la lingua è {{tv.original_language}}</span> -->
            <country-flag :country='getFlag(tv.original_language)' size='small'/>
            <span  v-show="(stars(tv.vote_average) != 0)" v-for="index in stars(tv.vote_average)" :key="index"><i class="fas fa-star"></i></span> <span v-show="(5 - (stars(tv.vote_average)) != 0)" v-for="index in (5-(stars(tv.vote_average)))" :key="index"><i class="far fa-star"></i></span> 
          </li>
      </ul>
  </div>
</template>

<script>
import CountryFlag from 'vue-country-flag'
export default {
    name: 'VideoList',
    data() {
       return {
           imgUrl: 'https://image.tmdb.org/t/p/w342/',
       
       }
    },
    components:{
        CountryFlag
    },
    props: {
        videosArray : Array,
        tvArray : Array
    },
    methods:{
         getFlag(language){
            if(language=="en")return "gb-eng"
            return language
        },
        poster(string) {
            console.log(this.imgUrl+string)
            if (this.imgUrl+string === 'https://image.tmdb.org/t/p/w342/null'){
                return require('../assets/img/not_found.png')
            }
            return this.imgUrl+string;
        },
        stars(number) {
            let starsNumber = (number/2).toFixed(0);
            console.log(starsNumber)
            return parseInt(starsNumber);
        }
    }
}
</script>

<style lang="scss" scoped>
div {
    background-color: gray;
    ul {
        height: 100%;
        display: flex;
        flex-wrap: wrap;
        list-style: none;
        li {
            width:calc(100vw / 9);
            height:calc(100vh / 4);
            margin: 20px;
            font-size: .5rem;
            border: 2px solid black;
            background-color: #d3cfcf;
        }
        img {
            height: 100px;
            width: 100px;
            object-fit: contain;
        }
    }
}
</style>