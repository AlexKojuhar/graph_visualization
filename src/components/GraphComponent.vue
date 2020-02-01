/* eslint-disable no-console */
<template>
  <div class="graph--container">
    
  </div>
</template> 
<script>

import * as d3 from "d3";
// import axios from 'axios';

const data = {
	"nodes": [{
			"name": "red"
		},
		{
			"name": "orange"
		},
		{
			"name": "yellow"
		},
		{
			"name": "green"
		},
		{
			"name": "blue"
		},
		{
			"name": "violet"
		}
	],
	"links": [{
			"source": "red",
			"target": "yellow"
		},
		{
			"source": "red",
			"target": "blue"
		},
		{
			"source": "red",
			"target": "green"
		},
		{
			"source": "violet",
			"target": "green"
		},
		{
			"source": "violet",
			"target": "green"
		}
	]
}

export default {
  name: "Graph",
  data() {
    return {
      data: data
    }
  },
  mounted() {
    var width = 960,
    height = 500
    // eslint-disable-next-line no-console
    console.log(data.nodes);

    var svg = d3.select('.graph--container').append("svg")
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

      simulation.nodes(data.nodes);
      simulation.force("link").links(data.links);

      link = link
        .data(data.links)
        .enter()
        .append("line")
        .attr("class", "link");

      node = node
        .data(data.nodes)
        .enter()
        .append("g")
        .attr("class", "node");

      node
        .append("circle")
        .attr("r", "30")

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