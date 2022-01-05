<template>
<div class="menu">
  <a v-for="작명 in menus" :key="작명">{{작명}}</a>
</div>

  <div>
    원룸샵
    <h4> XX 원룸 </h4>
    <p> {{ price1 }} 만원 </p>
  </div>
   <div>
    <h4> XX 원룸 </h4>
    <p> {{ price2 }} 만원 </p>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      price1 : 60,
      price2 : 70,
      menus : ['Home', 'Shop', 'About'],
      products  : ['역삼동 원룸', '천호동 원룸', '마포구 원룸']
    }
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
