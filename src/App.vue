<script>
import axios from "axios"
export default {
  data(){
    return{
    city:"",
    error:"",
    links:null
    }
  },
  computed:{
    showTemp(){
      return "Температура: " + Math.trunc(this.links.main.temp)
    },
    showFeels(){
      return "Ощущается как: " + Math.trunc(this.links.main.feels_like)
    },
    showWind(){
      return "Скорость ветра: " + Math.trunc(this.links.wind.speed) + "ms"
    },
    showTemp_max(){
      return "Максимальная температура: " + Math.trunc(this.links.main.temp_max) + "ms"
    }
  },
  methods:{
    getWeather(){
      if(this.city.trim().length < 3){
        this.error = "Введите больше символов"
        return this.error
      }else{
      this.error =""
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=324d8f896f3312a45015689c72c25bfc`)
      .then(r =>(this.links = r.data))}
    }
  }
}

</script>

<template>
  <div >
  <div class="weather">
<div>
  <h1>Погодное приложение</h1>
    <input type="text" placeholder="введите свой город" v-on:input="city = $event.target.value">
      <button class="btnYes" v-if="city != ''" @click="getWeather()">узнать погоду</button>
      <button class="btnNo" disabled v-else>введите город</button>
</div>
      <div v-if="this.city != ''">
        <p>{{ city != '' ? "погода в городе: " + city : "" }}</p>
        <p>{{ showTemp}}</p>
        <p>{{ showFeels }}</p>
        <p>{{ showWind }}</p>
        <p>{{ showTemp_max }}</p>
      </div>
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<style scoped>

.weather{
  width: 50vw;
  height: 60vh;
  background: blueviolet;
  border: 2px solid black;
  border-radius: 30px;
  box-shadow: 3px 5px black;
  padding: 0.3%;
  padding: 10px;
  .btnYes{
    cursor: pointer;
  }
  .btnNo{
    cursor: not-allowed;
  }

}
.weather button{
  border-radius: 5px;
  background: green;
  margin-left: 5px;
  font-size: 16px;
  padding: 3px;
}
.weather input{
  margin-top: 6px;
    border: 0;
    font-size: 16px;
    border-bottom: 2px solid black;
    outline: none;
  }
  .weather p{
    margin-top: 3px;
    font-size: 24px;
  }
  .weather input:focus{
    border-bottom-color:green ;
  }
  .weather .btnYes:hover{
    transform: scale(1.5) translateX(-2px);
  }

</style>
