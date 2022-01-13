<template>
<div class="clock">
 <h1> {{ampm2}} {{hours}} : {{minutes}}</h1>
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
 <input v-model="value" class="todo" required type="text" placeholder="Write To Do"/>
</form>
<ul>
  <li v-for="todo in todos" :key="todo.id" :id="todo.id" >
    <span> {{todo.text}} </span> 
    <button @click="deleteTodo">X</button> 
  </li>
</ul>
<div>
  <span> {{weather}} </span>
  <span> {{city}} </span>
</div>
<img :src="require(`./images/${img}.jpg`)">





</template>

<script>
import quoteData from "./data/quoteData"

const API_KEY ="15f88eaf631746bd32fc713337bd5c9d";

export default {
  name: 'App',
  data() {
    return {
     hours:"00",
     minutes:"00",
     id:"",
     isHidden:true,
     isHidden2:false,
     quote:"",
     author:"",
     savedUsername:"",
     todos: [],
     value:"",
     ampm2:"",
     todaysDate:"",
     weather:"",
     city: "",
     img: "nothing",
    }
  },
 
  methods:{
    getClock(){
      const date = new Date();
      const ampm = date.getHours();
      if(ampm > 13) {
        this.ampm2 = "P.M."
        this.hours = String(date.getHours() - 12).padStart(2,"0")
        this.minutes = String(date.getMinutes()).padStart(2, "0");
      } else if (ampm == 12) {
        this.ampm2 = "P.M.";
        this.hours = String(date.getHours()).padStart(2,"0")
        this.minutes = String(date.getMinutes()).padStart(2, "0");
      } else {
        this.ampm2 = "A.M.";
        this.hours = String(date.getHours()).padStart(2,"0")
        this.minutes = String(date.getMinutes()).padStart(2, "0");
      }
    } ,
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
     handleToDoSubmit(e){
       e.preventDefault();
       const todo = {
         text:this.value,
         id:Date.now(),
       }
      this.todos.push(todo);
      this.value = ""; 
      this.saveTodos();
     },
     deleteTodo(e) {
       const li = e.target.parentElement;
       
        this.todos= this.todos.filter((toDo) => toDo.id !== parseInt(li.id));
       li.remove();
       console.log(e);
       this.saveTodos();
     },
     saveTodos() {
       localStorage.setItem('todo', JSON.stringify(this.todos));
     },
     paintTodo() {
       const saveToDos = localStorage.getItem("todo");
       if(saveToDos) {
         const parsedTodos = JSON.parse(saveToDos);
         this.todos = parsedTodos;
       }
     },
    onGeoOk(position){
    const lat = position.coords.latitude;
    const log = position.coords.longitude;
     
    const url = `http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${log}&appid=${API_KEY}`;
    fetch(url)
    .then((response) => response.json())
    .then((data) => {
       
       this.weather = data.weather[0].main;
       this.city = data.name;
       this.img = data.weather[0].main;
      
    }); 
    }, 
   
    
    onGeoError() {
      alert("No location found!");
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
     this.paintTodo();
     navigator.geolocation.getCurrentPosition(this.onGeoOk, this.onGeoError);
     
  },
  
  components: {
   
  }
}
</script>

<style>

.hide {
  display: none;
}
.tododiv {
  display: flex;
  justify-content: center;
}
.font {
  font-size: 24px;
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
