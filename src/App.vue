<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 14 ? 'warm' : ''">
    
  <div class="loader">
    <div class="planet"></div>
  </div>
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="showWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="weather">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="qoshimcha">His qilinishi: <span>{{ weather.main.temp_min }}°C</span></div>
          <div class="qoshimcha">Namlik: <span>{{ weather.main.humidity }}%</span></div>
          <div class="qoshimcha">Bulutlik: <span>{{ weather.clouds.all }}%</span></div>
          <div class="qoshimcha">Shamol: <span>{{ weather.wind.speed }}m/s</span></div>
          <div class="qoshimcha">Ko'rish: <span>{{ weather.visibility }}m</span></div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
window.addEventListener("load", () => {
  const loader = document.querySelector(".loader");
  setTimeout(() => {
    loader.style.opacity = "0";
    setTimeout(() => {
      loader.style.display = "none";
    }, 300);
  }, 500);
});





export default {
  name: "app",
  data() {
    return {
      api_key: "145fabc56d8e21cc2bba523ed1d3549c",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    showWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
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

      return `${date}-${month} ${year}, ${day}`;
    },
  },
};
</script>


<style>
.loader {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #222229;
  transition: all .5s ease;
  z-index: 1000
}
.planet {
  display: block;
  width: 125px;
  height: 125px;
  position: relative;
  transform-style: preserve-3d;
  border-radius: 50%;
  background: #fcc96b;
  background: rgb(252, 201, 107);
  background: linear-gradient(
    180deg,
    rgba(252, 201, 107, 1) 0%,
    rgba(252, 201, 107, 1) 15%,
    rgba(247, 174, 1, 1) 15%,
    rgba(247, 174, 1, 1) 19%,
    rgba(252, 201, 107, 1) 19%,
    rgba(252, 201, 107, 1) 22%,
    rgba(247, 174, 1, 1) 22%,
    rgba(247, 174, 1, 1) 28%,
    rgba(252, 201, 107, 1) 28%,
    rgba(252, 201, 107, 1) 31%,
    rgba(252, 201, 107, 1) 33%,
    rgba(252, 201, 107, 1) 36%,
    rgba(247, 174, 1, 1) 36%,
    rgba(247, 174, 1, 1) 48%,
    rgba(252, 201, 107, 1) 48%,
    rgba(252, 201, 107, 1) 55%,
    rgba(247, 174, 1, 1) 55%,
    rgba(247, 174, 1, 1) 66%,
    rgba(252, 201, 107, 1) 66%,
    rgba(252, 201, 107, 1) 70%,
    rgba(247, 174, 1, 1) 70%,
    rgba(247, 174, 1, 1) 73%,
    rgba(252, 201, 107, 1) 73%,
    rgba(252, 201, 107, 1) 82%,
    rgba(247, 174, 1, 1) 82%,
    rgba(247, 174, 1, 1) 86%,
    rgba(252, 201, 107, 1) 86%
  );
  box-shadow: inset 0 0 25px rgba(0, 0, 0, 0.25),
    inset 8px -4px 6px rgba(199, 128, 0, 0.5),
    inset -8px 4px 8px rgba(255, 235, 199, 0.5),
    inset 20px -5px 12px #f7ae01,
    0 0 100px rgba(255, 255, 255, 0.35);
  transform: rotateZ(-15deg);
}

.planet::before {
  position: absolute;
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 16px solid #7b6f42;
  border-top-width: 0;
  border-radius: 50%;
  box-shadow: 0 -2px 0 #b1a693;
  animation: rings1 0.8s infinite linear;
}

.planet::after {
  position: absolute;
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 8px solid #b1a693;
  border-top-width: 0;
  border-radius: 50%;
  box-shadow: 0 -2px 0 #7b6f42;
  animation: rings2 0.8s infinite linear;
}

@keyframes rings1 {
  0% {
    transform: rotateX(65deg) rotateZ(0deg) scale(1.75);
  }
  100% {
    transform: rotateX(65deg) rotateZ(360deg) scale(1.75);
  }
}

@keyframes rings2 {
  0% {
    transform: rotateX(65deg) rotateZ(0deg) scale(1.7);
  }
  100% {
    transform: rotateX(65deg) rotateZ(360deg) scale(1.7);
  }
}



* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
    font-family: cursive;
}
#app {
  background-image: url(./assets/cold.jpg);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: 0.4s;
}
#app.warm {
  background-image: url(./assets/warm.jpg);
}
main {
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 1)
  );
}
.search-box {
  width: 90%;
}
.search-box .search-bar {
  font-family: cursive;
  margin-top: 30vh;
  display: block;
  width: 100%;
  padding: 15px;
  color: #3d3434;
  font-size: 20px;
  border: none;
  background: none;
  outline: none;
  appearance: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  margin-top: 10vh;
  color: #fff;
  font-size: 38px;
  font-weight: 500;
  text-align: left;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .weather {
  color: #fff;
  font-size: 24px;
  font-weight: 300;
  font-style: italic;
  text-align: left;
}
.weather-box {
  text-align: left;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 38px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
}
.weather-box .qoshimcha{
  color: #eee;
  font-size: 18px;
  font-weight: 300;
}
.weather-box .qoshimcha span{
  font-size: 20px;
  font-weight: 700;
}

@media (max-width: 700px) {
  #app {
    background-image: url(./assets/coldPhone.jpg);
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    transition: 0.4s;
  }
  #app.warm {
    background-image: url(./assets/hotPhone.jpg);
  }
  main {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0),
      rgba(0, 0, 0, 1)
    );
  }
  .search-box {
    width: 100%;
  }
  .search-box .search-bar {
    margin-top: 3vh;
    display: block;
    width: 100%;
    padding: 15px;
    color: #3d3434;
    font-size: 20px;
    border: none;
    background: none;
    outline: none;
    appearance: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }
  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
  }
  .location-box .location {
    margin-top: 0vh;
    color: #fff;
    font-size: 33px;
    font-weight: 500;
    text-align: left;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .location-box .weather {
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: left;
  }
  .weather-box {
    text-align: left;
  }
  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 80px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(255, 255, 255, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 20px 0;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .weather-box .weather {
    color: #fff;
    font-size: 38px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  }
  .weather-box .qoshimcha{
    color: #eee;
    font-size: 18px;
    font-weight: 300;
  }
  .weather-box .qoshimcha span{
    font-size: 20px;
    font-weight: 700;
  }
}
</style>
