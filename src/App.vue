<template>
  <div id="app">
  <b-container fluid>
    <b-row>
      <b-col cols="3"><Form/></b-col>
      <b-col cols="9"><Graph/></b-col>
    </b-row>
  </b-container>
  </div>
</template>

<script>
import Graph from './components/GraphComponent.vue'
import Form from './components/FormComponent.vue'

export default {
  name: 'app',
  components: {
    Graph,
    Form
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
