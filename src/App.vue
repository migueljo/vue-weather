<template>
  <div id="app" :class="getAppClass()">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{ weather.sys.country }}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        api_key: "bad572d586f319f477af8273228a928a",
        api_base: "https://api.openweathermap.org/data/2.5/",
        query: "",
        weather: {}
      }
    },
    methods: {
      fetchWeather(e) {
        if (e.key === "Enter") {
          fetch(`${this.api_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(response => response.json())
            .then(this.setResults)
        }
      },
      setResults(data) {
        this.weather = data
      },
      dateBuilder() {
        return new Date().toDateString()
      },
      getAppClass() {
        return typeof this.weather.main !== 'undefined' && this.weather.main.temp > 16 ? 'warm' : ''
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

  body {
    font-family: "montserrat", sans-serif;
  }

  #app {
    background-image: url("./assets/cold-bg.jpg");
    background-size: cover;
    background-position: center;
    transition: all 0.4s
  }

  #app.warm {
    background-image: url("./assets/warm-bg.jpg");
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: transparent;
    background: transparent;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  }

  .search-bar:focus {
    border-radius: 16px 0 16px 0;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
  }

  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: white;
    font-size: 20px;
    font-weight: normal;
    font-style: italic;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather {
    color: white;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

</style>
