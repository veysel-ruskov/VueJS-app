<template>
  <div>
    <div class="cards">
    <p class="cardName">{{element}}</p>
    </div>
  </div>
</template>

<script>

export default {
  name: "App",
  data() {
    return{
      element : []
    }
  },
  components: {
   // HelloWorld,
  },
  async created() {
    var response = await fetch('https://greet.bg/wp-json/wc/store/products?page=1')
    var data = await response.json();
    //console.log(data[0].images[0].src)
    for (let i = 0; i < data.length; i++) {
       this.element = data[i]
      console.log(this.element)
    }
  }
};
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
