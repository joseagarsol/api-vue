<template>
  <div class="general">
    <div class="center">
      <section id="content">
        <h2 class="subheader">Películas</h2>

        <div class="misDatos" v-if="misDatos">
          <p v-html="misDatos"></p>
          <br/>
          {{web | mayusculas | concatenaYear('¡ESTE ES EL MEJOR AÑO!')}}
        </div>

        <div class="favorita" v-if="favorita">
          <h3>La pelicula favorita es: {{favorita.title}}</h3>
        </div>

        <!--Listado articulos-->
        <div id="articles">
          <!-- v-for="pelicula in peliculas" v-bind:key="pelicula.title" -->
          <div v-for="pelicula in peliculasMayusculas" v-bind:key="pelicula.title">
            <Pelicula 
              :pelicula="pelicula"
              v-on:favorita="haLlegadoLaPeliculaFavorita"
             />
          </div>
          <!--AÑADIR ARTICULOS VIA JS-->
        </div>
      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import Pelicula from "./Pelicula.vue";
import Sidebar from './Sidebar.vue';
export default {
  name: "Peliculas",
  components: {
    Pelicula,
    Sidebar
  },
  data() {
    return {
      nombre: 'José Antonio',
      apellidos: 'García Solano',
      web: 'JoseAtonioWeb.com',
      favorita: null,
      peliculas: [
        {
          title: "Batman vs Superman",
          year: 2017,
          image:
            "https://www.entupantalla.com/wp-content/uploads/2020/12/batman-v-superman-bvs-poster2-1280x720-1.jpg",
        },
        {
          title: "Gran torino",
          year: 2015,
          image:
            "data:image/jpeg;base64,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",
        },
        {
          title: "El señor de los anillos",
          year: 2003,
          image:
            "https://www.ecestaticos.com/image/clipping/d91c249813fcefff3c125439a761b969/la-teoria-que-descarta-el-mapa-de-el-senor-de-los-anillos.jpg",
        },
      ],
    };
  },
  methods: {
    haLlegadoLaPeliculaFavorita(favorita){
      this.favorita = favorita;
    }
  },
  filters: {
    mayusculas(value){
      return value.toUpperCase();
    },
    concatenaYear(value, message){
      var date = new Date();

      return value + ' ' + date.getFullYear() + ' ' +message;
    }
  },
  computed: {
    peliculasMayusculas(){
      var peliculasMod = this.peliculas;
      for(var i = 0; i < this.peliculas.length; i++){
        peliculasMod[i].title = peliculasMod[i].title.toUpperCase();
      }
      return peliculasMod;
    },
    misDatos(){
      return this.nombre + '' + this.apellidos + '<br/>' + '<strong>Sitio Web:</strong> '+this.web;
    }
  }
};
</script>