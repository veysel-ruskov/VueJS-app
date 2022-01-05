<template>
  <div>
    <div class="cards_container">
      <div v-for="el in data" :key="el.id" class="cards">
        <img class="imgCard" :src="el.images[0].src" alt="Image" />
        <h3 class="cardName">{{ el.name }}</h3>
        <p class="cardDescr" v-if="el.description != ''">{{ el.description }}</p>
        <p class="cardCategorie" v-for="categrie in el.categories" :key="categrie.id">{{categrie.name}}</p>
        <Btn class="btn_add_to_cart"></Btn>
      </div>
    </div>
  </div>
</template>

<script>
import Btn from "./components/add_to_cart_btn.vue"
export default {
  name: "App",
  data() {
    return {
      data: [],
    };
  },
  components: {
    Btn,
  },
  async created() {
    var response = await fetch(
      "https://greet.bg/wp-json/wc/store/products?page=1"
    );
    this.data = await response.json()
    console.log(this.data)
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
.imgCard {
  width: 230px;
  height: 300px;
  border-radius: 7%;
}
.cards_container {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
  max-width: 100vw;
  margin: 0 auto;
}
.cards {
  max-width: calc(100vw/6);
}
.btn_add_to_cart {
  display: none;
}
.cards:hover {
  border: 2px solid ;
  border-color: red;
}
.cards:hover .cardCategorie{
  display: none;
}
.cards:hover .btn_add_to_cart {
  display: block;
}
.imgCard:hover{
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}
</style>
