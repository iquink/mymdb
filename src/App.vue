<template>
  <div id="container">
    <div id="headerContainer">
      <header id="header" class="dflex aligncenter spacebetween">
        <h1>20 лучших фильмов!</h1>
        <div><div>Based on:</div>
          <a
            href="https://www.themoviedb.org/"> 
            <img 
              src="https://www.themoviedb.org/assets/2/v4/logos/primary-green-d70eebe18a5eb5b166d5c1ef0796715b8d1a2cbc698f96d311d62f894ae87085.svg"
              alt="sourcelogo"
              id="sourceimage"
            >
          </a>
        </div>
      </header>
    </div>
    <div id="navContainer">
      <nav id="nav">
        <div>
          <label for="sortedByGenres">Выберите жанр:</label>
          <select id="sortedByGenres" name="sortedByGenres" v-model="genreSelected" @change="sortGenre">
            <option v-for="(genre,i) in genres" :key="i" v-bind:value="genre.name">{{genre.name}}</option>
          </select>
        </div>
        <div>
          <label for="sortedByDirector">Введите режиссёра:</label>
          <input type="text" name="sortedByDirector" v-model="directorSelected" @input="sortDirector" placeholder="Введите режиссёра">
        </div>
        <div>
          <label for="sortedByCast">Введите актёра:</label>
          <input type="text" name="sortedByCast" v-model="castSelected" @input="sortCast" placeholder="Введите актёра">
        </div>
      </nav>
    </div>
    <div id="mainContainer">
      <main id="main" class="dgrid">
        <div class='itemCard' v-show="movie.show" v-for="(movie,i) in movies" :key="i"> 
          <p class="title">{{ movie.title }}</p>
          <div class="titleImgContainer">
            <img class="titleImg" v-bind:src="'https://image.tmdb.org/t/p/w600_and_h900_bestv2'+movie.poster_path">
          </div>
          <p class="director">Режиссёр: <b>{{ movie.director }}</b></p> 
          <hr>
          <p class="cast">В ролях: <b>{{movie.cast[0]}}, {{movie.cast[1]}}, {{movie.cast[2]}}</b>  и другие</p>
          <hr>
          <p class="genre">Жанр: <span  v-for="(genre,i) in movie.genres" :key="i">
            <b>{{ genre.name }} </b>
          </span></p>
        </div>
      </main>
    </div>
  </div>
</template>

<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Open+Sans&display=swap');


#container {
  width: 100%;  
  font-family:  "Open Sans", Tahoma, sans-serif;  
  display:grid;
  grid-template-areas: 
  "head head"
  "navigation main";
  grid-template-columns: 200px 1fr;

}

@media screen and (max-width: 800px) {
  #container {
    display:block;
  }
}

.dgrid {
  display:grid;


}

.spacebetween {
  justify-content: space-between;
}

.aligncenter {
  align-items: center;
}

.dflex {
  display: flex;
}
#headerContainer {
  padding: 0 5%;
  background: rgb(0, 0, 0);
  color: rgb(255, 255, 255);
  grid-area: head;
  #header {

    width: 100%;
    height: 100px;
    
    

      h1 {
        margin:0;
        padding:0;
        font-size: calc(1.5rem + 6 * (100vw - 320px) / 880);
        letter-spacing: 5px;
        
        
      }

      #sourceimage {
        height: 70px;
        
      }
  }
}

#navContainer {
  background:rgb(179, 113, 15);
  padding: 10px 5%;
  height: 100%;
  margin-bottom: 10px;
  grid-area: navigation;
  

    input {
      width: 100%;
      height: 25px;
      margin-bottom: 5px;
      border:1px solid black;
      border-radius: 5px;
    }
     ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
        padding-left:5px;
        }
    ::-moz-placeholder { /* Firefox 19+ */
        padding-left:5px;
        }
    :-ms-input-placeholder { /* IE 10+ */
        padding-left:5px;
        }
     :-moz-placeholder { /* Firefox 18- */
        padding-left:5px;
        }
    select {
      width: 100%;
      height: 25px;
      margin-bottom: 5px;
      border:1px solid black;
       border-radius: 5px;
    }
    label {
      color:white;
      font-size: 20px;
    }
}

#mainContainer {
  padding: 10px 5%;
  color:#000000;
  grid-area: main;
  overflow-y: scroll;
  height: 80vh;

  

    #main {
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    grid-gap: 15px;
    width: 100%;
    .itemCard {
      width: 100%;
      border:1px solid rgba(211, 202, 202,0.5);
      background: #f3efef;

          .title {
        height: 40px;
        padding:0px 10px;
        font-weight:bold; 
        font-size:20px;

      }
      .director {
        padding:0px 10px;
      }
      .cast {
        padding: 0px 10px;
      }
      .genre {
        padding: 0px 10px;
      }
      p {
        margin:3px 0;
      }
      hr {
        width: 90%;
        border:1px solid rgba(211, 202, 202,0.5);
      }
  
        .titleImgContainer {
          padding:15px 15px;
          display:flex;
          justify-content: center;
            .titleImg {
              width: 100%;
        }
      }
    }
  }
}
  ::-webkit-scrollbar {
    width: 0px;  /* Remove scrollbar space */
    background: transparent;  /* Optional: just make scrollbar invisible */
}
@media screen and (max-width: 800px) {
  #mainContainer {
    height: 100%;
  }
}


</style>

<script>
import Axios from 'axios'

export default {
  data() {
    return{
      movies: [],
      genres: [],
      genreSelected: "",
      directorSelected: "",
      castSelected: ""
    }
  },
  created() {
    Axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=8e409bf9750ad60d1f84b3c57bb5bcdd&language=ru-RU')
      .then(response => {
      this.genres = response.data.genres;
      this.genres.unshift({"id":0,"name":"Показать все"})

    });
    Axios.get('https://api.themoviedb.org/3/movie/popular?api_key=8e409bf9750ad60d1f84b3c57bb5bcdd&language=ru-RU&page=1&region=ru')
      .then(response => {
        let results = response.data.results;
        let genres = this.genres;
        let popularMovies = results;
        for (let pm of popularMovies) { 
          let ids = pm.genre_ids;
          let filteredGenres = [];
          for (let g of genres) {
            if (ids.includes(g.id)) {
              filteredGenres.push(g);
            }
          }
          pm.genres = filteredGenres;
          pm.show = true;
          pm.sorted = true;
          pm.sortedbycast = true;
          pm.sortedbydirector = true;
          pm.director = '';
          pm.cast = [];
        }
        this.movies = popularMovies;
        let movies = this.movies;
        let director = [];
        for (let movie of movies) {
          let id = movie.id;
          Axios.get('https://api.themoviedb.org/3/movie/'+id+'/credits?api_key=8e409bf9750ad60d1f84b3c57bb5bcdd&')
            .then(response => {
              let crew = response.data.crew;
              let cast = response.data.cast;
              for (let item of crew) {
                if (item.job == "Director") {
                  movie.director = item.name;
                }
              }
              for (let item of cast) {
                movie.cast.push(item.name);
              } 
            }
          );
        }
        this.movies = movies;
      });
    },
  methods: {
    sortDirector: function() {
      let director = this.directorSelected;
      let movies = this.movies;
      for (let movie of movies) {
        if (movie.sorted === true && movie.sortedbycast === true) {
          movie.show = false;
          movie.sortedbydirector = false;
          let movieDirector = movie.director;
          if (movieDirector.toLowerCase().indexOf(director) > -1) {
          movie.show = true;
          movie.sortedbydirector = true;
          }
        }
      }
    this.movies = movies;
    },
    sortGenre: function() {
      this.directorSelected = "";
      this.castSelected = "";
      let genre = this.genreSelected;
      let movies = this.movies;
      for (let movie of movies) {
        movie.show = false;
        movie.sorted = false;
        movie.sortedbydirector = false;
        movie.sortedbycast = false;
        let genres = movie.genres;
        for (let g of genres) {
          if (g.name==genre) {
            movie.show = true;
            movie.sorted = true;
            movie.sortedbydirector = true;
            movie.sortedbycast = true;
          } else 
          if (genre === "Показать все") {
            movie.show = true;
            movie.sorted = true;
            movie.sortedbydirector = true;
            movie.sortedbycast = true;
          } 
        }
      }
    this.movies = movies;
    },   
    sortCast: function() {
     let castSelected = this.castSelected;
      let movies = this.movies;
      for (let movie of movies) {
        if (movie.sorted === true && movie.sortedbydirector === true) {
          movie.show = false;
          movie.sortedbycast = false;
          let movieCast = movie.cast;
          for (let cast of movieCast) {
            if (cast.toLowerCase().indexOf(castSelected) > -1) {
            movie.show = true;
            movie.sortedbycast = true;
            }
          }
        }
      }
    this.movies = movies;
    }  
  }
}   
    

</script>

