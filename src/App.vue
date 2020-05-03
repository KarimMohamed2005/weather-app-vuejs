<template>
  <div id="app" :class='typeof weather.main != "undefined" && getCelsius() > 16 ? "warm" : ""'>
    <main>
      <!-- Search Field -->
      <div class="searchbox">
        <input
          type="text"
          v-model="searchController"
          @keypress="fetchData"
          class="search-bar"
          placeholder="Search "
        />
      </div>
      <!-- Weather Section -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <!-- Location and Date -->
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>

          <!-- Temprature -->
          <div class="weather-box">
            <div class="temp">{{getCelsius()}}°C</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>

      <!-- Messages Section -->
      <div class="messages">
        <div class="falseCity" v-if="weather.message == 'city not found'">Please type a valid city.</div>
        <div
          class="noSearch"
          v-if="typeof weather.message === 'undefined' && typeof weather.main === 'undefined'"
        >Enter your city and search by Enter</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      apiKey: "55066d6049b008659ee115be66aabe0c",
      baseUrl: "https://api.openweathermap.org/data/2.5/",
      searchController: "",
      weather: {}
    };
  },
  methods: {
    fetchData(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.baseUrl}weather?q=${this.searchController}&units=metrics&APPID=${this.apiKey}`
        )
          .then(res => {
            console.log(res.json);

            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(result) {
      this.weather = result;
    },

    getCelsius() {
      var degree = Math.round(this.weather.main.temp);
      var celsius = degree - 273;
      return celsius;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
.weather-box {
  text-align: center;
}
#app.warm {
  background-image: url('./assets/images/warm-bg.jpg');
}

/* Temprature */
.weather-box .temp {
  text-align: center;
  font-size: 96px;
  padding: 15px;
  display: inline-block;
  color: white;
  margin: 25px 0px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 2px 3px rgba(0, 0, 0, 0.25);
  border-radius: 12px 12px 12px 12px;
}
/* Weather */
.weather-box .weather {
  color: white;
  font-size: 42px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
/* Country */
.location-box .location {
  font-size: 32px;
  text-align: center;
  font-weight: 500;
  color: #fff;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

/* Date */
.location-box .date {
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  color: #fff;
  text-shadow: rgba(0, 0, 0, 0.25);
}

/* Date */
.messages {
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  color: #fff;
  text-shadow: rgba(0, 0, 0, 0.25);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/images/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
  padding: 15px;
}
.searchbox {
  width: 100%;
  margin-bottom: 30px;
}

.searchbox .search-bar {
  display: block;
  width: 100%;
  box-shadow: 0px 0px 8px #313131;
  padding: 15px;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  color: #313131;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}

.searchbox .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px #313131;
  border-radius: 16px 16px 16px 16px;
}
</style>
