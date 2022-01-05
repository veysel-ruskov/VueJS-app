<template>
  <div>
    <div class="cards">
      <div v-for="el in data" :key="el.id">
        <img class="imgCard" :src="el.images[0].src" alt="Image" />
        <p class="cardName">{{ el.name }}</p>
        <p class="cardDescr">{{ el.description }}</p>
        <p class="cardCateg">{{ el }}</p>     
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      data: [],
    };
  },
  components: {
    // HelloWorld,
  },
  async created() {
    var response = await fetch(
      "https://greet.bg/wp-json/wc/store/products?page=1"
    );
    this.data = await response.json()
  },
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
