# Frontend Tech Challenge: The Movie Showcase

## Installation

The proof-of-concept is build using [Svelte](https://svelte.dev) + TypeScript and [Tailwind](https://tailwindcss.com) since it is the stack that you guys mainly use and I thought it would be a nice challenge to build this and learn both Svelte and Tailwind. 
Make sure there is an .env file present with an API-key (`PUBLIC_API_KEY`) for the [TheMovieDB](https://www.themoviedb.org), after making an account obtain an API-key [here](https://www.themoviedb.org/settings/api). 

```bash
npm install
npm run dev
```

## Todo if it wasnt a POC
  *  When a specific movie is displayed make the background a video instead of the standard backdrop. 
  *  Make a seperate class for obtaining the API data so there is less duplicate code. 
  *  Do more research into properly structuring the application. 
  *  Focus more on responsive. The focus for the assignment was larger screens so I have not payed much attention to responsive and how tailwind deals with mediaqueries. 
  *  More error handling.
  *  Tests.

## In short

A customer who runs a small local movie bluray/dvd shop (yes, that still exists in 2024!) wants you to setup a fancy, schmancy app that can be used to display several currently upcoming movies on several displays. The idea is to display those in an automated fashion, for example for store visitors or guests as expositions. The customer is specialised in the selling of special editions movies, like Criterion-like Bluray collectables. Their end-customers, and actual target audience, are actual movie collectors and enthousiasts. And we may assume are nitpicky on visual aesthetics. In a narrowcasting, or slideshow fashion, certain recent movies will be displayed in a tempting fashion. What you want to show - and how you want to show it - is for the proof of concept up for your own interpretation. What would make it look good? It is your assignment to bring up a working such a display. 


## Assignment
The app will connect to a publicly available movie database API and query some of the latest and greatest movies or tv shows that will be used. For each movie or show, it will neatly display the title of the movie, the most important metadata.

> Which movies, or tv-shows, will be shown can be arbitrary. It’s just a proof-of-concept for now. 
> As long as the data is fetched dynamically through the API.

The end target for the app is to:
  *  Provide a GitHub repo containing the end result in the preference. `main` branch. Use English as language.
  *  The project should run, perhaps with simple instructions from the README, after cloning the repo.
  *  You don’t have to concern yourself with hosting or deployment; as long as it runs on our dev machines, you’re fine
  *  It should be runnable on any system: Windows, Linux, macOS.
  *  Use any frontend framework or libraries you like. If you find it hard to choose, within SI we default these days to [Svelte](https://svelte.dev) + TypeScript, with optionally [Tailwind](https://tailwindcss.com).

## What we’ll be looking for
Obviously, this is a fictional scenario. But here are some tips on what we’ll be looking for when reviewing the final solution:
  *  Is it working: does your solution run out-of-the-box
  *  The way your commits and git history are structured. 
  *  What tools, or libraries have you used
  *  Code (best) practices, ease of readability and styling structure.
  *  Is it “pretty”: very subjective, but what efforts are made at making it look good. 