<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''" >
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Buscar..." v-model="query" @keypress="fetchWeather"/>
      </div>      
      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">{{weather.name}},{{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}° C</div>
          <div class="weather">{{capitalize(weather.weather[0].description)}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'app',
  data() {
    return {
      api_key:'08ba90eb2b3b0cab3a504219756e1589',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
    async fetchWeather(e){
      if(e.keyCode===13){
        const {data}=await axios.get(`${this.url_base}weather?q=${this.query}&units=metric&lang=es&appid=${this.api_key}`)
        console.log(data);
        this.setResults(data);
      }
    },
    setResults(data){
      this.weather=data;      
    },
    dateBuilder(){
      const date=new Date();
      moment.locale('es');
      const day=this.capitalize(moment(date).format('dddd'));
      const month=this.capitalize(moment(date).format('MMMM'));
      const year=moment(date).format('YYYY');
      const numDay=moment(date).format('DD');

      return (`${day}, ${numDay} de ${month} de ${year}`);
    },

    capitalize (s){
      if (typeof s !== 'string') return ''
      return s.charAt(0).toUpperCase() + s.slice(1)
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
body{
  font-family: 'Roboto', sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom ;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color :#313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date{
  color: #fff;
  font-size: 18px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
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

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color :#fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
