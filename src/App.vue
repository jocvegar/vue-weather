<template>
  <div id="app" :class="typeof weather.main !='undefined' && weather.main.temp> 16 ?
    'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search...."
          v-model="query"
          @keypress="fetchWeather">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted() {
    console.log("aca")
    console.log(typeof process.env.VUE_APP_API_KEY)
    console.log("aca2")
  },
  data: function() {
    return {
      query: '',
      weather: {},
      base_url: 'http://api.openweathermap.org/data/2.5/',
    }
  }, 
  methods: {
    fetchWeather(e) {
      // console.log(e.key)
      if (e.key == 'Enter') {
        fetch(`${this.base_url}weather?q=${this.query}&units=metric&APPID=${process.env.VUE_APP_API_KEY}`)
        .then(response => {
          return response.json();
        }).then(this.setResults);

        this.query = ''
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather)
    },

    dateBuilder() {
      let date = new Date();
      let options = {weekday: 'long', month: 'long', day: 'numeric' };
      return date.toLocaleString('es-ES', options)
    }
  }
}
</script>

<style>
  * {
    margin:0;
    padding:0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size: auto;
    background-position: center;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0,0,0.25), rgba(0,0,0,0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0, 0,0,0.25);
    background-color: rgba(225,225,225,0.5);
    transition: 0.4;
  }

  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0,0,0.25);
    background-color:  rgba(225,225,225,0.75);
    border-radius: 16px;
  }

  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0,0,0.25);
  }

  .location-box .date {
    color: white;
    font-size: 20;
    font-weight: 300;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

   .weather-box .temperature {
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(225,225,225,0.25);
    border-radius: 16px;
    margin: 30px 0px;

    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  .weather-box .weather {
    color: white;
    font-size: 48px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0, 0,0,0.25);
  }

</style>
