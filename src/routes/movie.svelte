<script lang="ts">
    import { PUBLIC_API_KEY } from "$env/static/public";
    import { onMount } from "svelte";
    import StarRating from "./starRating.svelte";
    import MovieDetails from "./movieDetails.svelte";

    interface Logo {
        file_path: string;
        width: number;
        height: number;
    }

    export let movie;
    let movieLogos: Logo[] = [];

    const API_URL = `https://api.themoviedb.org/3/movie/${movie.id}/images`; 

    onMount(async () => {
        await fetch(`${API_URL}?language=EN&api_key=${PUBLIC_API_KEY}`)
        .then( response => response.json() )
        .then( data => { movieLogos = data.logos} )
        .catch(error => {
            console.log(error);
            return [];
        });
    }); 
</script>

<div class="movie-slide flex-1 bg-cover bg-no-repeat bg-center flex justify-center items-end overflow-hidden transition-all duration-1000 relative" style="background-image: url(https://image.tmdb.org/t/p/original{movie.backdrop_path});">
    {#each movieLogos.slice(0,1) as logo}
        <img alt="logo movie" class="movie-logo max-w-[500px] lg:p-24 md:p-8 sm:p-4" src="https://image.tmdb.org/t/p/w500{logo.file_path}">
    {/each}
    <div class="movie-content text-white absolute w-full h-full opacity-0 bg-cyan-950 bg-opacity-60 grid grid-cols-3 gap-56 items-center transition-all duration-1000 ">
        <div class="movie-exerpt min-w-[650px] col-span-2 p-14">
            <div class="movie-info px-12">
                <h2 class="text-[140px] font-extrabold opacity-40">
                    {movie.release_date.substr(0, 4)}
                </h2>
                <h1 class="text-5xl font-extrabold uppercase -mt-8">{movie.title}</h1>
                <div class="details flex flex-row pt-4 pb-2">
                    <MovieDetails movieID={movie.id} />
                </div>
                <div class="overview py-3 text-xl">
                    {movie.overview}
                </div>
                <div class="rating pr-4 font-normal text-slate-300 text-lg">
                    <StarRating rating={movie.vote_average} />
                </div>
            </div>
        </div>
        <div>
        </div>
    </div>
</div>

<style>
    h1, h2 {
        font-family: "Open Sans", sans-serif;
    }
    .movie-info {
        font-family: "Roboto", sans-serif;
    }
</style>