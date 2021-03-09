<template>
  <div id="main" :class="[isDay ? 'day' : 'night', weatherName]">
    <div class="main-info">
      <div class="main-info__temperature">
        <h1 class="main-info__title">{{weather.temperature}}<span>&deg;</span></h1>
        <img src="./assets/img/thermometer.svg" alt="termometer">
      </div>
      <p class="main-info__city">{{weather.cityName}}</p>
    </div>
    <label class="main-search">
      <input type="text" :placeholder="place" class="main-search__input" v-model="citySearch" @keydown.enter="getWeather">
      <button class="main-search__btn" @click="getWeather">Найти</button>
    </label>
    <div class="main-descr">
      <h2 class="main-descr__title">{{weather.description}}</h2>
      <div class="main-descr__info">
        <div class="main-descr__temp">Мин. темп: <span>{{weather.lowTemp}}&deg;</span></div>
        <div class="main-descr__temp">Макс. темп: <span>{{weather.highTemp}}&deg;</span></div>
        <div class="main-descr__temp">Чувствуется: <span>{{weather.feelsLike}}&deg;</span></div>
        <div class="main-descr__temp">Влажность: <span>{{weather.humidity}}%</span></div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'mySinoptik',
  isDay: false,
  data() {
    return {
      weatherName: {
        clear: false,
        cloudly: false,
        rain: false,
        snow: false,
      },
      place: 'Найти город . . .',
      citySearch: '',
      weather: {
        cityName: 'Овруч',
        temperature: '22',
        description: 'Облачно',
        lowTemp: '18',
        highTemp: '22',
        feelsLike: '20',
        humidity: '45'
      }
    }
  },
  methods: {
    getWeather: async function() {
      const key = 'fe57b721fd47b8600afac45a7829c1ea';
      const baseUrl = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&lang=ru&units=metric&appid=${key}`;

      const response = await fetch(baseUrl);
      const data = await response.json();

      this.weather.cityName = data.name;
      this.weather.temperature = Math.round(data.main.temp);
      this.weather.description = data.weather[0].description;
      this.weather.lowTemp = Math.round(data.main.temp_min);
      this.weather.highTemp = Math.round(data.main.temp_max);
      this.weather.feelsLike = Math.round(data.main.feels_like);
      this.weather.humidity = Math.round(data.main.humidity);
      this.citySearch = ''

      const timeOfDay = data.weather[0].icon;

      if(timeOfDay.includes('n')) {
        this.isDay = false
      } else {
        this.isDay = true
      }

      const nameWeather = data.weather[0].description;

      if(nameWeather.includes("пасмурно")) {
        this.remooveAllWeather()
        this.weatherName.cloudly = true
      } else if(nameWeather.includes("снег")) {
        this.remooveAllWeather()
        this.weatherName.snow = true
      } else if(nameWeather.includes("дождь")) {
        this.remooveAllWeather()
        this.weatherName.rain = true
      } else {
        this.remooveAllWeather()
        this.weatherName.clear = true
      }
      
    },
    remooveAllWeather() {
      for(let key in this.weatherName) {
        this.weatherName[key] = false
      }
    }
  }
}
</script>

<style>
@import url(./assets/style/style.css);
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: 'Fira';
  }
  #main {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }
  .day {
    background: url('./assets/img/clouds.jpg')center center/cover no-repeat;
  }
  .night {
    background: url('./assets/img/clouds-night.jpg')center center/cover no-repeat;
  }
  #main.night.clear::before {
    background: url(./assets/img/partly-cloudy-night.svg)center center/cover no-repeat;
  }
  #main.night.cloudly::before {
    background: url(./assets/img/cloudy.svg)center center/cover no-repeat;
  }
  #main.night.rain::before {
    background: url(./assets/img/rain.svg)center center/cover no-repeat;
  }
  #main.night.snow::before {
    background: url(./assets/img/snow.svg)center center/cover no-repeat;
  }
  #main.day.clear::before {
    background: url(./assets/img/clear-day.svg)center center/cover no-repeat;
  }
  #main.day.cloudly::before {
    background: url(./assets/img/cloudy.svg)center center/cover no-repeat;
  }
  #main.day.rain::before {
    background: url(./assets/img/rain.svg)center center/cover no-repeat;
  }
  #main.day.snow::before {
    background: url(./assets/img/snow.svg)center center/cover no-repeat;
  }

  #main::before {
    content: '';
    position: abosolute;
    display: block;
    width: 120px;
    height: 120px;
    top: 0px;
    left: 0px;
  }
  .main-info {
    text-align: center;
  }
  .main-info__temperature {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .main-info__title {
    font-size: 64px;
    color: rgba(250, 241, 241, 0.918);
    text-shadow: 2px 0px 5px rgb(0 0 0 / 50%);
  }
  .main-info__title span {
    font-size: 30px;
    position: absolute;
  }
  .main-info__temperature img{
    width: 80px;
  }
  .main-info__city {
    color: rgba(250, 241, 241, 0.918);
    text-transform: uppercase;
    margin-top: 5px;
    font-size: 24px;
    text-shadow: 2px 0px 5px rgb(0 0 0 / 40%);
    font-weight: 900;
  }
  .main-search {
    display: flex;
    justify-content: center;
    margin: 30px 0;
  }
  .main-search__input {
    width: 50%;
    min-width: 280px;
    padding: 20px 15px;
    border-radius: 10px;
    border: none;
    display: block;
    background-color: rgb(232 243 247 / 60%);
    color: rgba(0, 0, 0, 0.4);
    font-weight: 900;
    box-shadow: 0px 0px 10px rgb(0 0 0 / 10%);
    transition: 0.3s all;
    cursor: pointer;
  }
  .main-search__btn {
    border: none;
    background-color:crimson;
    color: #fff;
    min-width: 100px;
    padding: 20px;
    border-radius: 10px;
    margin-left: 20px;
    font-weight: 900;
    transition: 0.3s all;
    cursor: pointer;
  }
  .main-search__btn:hover {
    background-color:rgb(172, 20, 50);
  }
  .main-search__btn:focus {
    outline: none;
  }
  .main-search__input::placeholder {
    color: rgba(0, 0, 0, 0.4);
    font-weight: 900;
  }
  .main-search__input:focus {
    outline: none;
    box-shadow: 0px 0px 10px rgb(0 0 0 / 30%);
    color: rgba(0, 0, 0, 0.4);
    font-weight: 900;
  }
  .main-descr {
    width: 95%;
    height: 80vh;
    margin: 0 auto;
    background-color: rgb(242 251 254 / 60%);
    border-radius: 10px 10px 0 0;
    text-align: center;
    padding: 30px 40px;
  }
  .main-descr__info {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-top: 10px;
  }
  .main-descr__title {
    color: #20a0d2;
    text-transform: uppercase;
  }
  .main-descr__temp {
    color:#595b5e;
    margin-bottom: 20px;
    font-size: 16px;
    min-width: 100px;
  }
  .main-descr__temp span {
    display: block;
    margin-top: 10px;
    font-size: 22px;
    font-weight: 900;
    color:#125cdb;
  }
</style>
