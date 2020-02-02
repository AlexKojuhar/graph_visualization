/* eslint-disable no-console */
<template>
  <div class="graph--container"></div>
</template>
<script>
import * as d3 from "d3";

export default {
  name: "Graph",
  props: {
    nodes: { type: Array, default: () => [] },
    links: { type: Array, default: () => [] }
  },
  data() {
    return {};
  },
  watch: {
    links() {
      this.renderGraph();
    }
  },
  methods: {
    renderGraph() {
      var width = 960,
        height = 500;

      var svg = d3
        .select(".graph--container")
        .append("svg")
        .attr("width", width)
        .attr("height", height);

      var simulation = d3
        .forceSimulation()
        .force("charge", d3.forceManyBody().strength(-200))
        .force(
          "link",
          d3
            .forceLink()
            .id(function(d) {
              return d.name;
            })
            .distance(80)
        )
        .force("x", d3.forceX(width / 2))
        .force("y", d3.forceY(height / 2))
        .on("tick", ticked);

      var link = svg.selectAll(".link"),
        node = svg.selectAll(".node");

      simulation.nodes(this.nodes);
      simulation.force("link").links(this.links);

      link = link
        .data(this.links)
        .enter()
        .append("line")
        .attr("class", "link");

      node = node
        .data(this.nodes)
        .enter()
        .append("g")
        .attr("class", "node");

      node.append("circle").attr("r", "30");

      node
        .append("text")
        .attr("dx", -25)
        .attr("dy", 5)
        .text(function(d) {
          return d.name;
        });

      function ticked() {
        link
          .attr("x1", function(d) {
            return d.source.x;
          })
          .attr("y1", function(d) {
            return d.source.y;
          })
          .attr("x2", function(d) {
            return d.target.x;
          })
          .attr("y2", function(d) {
            return d.target.y;
          });

        node.attr("transform", function(d) {
          return "translate(" + d.x + "," + d.y + ")";
        });
      }
    }
  }
};
</script>

<style lang="sass">
.graph--container
  background: rgba(0, 123, 255, 0.25);

.node
  cursor: pointer
  & circle
    stroke: #ffffff
    fill: #6ea4fb
    stroke-width: 2px
  &  text
    font-size: 10px
    text-transform: uppercase
    fill: white;
    font-weight: bold;

.link
  stroke: #ffffff
  stroke-width: 2px
</style>
