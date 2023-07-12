index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Netflix Clone</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <!-- Navabar -->
    <div class="nav" id="nav">
      <img src="NetflixLog.png" alt="" class="logo" />
      <img src="NavLo.png" class="sideIcon" alt="" />
    </div>

    <!-- banner -->
    <header id="banner">
      <div class="banner-content">
        <h1 id="banner-title"></h1>
        <div class="banner-buttons">
          <button class="banner-button">Play</button>
          <button class="banner-button">My List</button>
        </div>
        <h1 id="banner-description"></h1>
      </div>
      <div class="banner-fade"></div>
    </header>

    <!-- Row -->
    <div class="row" id="row0">
      <h2>Netflix Originals</h2>
      <div class="image-container" id="image0"></div>
    </div>
    <div class="row" id="row1">
      <h2>Trending</h2>
      <div class="image-container" id="image1"></div>
    </div>
    <div class="row" id="row2">
      <h2>Top Rated</h2>
      <div class="image-container" id="image2"></div>
    </div>
    <div class="row" id="row3">
      <h2>Action Movies</h2>
      <div class="image-container" id="image3"></div>
    </div>
    <div class="row" id="row4">
      <h2>Comedy Movies</h2>
      <div class="image-container" id="image4"></div>
    </div>
    <div class="row" id="row5">
      <h2>Horror Movies</h2>
      <div class="image-container" id="image5"></div>
    </div>
    <div class="row" id="row6">
      <h2>Romance Movies</h2>
      <div class="image-container" id="image6"></div>
    </div>
    <div class="row" id="row7">
      <h2>Documanteries</h2>
      <div class="image-container" id="image7"></div>
    </div>

    <!-- About -->
    <div id="container" class="container">
      <div id="moviePoster">
        <img id="moviePosterImg" src="" alt="" />
      </div>
      <div class="details">
        <h2 id="title"></h2>
        <div class="small-details">
          <p id="date"></p>
          <p id="language"></p>
          <p id="rating"></p>
        </div>
        <h1 id="description"></h1>
        <button id="play">Play</button>
      </div>
      <button id="close">x</button>
    </div>

    <script src="main.js"></script>
  </body>
</html>


const banner = document.getElementById("banner");
const bannerTitle = document.getElementById("banner-title");
const bannerDescription = document.getElementById('banner-description');

const title = document.getElementById("title");
const date = document.getElementById("date");
const language = document.getElementById("language");
const rating = document.getElementById("rating");
const description = document.getElementById("description");
const close = document.getElementById('close');
const container = document.getElementById("container");
const nav = document.getElementById("nav")

const API_KEY = "e64f3b3e26f432be01ccdbacadd96982";
const baseURL = "https://api.themoviedb.org/3";
const imageUrl = "https://image.tmdb.org/t/p/original/";

const requests = [
    `/discover/tv?api_key=${API_KEY}&with_networks=213`,
    `/trending/all/week?api_key=${API_KEY}&language=en-US`,
    `/movie/top_rated?api_key=${API_KEY}&language=en-US`,
    `/discover/movie?api_key=${API_KEY}&with_genres=28`,
    `/discover/movie?api_key=${API_KEY}&with_genres=35`,
    `/discover/movie?api_key=${API_KEY}&with_genres=27`,
    `/discover/movie?api_key=${API_KEY}&with_genres=10749`,
    `/discover/movie?api_key=${API_KEY}&with_genres=99`
]


// fetching data for rows
async function fetchData() {
    for (let i = 0; i < 8; i++) {
        const response = await fetch(`${baseURL}${requests[i]}`);
        const data = await response.json();
        var movies = data.results;
        console.log(movies);
        createRows(movies, i)
    }
    return movies
}
fetchData();

// adding data in rows
function createRows(movies, i) {
    movies.map((movie) => {
        const imag = document.getElementById(`image${i}`);
        const poster = document.createElement("img");
        if (i > 0) {
            poster.src = `${imageUrl}${movie.backdrop_path}`
        } else {
            poster.src = `${imageUrl}${movie.poster_path}`;
            poster.classList.add('largeImage');
        }
        poster.classList.add('image')
        imag.appendChild(poster);

        // Creating about section
        poster.addEventListener("click", function () {
            console.log(movie)
            container.style.display = "flex";
            const moviePosterImg = document.getElementById("moviePosterImg");
            moviePosterImg.src = `${imageUrl}${movie.poster_path}`;
            title.textContent = (movie.name || movie.title || movie.original_name);
            date.textContent = movie.first_air_date;
            language.textContent = movie.original_language;
            rating.textContent = movie.vote_average;
            description.textContent = movie.overview
        })
    })
}

// Fetching data for banner
async function fetchBannerData() {
    const bannerResponse = await fetch(`${baseURL}${requests[0]}`);
    const bannerData = await bannerResponse.json();
    var banners = bannerData.results;
    console.log(banners);
    const select = banners[Math.floor(Math.random() * banners.length)]
    console.log(select);
    banner.style.backgroundImage = `url(${imageUrl}${select.backdrop_path})`;
    bannerTitle.textContent = select.name;
    bannerDescription.textContent = select.overview;

}
fetchBannerData()

// closing the about element
close.addEventListener("click", function () {
    container.style.display = "none"
})

//navbar animation
window.addEventListener("scroll", function () {
    if (window.scrollY > 100) {
        nav.style.background = "#111";
    } else {
        nav.style.background = "none";
    }
})

style css
* {
  margin: 0;
}
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  background-color: #111;
  color: white;
}
#banner {
  background-size: cover;
  color: white;
  height: 448px;
}

.banner-content {
  margin-left: 30px;
  padding-top: 140px;
  height: 190px;
}

#banner-title {
  font-size: 3rem;
  font-weight: 800;
  font-family: monospace;
  padding-bottom: 0.3rem;
}

#banner-description {
  width: 45rem;
  line-height: 1.3rem;
  padding-top: 1rem;
  height: 80px;
  font-size: 0.8rem;
  max-width: 360px;
}

.banner-button,
button {
  cursor: pointer;
  color: #fff;
  outline: none;
  border: none;
  margin-right: 1rem;
  font-weight: 700;
  border-radius: 2px;
  background-color: rgba(51, 51, 51, 0.5);
  padding: 0.5rem 2rem;
}

.banner-button:hover,
#play:hover {
  color: #000;
  background-color: #e6e6e6;
  transition: all 0.2s;
}

.banner-fade {
  height: 7.4rem;
  background-image: linear-gradient(
    180deg,
    transparent,
    rgba(37, 37, 37, 0.61),
    #111
  );
}

.row {
  margin-left: 20px;
  margin-top: 5px;
}

.image-container {
  display: flex;
  padding: 10px;
  overflow-x: scroll;
  overflow-y: hidden;
}

.image-container::-webkit-scrollbar {
  width: 0; /*Remove scrollbar space*/
  background: rgba(0, 0, 0, 0.1);
}

.image {
  width: 100%;
  max-height: 100px;
  object-fit: contain;
  margin-right: 10px;
  transition: transform 450ms;
}

.image:hover {
  transform: scale(1.08);
}
.largeImage {
  max-height: 250px;
}

.container {
  width: 60%;
  display: none;
  justify-content: space-between;
  max-height: 500px;
  margin: 20px auto;
  background-color: black;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

#moviePoster img {
  max-height: 500px;
}

.details {
  padding: 10px 20px;
}
#title {
  font-size: 3rem;
  font-weight: 800;
  font-family: monospace;
}

.small-details {
  display: flex;
  padding: 0px;
}

#language {
  border-left: 2px solid white;
  border-right: 2px solid white;
}

.small-details p {
  padding: 1px 10px;
}

#description {
  font-size: 0.8rem;
  padding-top: 1rem;
  line-height: 1.3rem;
}

#play {
  margin-top: 20px;
}

#close {
  position: relative;
  right: 10px;
  height: fit-content;
  top: 10px;
  border: none;
  border-radius: 150px;
  font-size: 20px;
  padding: 5px 12px;
  cursor: pointer;
  font-weight: 600;
}

/* Navbar  */
.nav {
  width: 100%;
  display: flex;
  justify-content: space-between;
  position: fixed;
  top: 0;
  height: 30px;
  padding: 20px;
  transition-timing-function: ease-in;
  transition: all 0.5s;
}
.logo {
  width: 80px;
  object-fit: contain;
  position: fixed;
  left: 20px;
  top: 10px;
}

.sideIcon {
  object-fit: contain;
  width: 40px;
  position: fixed;
  right: 20px;
}
