<template>
  <div id="app" :class="results.main!='undefined'&&temp>16?'warm':''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="请搜索..."
          v-model="searchText"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap">
        <div class="location-box">
          <div class="location">{{location}}</div>
          <div class="date">{{ dateBuild() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ temp }}°c</div>
          <div class="weather">{{ weather }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      api_key: "69b76c59675157f3892ba6874ff9dd05",
      api_url: "api.openweathermap.org/data/2.5/weather?q=",
      searchText: "",
      location: "Beijing",
      results: {},
      temp: "10",
      weather: "clear"
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `//api.openweathermap.org/data/2.5/weather?q=${this.searchText}&appid=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.results = results;
      this.location = results.name;
      this.temp = Math.round(results.main.temp - 273.15);
      this.weather = results.weather[0].main;
    },
    dateBuild() {
      let d = new Date();
      let months = [
        "一月",
        "二月",
        "三月",
        "四月",
        "五月",
        "六月",
        "七月",
        "八月",
        "九月",
        "十月",
        "十一月",
        "十二月"
      ];
      let days = [
        "星期一",
        "星期二",
        "星期三",
        "星期四",
        "星期五",
        "星期六",
        "星期日"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${year}年 ${month}${date}日 ${day}`;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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
  font-style: italic;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  border-radius: 0 16px 0 16px;
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
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
  font-size: 20px;
  font-weight: 400;
  text-align: center;
  margin-top: 10px;
  letter-spacing: 1px;
  font-style: italic;
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
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
