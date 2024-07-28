<script lang="ts">
    import { PUBLIC_APIKEY } from "$env/static/public";
    import { onMount } from "svelte";

    export let movie;
    export let slide: number; 
    let movieImages: any = [];
    
    const url = `https://api.themoviedb.org/3/movie/${movie.id}/images?language=EN&api_key=${PUBLIC_APIKEY}`; 

    onMount(async () => {
        await fetch(url)
        .then( response => response.json() )
        .then( data => {movieImages = data.logos} )
        .catch(error => {
            console.log(error);
            return [];
        });
    }); 
</script>

<div class="movie-{slide} flex-1 bg-cover bg-no-repeat bg-center flex justify-center items-end overflow-hidden transition-all duration-1000" style="background-image: url(https://image.tmdb.org/t/p/original{movie.backdrop_path});">
    {#each movieImages.slice(0,1) as logo}
        <img alt="logo movie" class="lg:p-24 md:p-8 sm:p-4" src="https://image.tmdb.org/t/p/w500{logo.file_path}">
    {/each}
</div>



<style>
    .movie-0 {
        animation: movie0 60s linear infinite;
    }
    .movie-1 {
        animation: movie1 60s linear infinite;
    }
    .movie-2 {
        animation: movie2 60s linear infinite;
    }
    .movie-3 {
        animation: movie3 60s linear infinite;
    }

    @keyframes movie0 {
        0% {
            flex-basis: 0;
        }
        11% {
            flex-basis: 0;
        }
        13% {
            flex-basis: 100%;
        }
        35% {
            flex-basis: 100%;
        }
        37% {
            flex-basis: 0;
        }
    }
    @keyframes movie1 {
        35% {
            flex-basis: 0;
        }
        37% {
            flex-basis: 100%;
        }
        50% {
            flex-basis: 100%;
        }
        52% {
            flex-basis: 0;
        }
    }
    @keyframes movie2 {
        50% {
            flex-basis: 0;
        }
        52% {
            flex-basis: 100%;
        }
        75% {
            flex-basis: 100%;
        }
        77% {
            flex-basis: 0;
        }
    }
    @keyframes movie3 {
        75% {
            flex-basis: 0;
        }
        77% {
            flex-basis: 100%;
        }
        98% {
            flex-basis: 100%;
        }
        100% {
            flex-basis: 0;
        }
    }
</style>