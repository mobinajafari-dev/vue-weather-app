<template>
  <!-- starting weather app -->
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp < 16
        ? 'app-cold'
        : ''
    "
  >
    <main>
      <!-- search section -->
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="which city?"
          v-model="city"
          @keypress="fetchLocation"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <!-- location section -->
          <div class="location">
            {{ weather.name }} , {{ weather.sys.country }}
          </div>
          <!-- date section -->
          <div class="date">{{ dateBuilder() }}</div>
          <div class="weather-box">
            <!-- tempture section -->
            <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
            <!-- weather section -->
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  // required data
  data() {
    return {
      api_key: "d5c65cedcdbc91a462cb94305e005c02",
      url_location: "http://api.openweathermap.org/geo/1.0/",
      city: "",
      lat: "",
      lon: "",
      url_base: "https://api.openweathermap.org/data/2.5/",
      weather: {},
    };
  },
  methods: {
    // function for calculating the geo of the given city (lat , lon) from api

    fetchLocation(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_location}direct?q=${this.city}&appid=${this.api_key}`)
          .then((resGeo) => {
            return resGeo.json();
          })
          .then((responseGeo) => {
            this.lat = responseGeo[0].lat;
            this.lon = responseGeo[0].lon;
            return this.lat, this.lon;
          })
          .then(this.fetchWeather);
      }
    },

    // function for getting data from api about weather

    fetchWeather() {
      fetch(
        `${this.url_base}weather?lat=${this.lat}&lon=${this.lon}&units=metric&appid=${this.api_key}`
      )
        .then((resWeather) => {
          return resWeather.json();
        })
        .then((responseWeather) => {
          this.weather = responseWeather;
        });
    },

    // function for building date on the ui

    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
/* starting styles of the ui */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

/* background style */

#app {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app-cold {
  background-image: url("./assets/cold-bg.jpg") !important ;
}

/* main section style */

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

/* style of search-box */

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
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}

.search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

/* style of location title */

.location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

/* style of date title  */

.date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

/* style of tempture section */

.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #ffffff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

/* style of weather section */

.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
}

.weather-box {
  text-align: center;
}
</style>
