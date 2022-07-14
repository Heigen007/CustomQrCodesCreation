<template>
  <div id="app">
    <h1>Create your own QR code</h1>
    <div class="flexBox">
      <h3 style="margin-right: 2rem">Choose your QR type:</h3>
      <select v-model="select">
        <option>Text</option>
        <option>URL</option>
        <option>Whatsapp link</option>
        <option>Phone number</option>
        <option>Email</option>
        <option>Geo</option>
      </select>
    </div>
    <div class="flexBox">
      <h3 style="margin-right: 2rem">Fill the {{typeOfFilledData}}</h3>
      <input type="text" v-model="input">
    </div>
    <div class="flexBox" style="margin-bottom: 1rem" v-if="select=='Geo'">
      <button @click="fillGEO">Fill current GEO</button>
    </div>
    <div>
      <button style="margin-bottom: 1rem; padding: 5px 15px" @click="createQr">Create QR</button>
    </div>
    
    <vue-qr :text="text"></vue-qr>
  </div>
</template>

<script>
import VueQr from 'vue-qr'
export default {
  components: {
    VueQr
  },
  data() {
    return {
      select: '',
      input: '',
      text: '',
    }
  },
  methods: {
    createQr(){
      if(!this.select){
        alert('Please choose a type')
        return
      }
      if(this.select == 'Text'){
        this.text = this.input
      }
      if(this.select == 'URL'){
        this.text = this.input
      }
      if(this.select == 'Whatsapp link'){
        this.text = "https://wa.me/" + this.input
      }
      if(this.select == "Phone number"){
        this.text = "tel:" + this.input
      }
      if(this.select == "Email"){
        this.text = "mailto:" + this.input
      }
      if(this.select == "Geo"){
        this.text = "geo:" + this.input
      }
    },
    fillGEO(){
      if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition(position => {
          this.input = position.coords.latitude + "," + position.coords.longitude
        })
      }
    }
  },
  computed: {
    typeOfFilledData(){
      switch(this.select){
        case 'Text':
          return 'text(someText)'
        case 'URL':
          return 'URL'
        case 'Whatsapp link':
        case "Phone number":
          return 'phone number(+77077077077)'
        case 'Email':
          return 'email(hi@mail.ru)'
        case 'Geo':
          return 'geo(44.140315,-73.737373)'
        default:
          return 'data'
      }
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body{
  background-color: #a2c7bb;
}

.flexBox{
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>