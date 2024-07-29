<script lang="ts">
    import { onMount } from "svelte";
    import { PUBLIC_APIKEY } from "$env/static/public";
    import Movie from "./movie.svelte";
    
    let movies: any[] = [];
    const url = `https://api.themoviedb.org/3/movie/popular?api_key=${PUBLIC_APIKEY}`; 

    let childDivs: HTMLDivElement[] = [];
    let currentIndex = 0;
    let countdown = 0;
    let intervalId: number;
    let countdownWidth = 100; // Initial width of the countdown bar in percentage

    const timeBetweenMovies: number = 20; // in seconds

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
            startCountdown(timeBetweenMovies);
            applyClassToDivs();
        }, timeBetweenMovies * 1000);
    }); 

    function startCountdown(duration: number) {
        countdown = duration;
        countdownWidth = 100;
        clearInterval(intervalId);

        intervalId = setInterval(() => {
            countdown -= 1;
            countdownWidth = (countdown / duration) * 100;
            if (countdown <= 0) {
                clearInterval(intervalId);
            }
        }, 1000);
    }

    function applyClassToDivs() {
        if (childDivs.length === 0) return;

        const currentDiv = childDivs[currentIndex];
        currentDiv.classList.add('active');

        startCountdown(timeBetweenMovies);
        setTimeout(() => {
            currentDiv.classList.remove('active');
            currentIndex = (currentIndex + 1) % childDivs.length;

            if (currentIndex === 0) {
                // All divs have been active, now remove the class from all divs for a break period to display "main" screen
                setTimeout(() => {
                    childDivs.forEach(div => div.classList.remove('active'));
                    startCountdown(timeBetweenMovies);
                    setTimeout(() => {
                        applyClassToDivs();
                    }, timeBetweenMovies * 1000);
                }, 1000);
            } else {
                applyClassToDivs();
            }
        }, timeBetweenMovies * 1000);
    }
</script> 

{#if movies}
    <section class="movie-container h-screen flex">
        {#each movies.slice(0,4) as movie}
            <Movie {movie} />
        {/each}
    </section>
{/if}

<div class="countdown-bar bg-rose-800 absolute h-2 -mt-2" style="width: {countdownWidth}%;"></div>

<style>
    .countdown-bar {
        transition: width 1s linear; /* Smooth transition for the shrinking bar */
    }
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