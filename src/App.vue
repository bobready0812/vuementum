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
    <span> {{index}} </span>
    <button @click="deleteTodo">X</button> 
  </li>
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
     id:"",
     isHidden:true,
     isHidden2:false,
     quote:"",
     author:"",
     savedUsername:"",
     todos: [],
     value:"",
     ampm2:"",
    }
  },
 
  methods:{
    getClock(){
      const date = new Date();
      const ampm = date.getHours();
      if(ampm > 13) {
        
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
