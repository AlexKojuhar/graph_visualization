<template>
  <div class="form--container">
    <b-tabs content-class="mt-3">
      <b-tab title="Create Node" active @click="clean_nodes">
        <div class="mb-1">Node name:</div>
        <b-input v-model="new_node" placeholder="Enter node name" />
        <b-button class="mt-3" variant="primary" @click="create_node"
          >Create Node</b-button
        >
      </b-tab>
      <b-tab title="Create Link" @click="all_nodes">
        <div class="mb-1">Source Node:</div>
        <b-form-select v-model="selected_source" :options="options">
          <template v-slot:first>
            <b-form-select-option :value="null" disabled
              >Please select a source node</b-form-select-option
            >
          </template>
        </b-form-select>
        <div class="mt-2 mb-1">Target Node:</div>
        <b-form-select v-model="selected_target" :options="options">
          <template v-slot:first>
            <b-form-select-option :value="null" disabled
              >Please select a target node</b-form-select-option
            >
          </template>
        </b-form-select>
        <b-button class="mt-3" variant="primary" @click="create_link"
          >Create Link</b-button
        >
      </b-tab>
    </b-tabs>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Form",
  data() {
    return {
      new_node: "",
      selected_source: null,
      selected_target: null,
      options: []
    };
  },
  created() {},
  methods: {
    all_nodes() {
      axios.get("http://127.0.0.1:8000/graph/nodes/").then(response => {
        response.data.map(item => {
          this.options.push({ value: item.id, text: item.name });
        });
      });
    },
    clean_nodes() {
      this.options = [];
    },
    create_node() {
      if (this.new_node) {
        axios
          .post("http://127.0.0.1:8000/graph/nodes/", { name: this.new_node })
          .then(response => {
            if (response.status === 201) {
              this.$emit("updateGraph", response.data);
              return (this.new_node = "");
            }
            // eslint-disable-next-line no-console
            console.log(response);
          });
      }
    },
    create_link() {
      if (this.selected_source && this.selected_target) {
        axios
          .post("http://127.0.0.1:8000/graph/links/", {
            source: parseInt(this.selected_source),
            target: parseInt(this.selected_target)
          })
          .then(response => {
            if (response.status === 201) {
              this.$emit("updateGraph", response.data);
              return (
                (this.selected_source = null), (this.selected_target = null)
              );
            }
            // eslint-disable-next-line no-console
            console.log(response);
          });
      }
    }
  }
};
</script>
