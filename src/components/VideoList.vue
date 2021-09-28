<template>
  <div class="wrapper">
      <h1>FILM</h1>
      <ul>
        <li  v-for="video in videosArray" :key="video.id">
            <div @mouseleave="toggleFunctionLeave" @click="toggleFunction(video.id)" class="inner">
                <div class="card-front">
                <img :src="poster(video.poster_path)" :alt="video.title">
                </div>
                <div class="card-back">
                    <div class="apiMovie" v-if="toggle">
                        <h3 v-show="(index < 5)" class="castNames" v-for="(credit, index) in movieCredits" :key="credit.id">
                                cast: {{credit.name}}
                        </h3 >
                        <h3 class="genres" v-for=" (genre, index) in video.genre_ids" :key="index">
                            {{checkGenre(genre)}}
                        </h3>
                    </div>
                    <div class="inner-card-back" v-else>
                        <h3>Titolo: {{video.title}}</h3>
                        <h3> Titolo originale:  {{video.original_title}}</h3>
                        <country-flag :country='getFlag(video.original_language)' size='small'/>
                        <!-- capire perchè si duplicano le chiavi -->
                        <div class="moveStars">
                        <i v-for="n in 5" :key="n" class="fa-star" :class="n <= stars(video.vote_average) ? 'fas' : 'far'"></i>
                        </div>
                        <p>{{video.overview}}</p> 
                    </div>  
                            
                </div>
            </div>
        </li>
      </ul>
      <h1>TV SERIES</h1>
      <ul>
          <li v-for="tv in tvArray" :key="tv.id">
            <div @mouseleave="toggleFunctionLeave" @click="toggleTv(tv.id)" class="inner">
                <div class="card-front">
                  <img :src="poster(tv.poster_path)" :alt="tv.title">
                </div>
                <div class="card-back">
                    <div class="apiMovie" v-if="toggle">
                        <h3 v-show="(index < 5)" class="castNames" v-for="(credit, index) in movieCredits" :key="credit.id">
                                cast: {{credit.name}}
                        </h3 >
                        <h3 class="genres" v-for=" (genre, index) in tv.genre_ids" :key="index">
                            {{checkGenre(genre)}}
                        </h3>
                    </div>
                        <div class="inner-card-back" v-else>
                            <h3>Titolo: {{tv.name}}</h3>
                        <!-- <h3>{{imgUrl+tv.poster_path}}</h3> --> 
                        <h3> Titolo originale:  {{tv.original_name}}</h3>
                        <country-flag :country='getFlag(tv.original_language)' size='small'/>
                        <!-- capire perchè si duplicano le chiavi -->
                        <div class="moveStars">
                        <i v-for="n in 5" :key="n" class="fa-star" :class="n <= stars(tv.vote_average) ? 'fas' : 'far'"></i>
                        </div>
                        <p>{{tv.overview}}</p>
                    </div>
                    <!-- <p v-if="toggle">{{videoCredits.cast.name}}</p>  -->
                </div>
            </div>            
          </li>
      </ul>
  </div>
</template>

<script>
import CountryFlag from 'vue-country-flag';
import axios from 'axios'
// import Card from './Card.vue';
export default {
    name: 'VideoList',
    data() {
       return {
           imgUrl: 'https://image.tmdb.org/t/p/w342/',
           movieCredits:[],
           tvCredits:[],
           toggle:false,
           movieId: [],
       }
    },
    created: function() {
    this.APICallMovieGenre() 
    // this.APICallTv(this.firstResult) 
    },
    components:{
        CountryFlag,
        // Card,
    },
    props: {
        videosArray : Array,
        tvArray : Array,
        option : String,
    },
    methods:{
        toggleFunction(index) {
            this.toggle = !this.toggle
            console.log(index)
            this.APICallMovieCredits(index)
        },
        toggleTv(index) {
            this.toggle = !this.toggle
            console.log(index)
            this.APICallTvCredits(index)
        },
        toggleFunctionLeave() {
            if(this.toggle == false)
            return

            this.toggle = !this.toggle;
            this.tvCredits = [];
            this.movieCredits = [];
        },
         getFlag(language){
            if(language=="en")return "gb"
            if(language=="ja")return "jp"
            return language
        },
        poster(string) {
            if (this.imgUrl+string === 'https://image.tmdb.org/t/p/w342/null'){
                return require('../assets/img/not_found.png')
            }
            return this.imgUrl+string;
        },
        stars(number) {
            let starsNumber = Math.ceil(number/2).toFixed(0);
            return parseInt(starsNumber);
        },
        APICallMovieCredits(filmId) {
                axios.get(`https://api.themoviedb.org/3/movie/${filmId}/credits?api_key=e99307154c6dfb0b4750f6603256716d&language=it_IT`)
                    .then(res => {
                        return this.movieCredits = res.data.cast
                    })
                    .catch(err => {
                        console.log(err)
                    })
          },
          APICallTvCredits(tvId) {
                axios.get(`https://api.themoviedb.org/3//person/${tvId}/tv_credits?api_key=e99307154c6dfb0b4750f6603256716d&language=it_IT`)
                    .then(res => {
                        return this.tvCredits = res.data.cast
                    })
                    .catch(err => {
                        console.log(err)
                    })
          },
          APICallMovieGenre() {
                axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=e99307154c6dfb0b4750f6603256716d')
                    .then(res => {
                        return this.movieId = res.data.genres
                    })
                    .catch(err => {
                        console.log(err)
                        })
          },
          checkGenre(id) {
              for(let i = 0; i < this.movieId.length; i++){
                  console.log(this.movieId[i].id)
                  if(this.movieId[i].id === id)
                    return this.movieId[i].name
              }
          }
    }
}
</script>

<style lang="scss" scoped>
.wrapper {
    height: 100vh;
    overflow-y: scroll;
    h1 {
            text-align: center;
            color:gray;
        }
    ul {
        width:70%;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        list-style: none;
        li{
            background-color: transparent;
            perspective: 1000px;
            width: calc(100vw / 9);
            height: 240px;
            border: 1px solid white;
            box-shadow: 4px 4px 20px #090909;
            overflow: hidden;
            text-align: center;
        }
        .inner {
            position: relative;
            display: inline-block;
            width:100%;
            height: 100%;
            transition: transform 0.8s;
            transform-style: preserve-3d;
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
            background-color:#090909
        }
        .emptyStar {
            background-color: #d3cfcf;
            display:inline;   
        }
        .card-back {
            background-color:#090909;
            margin: 30px auto;
            width:calc(100vw / 9);
            height: 100%;
            transform: rotateY(180deg);
            .moveStars::before {
                content: "Voto:";
                color: white;
                padding-right: 10px;
                font-weight: 700;
            }
            p::before {
                content: 'Overview: '; 
            }
            p {
                display: inline-block;
                overflow: hidden;
                white-space: nowrap;
                text-overflow: ellipsis;
                width: 100%;
                padding: 5px 0;
                font-size: .6rem;
            }
            
        }
        .card-back div {
            color:gold;
            background-color: #090909;
            margin: 0 auto;
        }
        // li:hover div img,
        // li:hover .card-front,
        // {
        //     transition-duration: 1s;     
        // }
        li:hover .inner {
            // animation: myAnim .5s ease-in-out 0s 1 normal forwards;
            transform: rotateY(180deg);
        }
        .card-front, .card-back {
            position: absolute;
            -webkit-backface-visibility: hidden; /* Safari */
            backface-visibility: hidden;
        }
        .card-front {
            .flip-card-front {
            background-color: #bbb;
            color: black;
            }
        }
        .apiMovie {
            height: 1rem;
            color: white;
        }
        .castNames {
            color: white;
            padding: 5px;

        }
        .inner-card-back {
                color:white !important;
                font-size: .9rem;
                padding: 0 10px;
            }
        .genres {
            color: red;
        }
        // @keyframes myAnim {
        //     0% {
        //         transform: scaleX(1);
        //         width:100%;
        //         // height: 100%;
        //         transform-origin: 0% 50%;
        //     }

        //     100% {
        //         transform: scaleX(0);
        //         width:0;
        //         // height: 0;
        //         transform-origin: 0% 50%;
        //     }
        //  }
    }
}
</style>