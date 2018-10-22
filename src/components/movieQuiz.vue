<template>
  <v-container fluid>
      <v-layout align-center justify-center mb-2>
        <h1 class="display-1">Which is more Popular?</h1>
      </v-layout>
  <v-layout align-center justify-center>
    <v-flex xs6 mr-5>
      <v-card px-30
        ripple
        @click.native="cardClicked(1)"
       >
       <!-- https://image.tmdb.org/t/p/w300/ -->
        <v-img
          v-if="movie1"
          v-bind:src="'https://image.tmdb.org/t/p/w300/' + movie1.poster_path"
        >
        </v-img>
      </v-card>
    </v-flex>
        <v-flex xs6>
      <v-card px-30
        ripple
        @click.native="cardClicked(2)"
       >
        <v-img
         v-if="movie2"
          v-bind:src="'https://image.tmdb.org/t/p/w300/' + movie2.poster_path" 
        >
        </v-img>
      </v-card>
    </v-flex>
  </v-layout>
   <v-alert
      :value="alert"
      :type="alertType"
      transition="scale-transition"
    >
      <h2>{{alertText}}</h2>
    </v-alert>

  <v-layout align-center justify-center>
    <v-btn v-if="isRight" color="info" v-on:click="nextQuestion">Next question!</v-btn>
  </v-layout>


  </v-container>
</template>

<script>

const axios = require('axios');



export default {
  name: "MovieQuiz",
  props: {
    msg: String
  },
  data(){
    return{
      title: "movie Sydney title Highlands",
      movies:null,
      something:"yoimsomething",
      movie1:null,
      movie2:null,
      alert:!alert,
      alertText:'',
      alertType:'success',
      isRight:null,

    }

  },
  methods: {
    cardClicked(cardClicked){
        let movieClicked=[];
        let otherMovie=[];

        if(cardClicked == 1){
            movieClicked = this.movie1;
            otherMovie = this.movie2;
        }else{
            movieClicked = this.movie2;
            otherMovie = this.movie1;
          }


          if(movieClicked.popularity <= otherMovie.popularity ){
            this.alertText = "You're Correct!";
            this.alertType= 'success';
            this.alert = alert;
            this.isRight= true;
            
          }else{
            this.alertText = "You're Wrong!";
            this.alertType= 'error';
            this.alert = alert;

          }
        
    },
    init(){
      //setup 
        this.alert = !alert; 
        this.isRight = false;    
      if (this.movies != null) {
          this.movie1 = this.movies[this.getRandomInt(20)];
          this.movie2 = this.movies[this.getRandomInt(20)];

      }
    },
    getRandomInt(max){
        return Math.floor(Math.random() * Math.floor(max));
    },
    nextQuestion(){
       this.init();
    },

  },
  mounted () {
    //bee4cab2d7302e1a1d94e8b2b5170b50
    axios
      .get('https://api.themoviedb.org/3/movie/top_rated?page=1&language=en-US&api_key=bee4cab2d7302e1a1d94e8b2b5170b50')
      .then(response => (this.movies = response.data.results,
      this.init()
      )
     
    );  
      
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

@media only screen and (min-width: 600px) {
.container.fluid {
    max-width: 50%;
}
}



</style>
