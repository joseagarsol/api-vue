<template>
  <div class="general">
    <Slider :texto="'Búsqueda:' + searchString" />
    <div class="center">
      <section id="content">
        <h1 class="subheader" v-if="articles">Artículos encontrados</h1>
        <h1 class="subheader" v-else>No hay resultados</h1>

        <div id="articles" v-if="articles">
          <Articles :articles="articles" />
          <!--AÑADIR ARTICULOS VIA JS-->
        </div>
        <div v-else>
            No hay artículos que coincidan con tu búsqueda
        </div>
      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import Global from "../global";
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
import Articles from './Articles.vue';

export default {
  name: "Search",
  components: {
    Slider,
    Sidebar,
    Articles
  },
  mounted() {
    this.searchString = this.$route.params.search;
    this.getArticlesBySearch(this.searchString);
  },
  data() {
    return {
      articles: null,
      url: Global.url,
      searchString: null
    };
  },
  methods: {
    getArticlesBySearch(searchString) {
      axios.get(this.url+"search/"+searchString).then((res) => {
        if (res.data.status == "success") {
          this.articles = res.data.articles;
        }
        console.log(this.articles);
      });
    },
  },
};
</script>