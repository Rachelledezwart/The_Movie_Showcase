<script lang="ts">
  import { PUBLIC_API_KEY } from "$env/static/public";
  import { onMount } from "svelte";

    interface Genre {
        id: number;
        name: string;
    }

    export let movieID;
    let movieRunTime: string;
    let movieGenres: Genre[] = [];
    
    const API_URL = `https://api.themoviedb.org/3/movie/${movieID}`;

    onMount(async () => {
        await fetch(`${API_URL}?language=en-US&api_key=${PUBLIC_API_KEY}`)
        .then( response => response.json() )
        .then( data =>  { movieRunTime = data.runtime, movieGenres = data.genres} )
        .catch(error => {
            console.log(error);
            return [];
        });
    }); 
</script>

<div class="tag bg-white text-slate-900 align-middle rounded-full py-1 px-4 mr-4 leading-7">
    {movieRunTime + " Minutes"}
</div>

{#if movieGenres}
    {#each movieGenres.slice(0,3) as genre}
        <div class="tag border-white align-middle border-2 mx-3 rounded-full py-1 px-4">
            {genre.name}
        </div>
    {/each}
{/if}