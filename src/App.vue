<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        :class="`search-bar ${state}`"
        class="search-bar start"
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{buildDate()}}</div>
        </div>
        <div class="weather-box">
           <div class="temp">{{ Math.round(weather.main.temp)}}˚c <div class="humidity">{{weather.main.humidity}}</div></div>
           <div class="weather">{{weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data (){
   return{
     api_key: '9a9c99f013616c0f56a64bdf56bd2d92',
     url_base: 'https://api.openweathermap.org/data/2.5/',
     query: '',
     weather: {},
     state: "animation1"
   }
  },
  methods: {
    onStopAnimation(){
      this.state = 'animation2'
      console.log('animation ended')
    },
     fetchWeather (e){
       if(e.key == "Enter"){
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
           .then(res => {
             return res.json();
           }).then(this.setResults);
       }
     },
     setResults (results){
       this.weather = results;
       console.log(results)
     },
     buildDate() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    endAnimation(){
      let animation = document.querySelector(".animation1");
      animation.addEventListener('animationend', () => {
        console.log('animation ended')
      })
    }
  },
}


</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montseratt', sans-serif;
}
#app{
  background-image: url(https://images3.alphacoders.com/158/thumb-1920-158955.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main{
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

  color: #313131;

  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  border-radius: 16px;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);

  background-color: rgba(255,255,255,0.5);
  transform: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-clip: rgba(255,255,255,0.75);
  border-radius: 16px;
}

.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box{
  text-align:center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow:3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
 color: #FFF;
 font-size: 48px;
 font-weight: 700;
 font-style: italic;
 text-shadow: 3px 6px rgba(0,0,0,0.25);
}
.humidity{
  font-size: 30px;
}
.parent{
  height: 500px;
  width: 500px;
  background-color: white;
}
.animation1{
  background-color: red;
  height: 20px;
  width: 20px;
  animation-name: animation1;
  animation-duration: 2s;
  animation-iteration-count: 5;
}
@keyframes animation1{
  100%{
    transform: translateX(30px);
  }
}
@keyframes animation2{
  100%{
    transform: translateY(30px);
  }
}

</style>

