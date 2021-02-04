<template>
  <div class="general">
    <Slider texto="Página de pruebas"/>
    <div class="center">
      <section id="content">
        <h2 class="subheader">Pagina de pruebas</h2>
        <h3>{{ id_desde_url }}</h3>
        <button @click="actualizarNombre()">Actualizar nombre</button>
        <button @click="redirigirAlBlog()">Redirigir</button>
        <h4>{{ nombre }}</h4>
      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>
<script>
import Slider from './Slider.vue'
import Sidebar from './Sidebar.vue'
export default {
  name: "Pagina",
  components: {
    Slider,
    Sidebar
  },
  created() {
    console.log("componente creado");
  },
  mounted() {
    this.id_desde_url = this.$route.params.id;
    console.log("Componente montado");
  },
  updated() {
    console.log("componente actualizado");
  },
  destroyed() {
    console.log("componente eliminado");
  },
  data() {
    return {
      id_desde_url: null,
      nombre: "Jose Antonio",
    };
  },
  methods: {
    actualizarNombre() {
      this.nombre = "Jose Antonio Garcia Solano";
      console.log(this.nombre);
    },
    redirigirAlBlog() {
      this.$router.push("/blog");
    },
  },
};
</script>