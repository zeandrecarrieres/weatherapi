<template>

  <div class="container">
    <img alt="weather logo" id="logo" src="./assets/logo.png">
    <aside>
      <label for="city" class="label">Type the name of the city:</label>
      <form class="search-location" v-on:submit.prevent='getWeather'>
        <input type="text" id="city" name="city" class="city" v-model="citySearch" autocomplete="off">
        <button type="submit" id="submit">Search</button>
      </form>

    </aside>
  
   
    <main>
      <div div class="content" :class="isDay ? 'day' :'night'">
        <div class="not-found" v-if="!searchResults">
          <h3>City not found</h3>
        </div>
        <header class="localisation-info" v-if="searchResults">
          <h1>{{weather.cityName}}</h1>
          <p> {{weather.country}}</p>
        </header>
        <section class="details"  v-if="searchResults">
          <h2> {{weather.temperature}}</h2>
          <p> {{weather.description}}</p>
          <p>{{weather.lowTemp}}</p>
          <p>{{weather.highTemp}}</p>
          <p>{{weather.humidity}}</p>
          <p>{{weather.feelsLike}}</p>
        </section>

        <div v-if="isDay" class="jour">
          <img src="./assets/day_bg.jpg" alt="">
        </div>
        <div v-else class="nuit">
          <img src="./assets/night_bg.jpg" alt="">
        </div>
        <p v-if="isDay"> </p>
        <p v-else> ZZZZzzzzzzz!!!!!!!</p>
      </div>
     
    </main>

  </div>

</template>

<script>


  export default {
    name: 'App',
    components: {

    },
    data() {
      return {
        searchResults: false,
        isDay: true,
        citySearch: "",
        weather: {
          cityName: "Bordeaux",
          country: "FR",
          temperature: 9,
          description: "Cloudy with a chance of meatballs",
          lowTemp: 0,
          highTemp: 15,
          feelsLike: "Death",
          humidity: 55
        },
      }
    },
    methods: {
    getWeather: async function() {
      console.log(this.citySearch)
      const key = "52b601814b5a1dcb467d9468625d306f"
      const callURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`

      //call API with try catch
      try {
        const response = await fetch(callURL)
        const data = await response.json()
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name; 
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description
        this.weather.lowTemp = Math.round(data.main.temp_min)
        this.weather.highTemp = Math.round(data.main.temp_max)
        this.weather.feelsLike = Math.round(data.main.feels_like)
        this.weather.humidity = Math.round(data.main.humidity)
        this.searchResults = true;

        const dayNight = data.weather[0].icon;
        if(dayNight.includes("d")){
          this.isDay = true;
        }
        else{
          this.isDay = false;
        }

        // this.cityDisplay = false

      }catch(error){
        console.log(error)
        this.searchResults = false
      }
    }
  }

  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;


  }

  .container {
    background: #c5e2ff;
    width: 100vw;
    height: 100vh;

  }

  #logo {
    width: 300px;
  }

  .label {
    font-size: 1rem;
    margin-right: 1rem;
    color: 21597F;
    font-weight: bold;
  }

  .city {
    font-size: .8rem;
    border-radius: 16px 0px 0px 16px;
    background-color: rgb(255, 255, 255);
    border: none;
    color: #21597F;
    padding: 0.5rem 1rem;
    width: 400px;
  }

  #submit {
    font-size: .8rem;
    border-radius: 0 16px 16px 0;
    background-color: #21597F;
    border: none;
    color: white;
    padding: 0.5rem 1rem;
  }

  main {

    padding: 5rem 0;
    margin-top: 5rem;
    border: 1px solid white;
  }

.content {
  display: flex;
  justify-content: center;
}
.nuit img {
  width: 300px;
  height: 200px;
  background: blue;
}

.jour img {
  width: 300px;
  height: 200px;
  ;
  
}


 
</style>