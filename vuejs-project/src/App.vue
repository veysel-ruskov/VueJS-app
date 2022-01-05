<template>
  <div>
    <div class="cards_container">
      <div v-for="el in data" :key="el.id" class="cards">
        <img class="imgCard" :srcset="el.images[0].srcset" alt="Image" />
        <p class="cardName">{{ el.name }}</p>
        <p class="cardDescr" v-if="el.description != ''">{{ el.description }}</p>
        <p class="cardCategorie" v-for="categrie in el.categories" :key="categrie.id">{{ categrie.name }}</p>
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
  width: 100%;
  height: 300px;
  border-radius: 20px;
}
.cards_container {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
  max-width: 100vw;
  margin: 0 auto;
}
.cards {
  width: calc(100%/6 - 10px);
  height: 430px;
  flex-basis: calc(100%/6 - 10px);  
  margin-bottom: 15px;
}
.btn_add_to_cart {
  visibility: hidden;
}
.cards:hover {
    transition: .2s;
    border-radius: 20px;
    box-shadow: 0px 3px 15px #9e9e9e;
}
.cards:hover .cardCategorie{
  display: none;
}
.cards:hover .btn_add_to_cart {
  visibility: visible;
}
.imgCard:hover{
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}
.cardCategorie {
  height: 5px;
  font-size: 11px;
  color: grey;
  line-height: 0px;
}
.cardName {
  color: grey;
  font-size: 18px;
  margin-top: 16px !important;

}
</style>
