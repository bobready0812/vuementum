<template>
<div class="clock">
 <h1> {{hours}} : {{minutes}} : {{seconds}} </h1>
</div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
     hours:"00",
     minutes:"00",
     seconds:"00",
    }
  },
  methods:{
    getClock(){
      const date = new Date();
      this.hours = String(date.getHours()).padStart(2,"0");
      this.minutes = String(date.getMinutes()).padStart(2,"0");
      this.seconds = String(date.getSeconds()).padStart(2,"0");
      
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
