<template>
<div class="clock">
 <h1> {{hours}} : {{minutes}} : {{seconds}} </h1>
</div>
<form @submit="onLoginSubmit" v-bind:class="{ hide : isHidden2}">
 <input 
 required
 maxlength="15"
 type="text"
 placeholder="What is your name?"
 v-model="id"
 />
</form>
<h1 id="greeting" v-bind:class="{ hide : isHidden}"> {{id}} </h1>
<h1> {{quote}} </h1>
<h1> {{author}} </h1>
</template>

<script>
import quoteData from "./data/quoteData"


export default {
  name: 'App',
  data() {
    return {
     hours:"00",
     minutes:"00",
     seconds:"00",
     id:"",
     isHidden:true,
     isHidden2:false,
     quote:"",
     author:"",
    }
  },
  methods:{
    getClock(){
      const date = new Date();
      this.hours = String(date.getHours()).padStart(2,"0");
      this.minutes = String(date.getMinutes()).padStart(2,"0");
      this.seconds = String(date.getSeconds()).padStart(2,"0");
      
    },
    onLoginSubmit(a) {
    a.preventDefault();
    this.isHidden = false;
    this.isHidden2 = true;
    },
    getQuote() {
      
    }
   

  },
  mounted () {
     setInterval(this.getClock,1000);
  },
  
  components: {
   
  }
}
</script>

<style>

.hide {
  display: none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
