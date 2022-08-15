<template>
  <div id="app" :class="{ warm: isWarm }">
    <main id="main">
      <div class="search-box">
        <input
          type="text"
          name=""
          id="id"
          class="search-bar"
          placeholder="Search ...."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="location-box" v-if="weather.main">
        <div class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="date">{{ dateBuilder() }}</div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  data() {
    return {
      apiKey: "API_KEY",
      base_rul: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      isWarm: null,
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.base_rul}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      if (this.weather.main.temp > 20) {
        this.isWarm = true;
      } else {
        this.isWarm = false;
      }
    },
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

<style lang="css">
* {
  margin: 0;
  padding: 0;
  box-sizing: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
}

main {
  height: 90vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
  margin: auto;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
  margin: auto;
}

.search-box > .search-bar {
  display: block;
  width: 90%;
  padding: 15px;
  color: #313131;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  margin: auto;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}

.search-box > .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box {
  padding: 4%;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 15px;
  font-weight: 300;
  text-align: center;
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.25); */
  font-style: italic;
  padding: 2%;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
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
