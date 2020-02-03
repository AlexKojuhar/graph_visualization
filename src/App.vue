<template>
  <div id="app">
    <b-container fluid>
      <b-row>
        <b-col cols="3"><Form @updateGraph="updateGraph"/></b-col>
        <b-col cols="9"
          ><Graph :key="data_key" :links="data.links" :nodes="data.nodes"
        /></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Graph from "./components/GraphComponent.vue";
import Form from "./components/FormComponent.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Graph,
    Form
  },
  data() {
    return {
      data: {
        nodes: [],
        links: []
      },
      data_key: 0
    };
  },
  beforeMount() {
    this.getNodes();
    this.getLinks();
  },
  methods: {
    getNodes() {
      axios
        .get(`${this.$hostname}/graph/nodes/`)
        .then(response => {
          this.data.nodes = response.data;
        })
        .catch(error => {
          console.log(error.response);
        });
    },
    getLinks() {
      axios
        .get(`${this.$hostname}/graph/links/`)
        .then(response => {
          this.data.links = response.data;
        })
        .catch(error => {
          console.log(error.response);
        });
    },
    updateGraph(event) {
      if (event) {
        this.getNodes();
        this.getLinks();
        this.data_key = event.id;
      }
    }
  }
};
</script>

<style lang="sass">
#app
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  color: #2c3e50
  margin-top: 20px
  font-size: 14px
  .custom-select, button, input
    font-size: 14px!important;
</style>
