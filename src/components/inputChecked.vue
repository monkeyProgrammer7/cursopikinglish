<template>
  <div>
    <div class="input-group">
      <div v-if="conTexto" style="margin-right:10px;">
        <h5 v-html="textoA">{{textoA}}</h5>
      </div>
      <b-form-input
        :name="this.name"
        :id="this.name+esperado[0]"
        v-model="text"
        :state="this.contiene()"
        aria-describedby="input-live-help input-live-feedback"
        trim
      ></b-form-input>
      <div class="input-group-append">
        <button class="btn btn-outline-secondary" type="button" v-on:click="completar">
          <i class="fa fa-eye"></i>
        </button>
      </div>
      <div v-if="conTexto" style="margin-left:10px;">
        <h5 id="inputText" v-html="textoD">{{textoD}}</h5>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  props: {
    esperado: Array,
    resuelto: Boolean,
    name: "",
    conTexto: Boolean,
    textoA: String,
    textoD: String,
  },
  data() {
    return {
      text: "",
      esperado1: this.esperado,
    };
  },
  methods: {
    completar() {
      this.text = this.esperado[0];
    },
    quitarAcentos(cadena) {
      const acentos = {
        á: "a",
        é: "e",
        í: "i",
        ó: "o",
        ú: "u",
        Á: "A",
        É: "E",
        Í: "I",
        Ó: "O",
        Ú: "U",
      };
      return cadena
        .split("")
        .map((letra) => acentos[letra] || letra)
        .join("")
        .toString();
    },
    contiene() {
      //console.log(this.esperado1)
      this.text = this.text.replace("´", "'");
      if (
        this.esperado1.includes(this.quitarAcentos(this.text.toLowerCase()))
      ) {
        this.$emit("output", this.text);
        return true;
      }
      return false;
    },
  },
  mounted() {
    if (this.resuelto) {
      this.text = this.esperado1[0];
    }
    var nuevo2 = [];
    for (var i = 0; i < this.esperado1.length; i++) {
      nuevo2.push(
        this.quitarAcentos(this.esperado1[i]).toLowerCase().replace("´", "'")
      );
    }
    this.esperado1 = nuevo2;
  },
};
</script>

<style scoped>
.input-group-append {
  height: 100%;
}
/deep/ .input-group > .form-control:not(:first-child),
.input-group > .custom-select:not(:first-child) {
  border-top-left-radius: 0.4em;
  border-bottom-left-radius: 0.4em;
}
.input-group > .input-group-prepend > .btn,
.input-group > .input-group-prepend > .input-group-text,
.input-group > .input-group-append:not(:last-child) > .btn,
.input-group > .input-group-append:not(:last-child) > .input-group-text,
.input-group
  > .input-group-append:last-child
  > .btn:not(:last-child):not(.dropdown-toggle),
.input-group
  > .input-group-append:last-child
  > .input-group-text:not(:last-child) {
  border-top-right-radius: 0.4em;
  border-bottom-right-radius: 0.4em;
}
</style>