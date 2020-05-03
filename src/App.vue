<template>
  <div id="app">
    <main>
      <div class="search-city">
        <input type="text" placeholder="Search for a city" class="new-search" v-model="query" @keypress="fetchWeather"/>
      </div>

        <div class="wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="cityName">
        <div class="city">{{weather.name}}</div>
        <div class="date">{{dateBuilder()}}</div>
        </div>

    <div class="current">
        <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
        <div class="description">{{weather.weather[0].main}}</div>
        <div class="icon"><img id="weatherIcon" v-bind:src="iconurl" alt="Weather Icon"></div>
        <div class="min">Min-temperature : {{(Math.round(weather.main.temp_min))}}°C</div>
        <div class="max">Max-temperature : {{(Math.round(weather.main.temp_max))}}°C</div>
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
     api_key:'de1b251cd40c91a504993e70b2b107cf',
     url: 'https://api.openweathermap.org/data/2.5/',
     query:'',
     weather:{},
     wicon: '',
     iconurl: ''
   }
 },
 methods:{
   fetchWeather(event){
     if(event.key === "Enter"){
       fetch(`${this.url}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
       .then(result => {
         return result.json();
       }).then(this.setResults);
     }
   },
   setResults(data){
     this.weather = data;
     this.wicon = this.weather.weather[0].icon;
     this.iconurl = "http://openweathermap.org/img/w/" + this.wicon + ".png";

   },
   dateBuilder(){
     let d = new Date();
     let months = ['January', 'february', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
     let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
     let day = days[d.getDay()];
     let date = d.getDate();
     let month = months[d.getMonth()];
     let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
   },

  
        // document.getElementById('weatherIcon').setAttribute('src', iconurl);
   
        
 }

}
</script>

<style>
*{
    padding: 0px;
    margin: 0px;
    box-sizing: border-box;
}

#app{
  background: linear-gradient(to left top, #009688, #9C27B0)fixed;
}
main{
  min-height: 100vh;
  padding:25px;
  color:white;
}
.search-city{
    display:flex;
    justify-content: center;
    align-items: center;
    padding: 50px 15px;
    margin-top: 50px;
}
.search-city input{
    padding: 15px;
    width: 100%;
    max-width: 500px;
    font-size:large;
    border:none;
    outline: none;
    display: block;
    margin: auto;
}

.wrapper{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    text-align: center;
    margin: 30px 0px;
}
.cityName{
    margin-top: 50px;
}
.cityName .city{
    font-size:30px;
    margin-bottom: 20px;
    font-weight:600;
}
.cityName .date{
    font-size:20px;
    margin-bottom: 20px;
}
.current .temperature{
    font-size:80px;
    font-weight: 900;
    margin: 50px 0px;
    border: 1px solid white;
    padding: 30px;
    border-radius: 10%;
}
.current .description{
    font-size:25px;
    font-weight:500;
    margin: 30px 0px;
    font-style:italic;
}
.current .min{
    font-size:25px;
    font-weight: 500;
    margin: 30px 0px;
}
.current .max{
    font-size:25px;
    font-weight: 500;
    margin: 30px 0px;
}
#weatherIcon{
  height:80px;
  width:80px;
}

</style>
