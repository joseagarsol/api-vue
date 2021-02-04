<template src="./CreateArticle.html"></template>

<script>
import Sidebar from "./Sidebar.vue";
import Global from "../global";
import axios from "axios";
import Article from "../models/Article";
import { required } from "vuelidate/lib/validators";
import swal from "sweetalert";

export default {
  name: "CreateArticle",
  components: {
    Sidebar,
  },
  data() {
    return {
      url: Global.url,
      file: "",
      article: new Article("", "", null, ""),
      submitted: false,
    };
  },
  mounted() {},
  validations: {
    article: {
      title: {
        required,
      },
      content: {
        required,
      },
    },
  },
  methods: {
    save() {
      this.submitted = true;

      this.$v.$touch();
      if (this.$v.$invalid) {
        return false;
      } else {
        axios
          .post(this.url + "save", this.article)
          .then((res) => {
            console.log(res);
            if (res.data.status == "success") {
              //subida de archivo
              if (
                this.file != null &&
                this.file != undefined &&
                this.file != ""
              ) {
                const formData = new FormData();

                formData.append("file0", this.file, this.file.name);

                var articleId = res.data.articleStored._id;
                axios
                  .post(this.url + "upload-image/" + articleId, formData)
                  .then((res) => {
                    if (res.data.article) {
                      swal(
                        "Articulo creado",
                        "El artículo se ha creado correctamente",
                        "success"
                      );
                      this.article = res.data.article;
                      this.$router.push("/blog");
                    } else {
                      //mostrar alerta de error
                      swal(
                        "Creación fallida",
                        "El articulo no se ha guardado correctamente",
                        "error"
                      );
                    }
                  })
                  .catch((err) => {
                    console.log(err);
                  });
              } else {
                swal(
                  "Articulo creado",
                  "El artículo se ha creado correctamente",
                  "success"
                );
                this.article = res.data.article;
                this.$router.push("/blog");
              }
            }
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    fileChange() {
      this.file = this.$refs.file.files[0];
      console.log(this.file);
    },
  },
};
</script>