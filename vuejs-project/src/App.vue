<template>
  <div class="container">
    <!-- FILTERS -->
    <div class="dropdown">
      <!-- Categroies dropdown -->
      <div class="categorieDropdown">
        <label for="Categries">Categroies:</label>
        <select v-model="selectedType">
          <option value="All categroies">All categroies</option>
          <option v-for="categorieDrop in filter_category" :key="categorieDrop">
            {{ categorieDrop }}
          </option>
          <span>{{ selectedType }}</span>
        </select>
      </div>
      <!-- Order by dropdown -->
      <div class="price_nameDropdown">
        <label for="price_names">Order by:</label>
        <select v-model="selectedOrder">
          <option v-for="order in order_by" :key="order">{{ order }}</option>
          <span>{{ selectedOrder }}</span>
        </select>
      </div>
    </div>
    <!-- CARDS -->
    <!-- All Categories -->
    <div
      v-if="selectedType == 'All categroies' || selectedType == ''"
      
    >
    <div>
      <div>
     
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
      </div>
    </div>
    <!-- Selected categorie -->
    <div v-else class="cards_container">
      <div v-for="el in data" :key="el.id">
        <div v-for="filter in el.categories" :key="filter.name">
          <div v-if="filter.name == selectedType" >
            
            <div class="cards">
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
      </div>
    </div>
  </div>
</template>

<script>
import Btn from "./components/add_to_cart_btn.vue";
let startPage = 2;
// let arr = [];
export default {
  name: "App",
  data() {
    return {
      data: [],
      selectedType: "",
      filter_category: [],
      order_by: ["Name", "Price"],
      selectedOrder: "",
    };
  },
  components: {
    Btn,
  },
  // Load 1st page
  async created() {
    var response = await fetch(
      "https://greet.bg/wp-json/wc/store/products?page=1"
    );
    this.data = await response.json();
    this.category_list();
  },
  methods: {
    // Load pages if scroll hit bottom
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
    // Check scroll
    pages: async function () {
      window.addEventListener("scroll", () => {
        if (
          window.scrollY + window.innerHeight >=
          document.documentElement.scrollHeight
        ) {
          this.load_pages();
        }
      });
    },
    // Categories dynamically populated, based on the results, loaded.
    category_list: function () {
      for (let i = 0; i < this.data.length; i++) {
        for (let j = 0; j < this.data[i].categories.length; j++) {
          if (!this.filter_category.includes(this.data[i].categories[j].name)) {
            this.filter_category.push(this.data[i].categories[j].name);
          }
        }
      }
    },
    // Sort by name
    name_list: function () {
      if (this.selectedOrder == "Name") {
        this.data.sort((a, b) => {
          return a.name.localeCompare(b.name);
        });
      }
    },
    
    // price_list: function () {
    //   if(this.selectedOrder == 'Price'){
    //     arr = this.data.prices
    //     arr.sort((a, b) => {
    //       return parseFloat(a.price) - parseFloat(b.price)
    //     })
    //   }
    // },
  },
  beforeUpdate() {
    this.name_list();
    // this.price_list();
  },
  mounted() {
    this.pages();
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
  justify-content: space-between;
  margin: 0 auto;
}
.cards {
  width: 300px;
  height: 430px;
  flex-basis: calc(100% / 5 - 10px);
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
/* .cards2{
    height: 430px;
    width: 250px;
  margin-bottom: 15px;

} */
</style>
