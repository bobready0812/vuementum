<template>
<!-- 토글스위치 -->
 <div class="switch-component-wrapper">
  <div class="class switch-wrapper" @click="changeSwitchValue" :class="{'on' : switchValue, 'off': !switchValue }">
     <div class="circle">
     </div>
  </div>
 </div>
<!-- 시계 -->
<div class="clock">
 <h1> {{ampm2}} {{hours}} : {{minutes}}</h1>

</div>
<!-- 로그인폼 -->
<form @submit="onLoginSubmit" v-bind:class="{ hide : isHidden2}">
 <input 
 required
 maxlength="15"
 type="text"
 placeholder="What is your name?"
 v-model="id"
 />
</form>
<!-- 로그인 후 h1 -->
<h1 id="greeting" v-bind:class="{ hide : isHidden}"> Hi! {{id}} </h1>
<!-- 명언 -->
<h1> {{quote}} </h1>
<h1> {{author}} </h1>
<!-- todo 폼 -->
<form @submit="handleToDoSubmit">
 <input v-model="value" class="todo" required type="text" placeholder="Write To Do"/>
</form>
<!-- todo -->
<ul>
  <li v-for="todo in todos" :key="todo.id" :id="todo.id" >
    <span> {{todo.text}} </span> 
    <button @click="deleteTodo">X</button> 
  </li>
</ul>
<!-- 날씨 -->
<div>
  <span> {{weather}} </span> <br/>
  <span> {{city}} </span>
</div>
<!-- 날씨 배경 -->
<img :src="require(`./images/${img}.jpg`)">
<h1 @click="changeNoteShow1" v-bind:class="{ hide : isHidden3}">노트</h1>
<div v-bind:class="{ hide : isHidden4}" class="note">
  <h6 @click="changeNoteShow2" class="shutdown">닫기</h6>
  <h4 v-bind:class="{ hide : isHidden5}"> 저장됨 </h4>
  <textarea v-model="texta" class="noteinput">
</textarea>
</div>

 
  
</template>

<script>
import quoteData from "./data/quoteData";



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
     isHidden3:false,
     isHidden4:true,
     isHidden5:true,
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
     switchValue: false,
     texta:"",
    }
  },
 
  methods:{
    //시계
    getClock(){
      const date = new Date();
      const ampm = date.getHours();
      if(this.switchValue === false) {
      if(ampm >= 13) {
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
      } } else {
         this.ampm2 = "P.M."
         this.hours = String(date.getHours()).padStart(2,"0")
         this.minutes = String(date.getMinutes()).padStart(2, "0");

      } 
    } ,
    //로그인
    onLoginSubmit(a) {
    a.preventDefault();
    this.isHidden = false;
    this.isHidden2 = true;
    localStorage.setItem("username", this.id);
    },
    //스토레지에 저장된 유저이름을 불러오는 함수
    getSavedUserName() {
      this.savedUsername = localStorage.getItem("username");
    },
    //랜덤 명언 설정
    getQuote() {
      const todaysQuote = quoteData[Math.floor(Math.random() * quoteData.length)];
      this.quote = todaysQuote.quote;
      this.author = todaysQuote.Author;
    },
    //투두 추가
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
     //투두 삭제
     deleteTodo(e) {
       const li = e.target.parentElement;
       
        this.todos= this.todos.filter((toDo) => toDo.id !== parseInt(li.id));
       li.remove();
       console.log(e);
       this.saveTodos();
     },
     //투두 저장
     saveTodos() {
       localStorage.setItem('todo', JSON.stringify(this.todos));
     },
     //스토레지에 저장된 투두들 가져와서 보여주는 함수
     paintTodo() {
       const saveToDos = localStorage.getItem("todo");
       if(saveToDos) {
         const parsedTodos = JSON.parse(saveToDos);
         this.todos = parsedTodos;
       }
     },
     //토글 스위치 on off 함수
     changeSwitchValue() {
     this.switchValue = !this.switchValue;

     },

     //날씨를 받아와 보여주는 함수
    onGeoOk(position){
    const lat = position.coords.latitude;
    const lon = position.coords.longitude;
     
    let url = `https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lon}&exclude=current,minutely,hourly,alerts&appid=${API_KEY}&units=metric`;
  
    fetch(url)
    .then((response) => response.json())
    .then((data) => {
       
       this.weather = data.daily;
       console.log(url);
      
    }); 
    }, 
   
    //날씨가 호출되지 않았을때 실행되는 함수
    onGeoError() {
      alert("No location found!");
    },
    changeNoteShow1() {
       this.isHidden3 = true;
       this.isHidden4 = false;
    },
    changeNoteShow2() {
      this.isHidden3 = false;
      this.isHidden4 = true;
      localStorage.setItem('note', this.texta);
    
    },
    getNote() {
      this.texta = localStorage.getItem('note');
    },
  

  
    
  },
  mounted () {
    //0.1초 간격으로 시계가 업데이트 됨
     setInterval(this.getClock,100);
    //스토레지에 저장된 유저이름 불러오는 함수 호출 
     this.getSavedUserName(); 
     //저장된 유저이름이 없다면 로그인 폼을 보여줌
     if( this.savedUsername === null ) {
       this.isHidden2 = false;
       this.isHidden = true;
       this.id = "";
       //저장된 유저이름이 있다면 자동 로그인 해줌
     } else {
       this.isHidden = false;
       this.isHidden2 = true;
       this.id = this.savedUsername;
     }
     //랜덤 명언 함수를 호출
     this.getQuote();
     //스토레지에 저장된 투두 불러오는 함수 호출
     this.paintTodo();
     //자신의 위치를 받아와서 API를 호출하는 함수
     navigator.geolocation.getCurrentPosition(this.onGeoOk, this.onGeoError);
     this.getNote();
    
     
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
.switch-component-wrapper {
  display: flex;
}
.switch-wrapper {
   width: 44px;
   min-height: 22px;
   display: flex;
   cursor: pointer;
   border-radius: 22px;
   align-items: center;
  padding: 2px;
  transition: all 0.5s;
  background: green;
}

.on {
  background: green;
  justify-content: flex-end;
}

.off {
  background: gray;
  justify-content: flex-start;
}

.circle {
  background: #fff;
  width: 18px;
  height: 18px;
  border-radius: 18px;
}
.note {
  width: 300px;
  height: 300px;
  border:1px solid black;
  margin:0;
  border-radius: 14px;
}
.noteinput {
  width: 100%;
  border:none;
  padding: 0;
  height: 90%;
  overflow-y:scroll;
}
.noteinput::-webkit-scrollbar {
  width:10px;
  
 
}
.noteinput::-webkit-scrollbar-thumb {
  width:10px;
  background-color: skyblue;
  border-radius: 10px;
 
}
.noteinput::-webkit-scrollbar-track {
  width:10px;
  background-color: blanchedalmond;
  border-radius: 10px;
  box-shadow: inset 0px 0px 5px white;
 
}
.noteinput:focus {
  outline: none; 
  
}
.shutdown {
  margin:0;
}
</style>
