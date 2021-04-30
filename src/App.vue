
<template>
  <div id="app" v-bind:class="typeof weather.main != 'undefined' ? weather.weather[0].main : temp">
      <main>
         <input type="text" class="InputField" v-model="location" @keyup.enter="getData" placeholder="enter the city..."/>
         <transition name="slide" mode="out-in">
            <div v-if="typeof weather.main != 'undefined'" class="content" v-bind:key="weather">
              <p class="temp"> {{weather.main.temp.toFixed(1)}}&#176;C</p>
              <div class="desc">
                  <p class="sky">{{weather.weather[0].description}}</p>
              </div>
              <p class="location"> {{weather.name}}, {{weather.sys.country}}</p>
              <p class="date">{{getDate()}}</p>
            </div>
         </transition>
      </main>
       <span class="footer">
              <p>Made by Prabhav Negi</p>
              <a href="https://github.com/prabhavnegi/vuejs-weather-app">Github Repo</a>
        </span>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

interface State {
  weather: Record<string, unknown>
  location: string
  date: string
  temp: ''
}

export default defineComponent({
  name: 'App',
  data: () : State => {
    return {
      weather: {},
      location: '',
      date: '',
      temp: ''
    }
  },
  methods: {
    getData (): void {
      fetch(`https://community-open-weather-map.p.rapidapi.com/find?q=${this.location}&cnt=1&mode=null&lon=0&type=link%2C%20accurate&lat=0&units=metric`, {
        method: 'GET', headers: { 'x-rapidapi-key': '9d6246ee99msh91c7f8b0a96db33p12ea70jsnfb67dfad8ffa', 'x-rapidapi-host': 'community-open-weather-map.p.rapidapi.com' }
      })
        .then(response => response.json())
        .then(dat => { this.weather = dat.list[0]; this.temp = dat.list[0].weather[0].main })
        .catch(err => { console.error(err) })
    },
    getDate () : string {
      let options: Intl.DateTimeFormatOptions
      const d = new Date(Date.now())
      options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }
      return d.toLocaleDateString(undefined, options)
    }
  }
})
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: montserrat;
}

input, textarea, select { font-family: inherit; }

 #app {
   background-image: url('./assets/main.jpg');
   background-size: cover;
   background-position: bottom;
   transition: 1s;
   position: relative;
 }

 #app.Clouds {
   background-image: url('./assets/Cloud.jpg');
 }
 #app.Clear {
   background-image: url('./assets/Clear.jpg');
 }
#app.Snow {
   background-image: url('./assets/Snow.jpg');
 }
#app.Thunderstorm {
   background-image: url('./assets/Thunderstorm.jpg');
 }
 #app.Rain {
   background-image: url('./assets/Rain.jpg');
 }
#app.Drizzle {
   background-image: url('./assets/Drizzle.jpg');
 }
#app.Haze, #app.Mist, #app.Fog, #app.Dust, #app.Smoke {
   background-image: url('./assets/Mist.jpg');
 }
#app.Tornado, #app.Squall {
  background-image: url('./assets/Storm.jpg');
}
#app.Sand{
   background-image: url('./assets/Sand.jpg');
 }

main {
    min-height: 100vh;
   display: flex;
   justify-content: flex-start;
   flex-direction: column;
   align-items: center;
   background-color: rgba(20, 20, 20, 0.6);
   padding: 50px;

 }

p {
  color: rgb(231, 231, 231);
}

.InputField {
  margin: 3% 0 6%;
  width: 80%;
  font-size: 2em;
  border-radius: 20px 0;
  padding: 20px;
  display: block;
  transition: 0.5s;
  background-color: rgba(0, 0, 0, 0.7);
  outline: none;
  color: white;
}

.InputField:focus {
  border-radius: 0 20px;
  background-color: rgb(222, 222, 222);
  color: black;
}

.desc {
  text-align: right;
  padding: 10px 0 0;
}
.content {
  text-align: center;
}

.temp {
  font-size: 15vw;
  font-weight: 600;
  line-height: 80%;
  text-shadow: 5px 3px 5px rgba(0, 0, 0, 0.6);
}

.location {
  font-size: 5.5vw;
  margin:2% 0 0 ;
  font-style: italic;
  font-weight: 600;
  text-shadow: 5px 3px 5px rgba(0, 0, 0, 0.6);
}

.date {
  font-size: 2.5vw;
  margin-top: 3%;
  text-shadow: 5px 3px 5px rgba(0, 0, 0, 0.6);
}

.sky {
  font-style: italic ;
  font-size: 2vw;
  text-shadow: 5px 3px 5px rgba(0, 0, 0, 0.6);
}

.footer {
  position: absolute;
  bottom: 10px;
  right: 10px;
  display: flex;
  flex-direction: row;
  padding: 10px;
}

.footer p, .footer a{
  margin: 0 10px;
  font-style: italic;
  text-decoration: none;
  color: rgb(243, 243, 243);
}

.footer a:hover {
  color:rgb(211, 58, 31)
}

.slide-enter-active {
  transition: all .5s ease-in;
}
.slide-leave-active {
  transition: all .5s ease-out;
}
.slide-enter, .slide-leave-to {
  transform: scale(10px);
  opacity: 0;
}
</style>
