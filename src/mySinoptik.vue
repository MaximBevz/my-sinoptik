<template>
  <div class="main">
    <div class="main-info">
      <h1 class="main-info__temperature">{{weather.temperature}}<span>&deg;</span></h1>
      <p class="main-info__city">{{weather.cityName}}</p>
    </div>
    <input type="text" :placeholder="place" class="main-search" v-model="citySearch">
    <div class="main-descr">
      <h2 class="main-descr__title">{{weather.description}}</h2>
      <div class="main-descr__info">
        <div class="main-descr__temp">Min.temp: <span>{{weather.lowTemp}}&deg;</span></div>
        <div class="main-descr__temp">Max. temp: <span>{{weather.highTemp}}&deg;</span></div>
        <div class="main-descr__temp">Feels like: <span>{{weather.feelsLike}}&deg;</span></div>
        <div class="main-descr__temp">Humiditly: <span>{{weather.humiditly}}%</span></div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'mySinoptik',
  data() {
    return {
      place: 'Search city . . .',
      citySearch: '',
      weather: {
        cityName: 'Ovruch',
        temperature: '22',
        description: 'Clouds',
        lowTemp: '18',
        highTemp: '22',
        feelsLike: '20',
        humiditly: '45'
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
  .main {
    background: url('./assets/img/clouds.jpg')center center/cover no-repeat;
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }
  .main::before {
    content: '';
    position: abosolute;
    display: block;
    width: 120px;
    height: 120px;
    top: 0px;
    left: 0px;
    background: url(./assets/img/clear-day.svg)center center/cover no-repeat;
  }
  .main-info {
    text-align: center;
  }
  .main-info__temperature {
    font-size: 64px;
    color: rgba(250, 241, 241, 0.918);
    text-shadow: 2px 0px 5px rgb(0 0 0 / 50%);
    position: relative;
  }
  .main-info__temperature span{
    font-size: 40px;
    position: absolute;
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
    width: 80%;
    min-width: 280px;
    padding: 10px;
    border-radius: 10px;
    border: none;
    display: block;
    background-color: rgb(232 243 247 / 60%);
    color: rgba(250, 241, 241, 0.9);
    box-shadow: 0px 0px 10px rgb(0 0 0 / 10%);
    transition: 0.3s all;
    margin: 40px auto;
  }
  .main-search::placeholder {
    color: rgba(0, 0, 0, 0.4);
    font-weight: 900;
  }
  .main-search:focus {
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
