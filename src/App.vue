<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input  type="text" 
                class="search-bar" 
                placeholder="Search"
                v-model="query"
                @keypress="fetchWeather" />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' " >
        <div class="location-box">
          <div class="location"> {{ weather.name }}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }} </div>
        </div>
        <div class="weather-box">
          <div class="temp"> {{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key: '574e71942c895ddd7a22689c34db02e9',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {

      }
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let days = ["Mon", "Tuesd", "Wends", "Thurs", "Frid", "Sat", "Sund"]
      let months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Agst", "Sept", "Oct", "Nov", "Dec"]
      
      let date = d.getDate();
      let month = months[d.getMonth()];

      let day = days[d.getDay()];
      let year = d.getFullYear();

      return `${day}, ${date} - ${month} - ${year}`;
    }
  }
  
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  /* -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
  background-image: url('./assets/weather.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main{
  min-height: 100vh;
  padding: 25px;
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: azure;
  border-radius: 0px 16px 0px 16px;
}

.location-box .location{
  color: beige;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
}

.location-box .date{
  color: beige;
  font-size: 20px;
  font-weight: 500;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
}

.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;

}
  </style>
