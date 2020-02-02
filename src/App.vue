<template>
  <div id="app">
    <b-container fluid>
      <b-row>
        <b-col cols="3"><Form /></b-col>
        <b-col cols="9"><Graph :nodes="nodes" :links="links"/></b-col>
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
      nodes: [],
      links: []
    };
  },
  beforeMount() {
    axios.get("http://127.0.0.1:8000/graph/nodes/").then(response => {
      this.nodes = response.data;
      console.log(this.nodes);
    });
    axios.get("http://127.0.0.1:8000/graph/links/").then(response => {
      this.links = response.data;
      console.log(this.links);
    });
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
