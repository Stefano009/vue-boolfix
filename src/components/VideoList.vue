<template>
  <div>
      <h1>FILM</h1>
      <ul>
        <li v-for="(video, index) in videosArray" :key="index">
            <div class="card-front">
                <img :src="poster(video.poster_path)" :alt="video.title">
            </div>
            <div class="card-back">
                <h3>Titolo: {{video.title}}</h3>
                <h3> Titolo originale:  {{video.original_title}}</h3>
                <country-flag :country='getFlag(video.original_language)' size='small'/>
                <!-- capire perchè si duplicano le chiavi -->
                <i v-for="(n, index) in stars(video.vote_average)" :key="index" class="fas fa-star"></i>
                <div v-if="(stars(video.vote_average) < 5)" class="emptyStar">
                <i v-for="(n, index) in (5-(stars(video.vote_average)))" :key="index" class="far fa-star"></i>
                </div>
            </div>
        </li>
      </ul>
      <h1>TV SERIES</h1>
      <ul>
          <li v-for="(tv, l) in tvArray" :key="l">
            <div class="card-front">
                  <img :src="poster(tv.poster_path)" :alt="tv.title">
            </div>
            <div class="card-back">
                <h3>Titolo: {{tv.name}}</h3>
                <!-- <h3>{{imgUrl+tv.poster_path}}</h3> --> 
                <h3> Titolo originale:  {{tv.original_name}}</h3>
                <!-- <span> la lingua è {{tv.original_language}}</span> -->
                <country-flag :country='getFlag(tv.original_language)' size='small'/>
                <!-- capire perchè si duplicano le chiavi -->
                <i v-show="(stars(tv.vote_average) != 0)" v-for="(n, index) in stars(tv.vote_average)" :key="index" class="fas fa-star"></i>
                <div v-if="(stars(tv.vote_average) < 5)" class="emptyStar">
                <i v-for="(n, index) in (5-(stars(tv.vote_average)))" :key="index" class="far fa-star"></i>
                </div>
            </div>
            
          </li>
      </ul>
  </div>
</template>

<script>
import CountryFlag from 'vue-country-flag';
// import Card from './Card.vue';
export default {
    name: 'VideoList',
    data() {
       return {
           imgUrl: 'https://image.tmdb.org/t/p/w342/',
       }
    },
    components:{
        CountryFlag,
        // Card,
    },
    props: {
        videosArray : Array,
        tvArray : Array
    },
    methods:{
         getFlag(language){
            if(language=="en")return "gb"
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
    h1 {
            text-align: center;
            color:gray;
        }
    ul {
        height: 100%;
        width:70%;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        list-style: none;
        li {
            display: flex;
            width:calc(100vw / 9);
            height: 250px;
            border: 1px solid white;
            box-shadow: 4px 4px 20px black;
            overflow: hidden;
            h3 {
                padding:5px 0;
                font-size: .8rem;
            }
        }
        div img,
        img {

            width:calc(100vw / 9);
            height: 100%;
            object-fit: fill;
            background-color:black
        }
        .emptyStar {
            background-color: #d3cfcf;
            display:inline;   
        }
        .card-back {
            background-color:black;
            width:calc(100vw / 9);
            height: 250px;
            color:white;
            font-size: .9rem;
            padding: 0 10px;
        }
        .card-back i,
        .card-back div {
            color:gold;
            background-color: black;

        }
        // li:hover div img,
        // li:hover .card-front,
        // {
        //     transition-duration: 1s;     
        // }
        li:hover .card-front {
            animation: myAnim .5s ease-in-out 0s 1 normal forwards;
        }
        @keyframes myAnim {
            0% {
                transform: scaleX(1);
                width:100%;
                // height: 100%;
                transform-origin: 0% 50%;
            }

            100% {
                transform: scaleX(0);
                width:0;
                // height: 0;
                transform-origin: 0% 50%;
            }
         }
    }
}
</style>

 