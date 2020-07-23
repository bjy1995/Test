<script>
  import { onMount } from 'svelte';
  import { afterUpdate } from 'svelte';
  import { createEventDispatcher } from 'svelte';

  let simple_network;
  const redraw = () =>{
    var tlw = d3.select("#two_layer_network")
      .attr("width", 200)
      .attr("height", 200);

    tlw.append("circle")
      .attr('cx',50)
      .attr('cy',50)
      .attr('r',15)

    tlw.append("circle")
      .attr('cx',50)
      .attr('cy',150)
      .attr('r',15)

     tlw.append("circle")
      .attr('cx',150)
      .attr('cy',100)
      .attr('r',15);

    tlw.append("line")
            .attr("x1", 50)
            .attr("y1", 50)
            .attr("x2", 150)
            .attr("y2", 100)
            .attr("stroke", "black")
            .attr("stroke-width", "1px");

    tlw.append("line")
            .attr("x1", 50)
            .attr("y1", 150)
            .attr("x2", 150)
            .attr("y2", 100)
            .attr("stroke", "black")
            .attr("stroke-width", "1px");
    
  }
  onMount(async () => {
    redraw();
  });

</script>

  
<div style="display: inline-block; vertical-align: middle;">
  <svg id="two_layer_network" width=100% height=100%></svg>
</div>