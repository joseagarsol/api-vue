<template>
  <div class="general">
    <Slider texto="Blog" />
    <div class="center">
      <section id="content">
        <h1 class="subheader">Blog</h1>

        <div id="articles" v-if="articles">
          <Articles :articles="articles" />
          <!--AÑADIR ARTICULOS VIA JS-->
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
  name: "Blog",
  components: {
    Slider,
    Sidebar,
    Articles
  },
  mounted() {
    this.getArticles();
  },
  data() {
    return {
      articles: null,
      url: Global.url
    };
  },
  methods: {
    getArticles() {
      axios.get(this.url+"articles").then((res) => {
        if (res.data.status == "success") {
          this.articles = res.data.articles;
        }
        console.log(this.articles);
      });
    },
  },
};
</script>