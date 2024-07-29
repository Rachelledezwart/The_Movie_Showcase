<script lang="ts">

    import { onMount } from "svelte";
    import { PUBLIC_APIKEY } from "$env/static/public";
    import Movie from "./movie.svelte";
    
    let movies: any[] = [];
    const url = `https://api.themoviedb.org/3/movie/popular?api_key=${PUBLIC_APIKEY}`; 

    let childDivs: HTMLDivElement[] = [];
    let currentIndex = 0;

    onMount(async () => {
        await fetch(url)
        .then( response => response.json() )
        .then( data => {movies = data.results} )
        .catch(error => {
            console.log(error);
            return [];
        });

        childDivs = Array.from(document.querySelectorAll('.movie-container > div'));

        setTimeout(() => {
            applyClassToDivs();
        }, 10000);
    }); 

    function applyClassToDivs() {
        if (childDivs.length === 0) return;

        const currentDiv = childDivs[currentIndex];
        currentDiv.classList.add('active');

        setTimeout(() => {
            currentDiv.classList.remove('active');
            currentIndex = (currentIndex + 1) % childDivs.length;

            if (currentIndex === 0) {
                setTimeout(() => {
                    childDivs.forEach(div => div.classList.remove('active'));
                    setTimeout(() => {
                        applyClassToDivs();
                    }, 20000); // 20 seconds to display the "main" section
                }, 20000);
            } else {
                applyClassToDivs();
            }
        }, 20000); // 20 seconds for each div
  }
</script> 

{#if movies}
    <section class="movie-container h-screen flex">
        {#each movies.slice(0,4) as movie, i}
            <Movie {movie} />
        {/each}
    </section>
{/if}

<style>
    :global(.active) {
        flex-basis: 100%;
    }
    :global(.active .movie-logo) {
        opacity: 0;
    }
    :global(.active .movie-content) {
        opacity: 100;
    }
</style>