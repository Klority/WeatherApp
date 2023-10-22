<template>
 <div id="app" :class="typeof weather.main!='undefined'&& tempTranslator(weather.main.temp)>15?
  'warm':''">
  <main>
    <div class="search-box">
      <input type="text" 
      class="search-bar"
       placeholder="Search..."
       v-model="query"
       @keypress="fetchWeather"
       />
    </div>

    <div class="weather-wrap" v-if="typeof weather.main!='undefined'">
      <div class="location-box">
        <div class="location">{{ weather.name }},{{ weather.sys.country }}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{tempTranslator(weather.main.temp) }}°c</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>

  </main>
 </div>
</template>

<script>
export default{
  name: 'app',
  data(){
    return{
      api_key:'KEY',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key=="Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&limit=5&appid=${this.api_key}`)
        .then(res=>{
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results;
    },
    tempTranslator(temperature){
      return (Math.round(temperature- 273,15));
    },
    dateBuilder(){
      let d=new Date();
      let months=["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"];
      let days=["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];

      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();

      return `${day} ${date} ${month} ${year}`

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
  font-family: Open sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: bottom;
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  outline: none;
  border:none;
  border-radius: 15px 0 15px 0;
  background-color: rgba(255,255,255,0.5);
  box-shadow: none;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  border-radius: 15px 15px 15px 15px;
  background-color: rgba(255,255,255,0.75);
  box-shadow: 0px 0px 16px 0px;
}

.location-box, .weather-box{
  color: white;
font-size: 32px;
font-weight: 500;
text-align: center;
}

.location-box .location{
  text-shadow: 1px 2px green;  
  margin-bottom: 30px;
}

.date, .temp{
  font-size: 28px;
  opacity: 0.8;
  text-shadow: 1px 2px black;  
  
}

.weather-box .temp{
  display: inline-block;
  align-items: center;
  text-shadow: 3px 3px rgba(0,0,0,0.25);
  font-size: 80px;
  font-weight: 800;
  color: black;
  background:none;
  background-color: rgba(255,255,255,0.25);
  border-radius: 30px;
  margin: 25px;
  padding: 30px;
  box-shadow: 3px 3px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: white;
  font-size: 46px;
  font-weight: 600;
}


</style>
