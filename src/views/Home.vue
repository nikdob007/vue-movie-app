<template>
    <div class="home">
        <div class="featured-card">
            <router-link to="/movie/tt0409591">
                <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg" class="featured-img" alt="">
                <div class="detail">
                    <h3>Naruto</h3>
                    <p>A High Quality Poster from our poster collection makes your gaming setup, office desk, study table or any wall look stunning.</p>
                </div>
            </router-link>
        </div>
        <form @submit.prevent="SearchMovies()" class="search-box">
            <input type="text" placeholder="what are you loking for!" v-model="search">
            <input type="submit" value="search">
        </form>

        <div class="movies-list">
            <div class="movie" v-for="movie in movies" :key="movie.imdbID">
                <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
                    <div class="product-image">
                        <img :src="movie.Poster" alt="Movie Poster">
                        <div class="type">{{ movie.Type}}</div>
                    </div>
                    <div class="detail">
                        <p class="year">{{ movie.Year }}</p>
                        <h3>{{ movie.Title}}</h3>
                    </div>
                </router-link>
            </div>
        </div>

    </div>
</template>

<script>
import { ref } from 'vue'
import env from '@/env.js'
export default {
    setup() {
        const search = ref("")
        const movies = ref([])

        const SearchMovies = () => {
            if(search.value != "") {
                fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
                .then(response => response.json())
                .then( data => {
                    movies.value = data.Search
                    search.value = ""
                })
            }
        }

         return {
             search,
             movies,
             SearchMovies
         }
    }
}
</script>

<style>
.home  .featured-card {
   position: relative;
}
.home  .featured-card .featured-img {
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;
    position: relative;
    z-index: 0;
}
.home  .featured-card .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0 , 0, 0, 0.6);
    padding: 16px;
    z-index: 1;
}
.home  .featured-card .detail h3 {
    color: #fff;
    margin-bottom: 16px;
}
.home  .featured-card .detail p {
    color: #fff;
}
.search-box {
    display: block;
    flex-direction: column;
    justify-content: center;
    padding: 16px;

}
.search-box input {
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;
}
.search-box input[type="text"] {
    width: 100%;
    color: #fff;
    background-color: #496583;
    font-size: 20px;
    padding: 10px 16px;
    border-radius: 8px;
    margin-bottom: 15px;
    transition: 0.4s;
}
.search-box input[type="text"]::placeholder {
    color: #f3f3f3;
}
.search-box input[type="text"]:focus{
    box-shadow: 0px 3px 6px rgba(0 , 0, 0, 0.2);
}
.search-box input[type="submit"] {
    width: 100%;
    max-width: 300px;
    padding: 16px;
    margin: 0 auto;
    background-color: #42B883;
    border-radius: 8px;
    color: #fff;
    font-size: 20px;
    text-transform: uppercase;
    transition: 0.4s;

}
.search-box input[type="submit"]:active {
    background-color: #388070;
}
.movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0 8px;
}
.movies-list .movie {
    max-width: 50%;
    flex: 1 1 50%;
    padding: 16px 8px;
}
.movies-list .movie .movie-link {
    display: flex;
    flex-direction: column;
    height: 100%;
}
.movies-list .movie .movie-link  .product-image {
    position: relative;
    display: block;
}
.movies-list .movie .movie-link  .product-image img{
    display: block;
    width: 100%;
    height: 275px;
    object-fit: cover;
}
.movies-list .movie .movie-link  .product-image .type {
    position: absolute;
    padding: 8px 16px;
    background-color: #42B883;
    color: #fff;
    bottom: 16px;
    left: 0;
    text-transform: capitalize;
}
.movies-list .movie .movie-link .detail {
    background-color: #496583;
    padding: 16px 8px;
    flex: 1 1 100%;
    border-radius: 0px 0px 8px 8px;
}
.movies-list .movie .movie-link .detail .year {
    color: #aaa;
    font-size: 14px;
}
.movies-list .movie .movie-link .detail h3 {
    color: #fff;
    font-weight: 600;
    font-size: 18px;
}
</style>