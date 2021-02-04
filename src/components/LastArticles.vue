<template>
  <div class="general">
  <Slider 
  texto="Bienvenido al Curso de Vue con Víctor Robles de victorroblesweb.es"
  home="true"/>
  <div class="center">
    <section id="content">
      <h2 class="subheader">Últimos artículos</h2>

      <!--Listado articulos-->
      <div id="articles">
        <Articles v-bind:articles="articles"/>
      </div>
    </section>
    <Sidebar />
  <div class="clearfix"></div>
  </div>
  </div>
</template>

<script>
import Slider from './Slider.vue'
import Sidebar from './Sidebar.vue'
import Articles from './Articles.vue'
import Global from "../global";
import axios from 'axios'
export default {
  name: "LastArticles",
  components: {
    Slider,
    Sidebar,
    Articles
  },
  mounted() {
    this.getLastArticles();
  },
  data() {
    return {
      articles: null,
      url: Global.url
    };
  },
  methods: {
    getLastArticles() {
      axios.get(this.url+"articles/true").then((res) => {
        if (res.data.status == "success") {
          this.articles = res.data.articles;
        }
        console.log(this.articles);
      });
    },
  },
};
</script>