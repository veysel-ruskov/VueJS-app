<template>
  <div class="container">
    <!-- FILTERS -->
    <div class="dropdown">
      <!-- Categroies dropdown -->
      <div class="categorieDropdown">
        <label for="Categries">Categroies:</label>
        <select v-model="selectedType">
          <option v-for="categorieDrop in filter_category" :key="categorieDrop">
            {{ categorieDrop }}
          </option>
        </select>
      </div>
      <!-- Order by dropdown -->
      <div class="price_nameDropdown">
        <label for="price_names">Order by:</label>
        <select>
          <!-- sort by ........ || v-for sort-option & data() sort: {name,price}-->
          <option value="default">None</option>
          <option value="name">Name</option>
          <option value="price">Price</option>
        </select>
      </div>
    </div>
    <!-- CARDS -->
    <div class="cards_container">
      <div v-for="el in data" :key="el.id" class="cards">
        <img class="imgCard" :srcset="el.images[0].srcset" alt="Image" />
        <p class="cardName">{{ el.name }}</p>
        <p class="cardDescr" v-if="el.description != ''">
          {{ el.description }}
        </p>
        <p
          class="cardCategorie"
          v-for="categrie in el.categories"
          :key="categrie.id"
        >
          {{ categrie.name }}
        </p>
        <Btn :href="'https://greet.bg/?add-to-cart=' + el.id"></Btn>
      </div>
    </div>
  </div>
</template>

<script>
import Btn from "./components/add_to_cart_btn.vue";
let startPage = 2;
export default {
  name: "App",
  data() {
    return {
      data: [],
      selectedType: "",
      filter_category: [],
    };
  },
  components: {
    Btn,
  },
  async created() {
    var response = await fetch(
      "https://greet.bg/wp-json/wc/store/products?page=1"
    );
    this.data = await response.json();
    this.category_list();
  },
  methods: {
    load_pages: async function () {
      var response = await fetch(
        "https://greet.bg/wp-json/wc/store/products?page=" + startPage
      );
      var dataRes = await response.json();

      for (let i = 0; i < dataRes.length; i++) {
        this.data.push(dataRes[i]);
      }
      startPage++;
      this.category_list();
    },
    pages: function () {
      window.addEventListener("scroll", () => {
        if (
          window.scrollY + window.innerHeight >=
          document.documentElement.scrollHeight
        ) {
          this.load_pages();
        }
      });
    },
    category_list: function () {
      for (let i = 0; i < this.data.length; i++) {
        for (let j = 0; j < this.data[i].categories.length; j++) {
          if (!this.filter_category.includes(this.data[i].categories[j].name)) {
            this.filter_category.push(this.data[i].categories[j].name);
          }
        }
      }
    },
    selectedCategorieFiltering: function () {
      // if (selectedCategorie == data.categori) { display } ? || watch () { selectedCategorie} ?
    },
  },
  mounted() {
    this.pages();
    this.selectedCategorieFiltering();
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
  width: calc(100% / 6 - 10px);
  height: 430px;
  flex-basis: calc(100% / 6 - 10px);
  margin-bottom: 15px;
}
.btn_add_to_cart {
  visibility: hidden;
}
.cards:hover {
  transition: 0.2s;
  border-radius: 20px;
  box-shadow: 0px 3px 15px #9e9e9e;
}
.cards:hover .cardCategorie {
  display: none;
}
.cards:hover .btn_add_to_cart {
  visibility: visible;
}
.imgCard:hover {
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
/* .categorieDropdown {
}
.price_nameDropdown {
} */
.dropdown {
  display: flex;
  justify-content: space-between;
}
</style>
