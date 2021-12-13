<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm': ''">
    <main>
      <div class="main-weather">
        <h3 class="headline">Mini WeatherApp</h3>
        <div class="search-box">
          <input v-model="query" @keypress="fetchWeather" class="search-bar" type="text" placeholder="search a city"/>
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
            <div class="date">{{dateBuilder()}}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>
      <p class="footer">made by <a href="https://www.linkedin.com/in/salaralali" target="blank">Salar Al Ali with </a>❤️</p>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  mounted(){
    document.title = 'WeatherApp by Salar'
  },
  data(){
    return {
      api_key: '73b550f7c423232a16bba2e078e957e9',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if(e.key == 'Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => res.json())
          .then(this.setResult)
      }
    },
    setResult(results) {
      this.weather = results
    },
    dateBuilder(){
      let d = new Date()
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`; 
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;500;700;900&display=swap');
  
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font-family: 'Montserrat', sans-serif;
  }
  .headline {
    color: #FFF;
    font-family: 'Montserrat', sans-serif;
    font-size: 30px;
    margin: 20px;
    text-align: center;
  }
  main{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0, 0.25), rgba(0,0,0,0.75));

    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: .4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
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
    border: none;
    outline: none;
    appearance: none;
    background: none;

    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
    background-color: rgba(225,225,225,0.5);
    border-radius: 0px 16px 0 16px;
    transition: .4s;
  }
  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);;
    background-color: rgba(225,225,225,0.75);
    border-radius: 16px 0 16px 0;
  }
  .location-box .location {
    color: #FFF;
    font-size: 32px ;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }
  .location-box .date {
     color: #FFF;
    font-size: 20px ;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
  }
  .weather-box{
    text-align: center;
    transition: 1s ease;
    
  }
  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #FFF;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0,0,0, 0.25);
    background-color: rgba(225,225,225,0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }
  .weather-box .weather {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0,0,0, 0.25);
  }

  .footer {
    color: #FFF;
    text-align: center;
    font-size: 15px;
    margin-bottom: 30px;
  }
  .footer a {
    font-size: 20px;
  }
  .footer a:link, a:visited {
    text-decoration: none;
    color: inherit;
    cursor: pointer;
    transition: .4s;
  }
  .footer a:hover {
    color: rgb(228, 228, 228);
  }
</style>
