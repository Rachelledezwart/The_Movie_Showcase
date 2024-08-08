<script lang="ts">
    import { PUBLIC_API_KEY } from "$env/static/public";
    import { onMount } from "svelte";
    import Movie from "./movie.svelte";

    interface Movie {
        id: number;
        title: string;
        overview: string;
        release_date: string;
        backdrop_path: string;
        vote_average: number;
    }

    let movies: Movie[] = [];
    const API_URL = `https://api.themoviedb.org/3/movie/popular`;

    let childDivs: HTMLDivElement[] = [];
    let currentIndex = 0;
    let countdown = 0;
    let intervalId: number;
    let countdownWidth = 100; // Initial width of the countdown bar in percentage

    const timeBetweenMovies: number = 20; // in seconds

    onMount(async () => {
        await fetch(`${API_URL}?api_key=${PUBLIC_API_KEY}`)
        .then( response => response.json() )
        .then( data => {movies = data.results} )
        .catch(error => {
            console.log(error);
            return [];
        });
        startCountdown(timeBetweenMovies);

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

<div class="countdown-bar bg-sky-800 absolute h-2 -mt-2" style="width: {countdownWidth}%;"></div>

<style>
    .countdown-bar {
        transition: width 1s linear;
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