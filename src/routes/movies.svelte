<script lang="ts">

    import { onMount } from "svelte";
    import { PUBLIC_APIKEY } from "$env/static/public";
    import MovieHeader from "./movieHeader.svelte";
    
    let movies: any[] = [];
    const url = `https://api.themoviedb.org/3/movie/popular?api_key=${PUBLIC_APIKEY}`; 

    onMount(async () => {
        await fetch(url)
        .then( response => response.json() )
        .then( data => {movies = data.results} )
        .catch(error => {
            console.log(error);
            return [];
        });
    }); 

</script> 

{#if movies}
    <section class="h-screen flex">
        {#each movies.slice(0,4) as movie, i}
            <MovieHeader {movie} slide={i} />
        {/each}
    </section>
{/if}

<!-- {#each movies.slice(0,4) as movie}
    <section class="h-screen bg-cover bg-no-repeat bg-center" style="background-image: url(https://image.tmdb.org/t/p/original{movie.backdrop_path});">
        <h2 class="font-bold">{movie.title}</h2>
        <span class="font-extralight">{movie.release_date}</span>
        <span class="font-extralight">{movie.vote_average}</span>
        <p>{movie.overview}</p>
    </section>
{/each} -->