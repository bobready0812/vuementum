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
<h1 id="greeting" v-bind:class="{ hide : isHidden}"> Hi! {{id}} </h1>
<h1> {{quote}} </h1>
<h1> {{author}} </h1>
<form @submit="handleToDoSubmit">
 <input v-model="todo" class="to_do" required type="text" placeholder="Write To Do"/>
</form>
<ul>

</ul>
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
     savedUsername:"",
     todo:""
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
    localStorage.setItem("username", this.id);
    },
    getSavedUserName() {
      this.savedUsername = localStorage.getItem("username");
    },

    getQuote() {
      const todaysQuote = quoteData[Math.floor(Math.random() * quoteData.length)];
      this.quote = todaysQuote.quote;
      this.author = todaysQuote.Author;
    },
    handleToDoSubmit(event) {
     event.preventDefault(); 
     const newTodo = this.todo;
     this.todo = "";
     this.paintToDo(newTodo);
    },
    paintToDo(newTodo) {
     
    }
    
  },
  mounted () {
     setInterval(this.getClock,1000);
     this.getSavedUserName(); 
     if( this.savedUsername === null ) {
       this.isHidden2 = false;
       this.isHidden = true;
       this.id = "";
     } else {
       this.isHidden = false;
       this.isHidden2 = true;
       this.id = this.savedUsername;
     }
     this.getQuote();
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
