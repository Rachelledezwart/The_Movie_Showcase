<script lang="ts">
    import { PUBLIC_APIKEY } from "$env/static/public";
    import { onMount } from "svelte";

    export let movie;
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
        console.log(movieImages)
    }); 
</script>

<div class="flex-1 bg-cover bg-no-repeat bg-center flex justify-center items-end overflow-hidden transition-all duration-1000 hover:basis-full" style="background-image: url(https://image.tmdb.org/t/p/original{movie.backdrop_path});">
    {#each movieImages.slice(0,1) as logo}
        <img alt="logo movie" class="lg:p-24 md:p-8 sm:p-4" src="https://image.tmdb.org/t/p/w500{logo.file_path}">
    {/each}
</div>