<template>
  <section id="articles-component">
    <div id="articles-list" v-if="articles && articles.length >= 1">
      <article
        class="article-item"
        v-for="article in articles"
        :key="article.id"
      >
        <div class="image-wrap">
          <img
            v-if="article.image"
            :src="url + 'get-image/' + article.image"
            :alt="article.title"
          />
          <img
            v-if="!article.image"
            src="../assets/images/default-image.png"
            :alt="article.title"
          />
        </div>

        <h2><router-link :to="{name: 'article', params: {id: article._id}}">{{ article.title }}</router-link></h2>
        <span class="date">{{ article.date | moment("from", "now")}}</span>
        <router-link :to="{name: 'article', params: {id: article._id}}">Leer más...</router-link>

        <div class="clearfix"></div>
      </article>
    </div>
    <div v-else-if="articles && articles.length < 1">
      No hay articulos para mostrar
    </div>
    <div v-else>
      Cargando...
    </div>
  </section>
</template>

<script>
import Global from "../global";
export default {
  name: "Articles",
  props: ["articles"],
  data() {
    return {
      url: Global.url,
    };
  },
};
</script>