<script>
  export let input1 ;
  export let input2 ;
  export let weight1 ;
  export let weight2 ;
  let output ;

  import { createEventDispatcher } from 'svelte';
  import Graph1Dataview from './Graph1_Dataview.svelte';
  
  const dispatch = createEventDispatcher();

  function calculate(){
    output  = input1 * weight1 + input2 * weight2;
  }
  function handleMouseover(event) {
    let outputMappings = generateOutputMappings(stride, output, kernel.length, image.length, dilation);
    outputHighlights = array1d(output.length * output.length, (i) => false);
    const animatedH = event.detail.hoverH;
    const animatedW = event.detail.hoverW;
    outputHighlights[animatedH * output.length + animatedW] = true;
    inputHighlights = compute_input_multiplies_with_weight(animatedH, animatedW, image.length, kernel.length, outputMappings, kernel.length)
    isPaused = true;
    dispatch('message', {
      text: isPaused
    });
  }
</script>

<style>
  .wrapper {
    display: flex;
    flex-direction: row;
    align-items: flex-end;
  }

  .column {
    padding: 5px 10px 10px 10px;
  }

  .header-text {
    line-height: 1.1;
  }
</style>

<div class="wrapper">
  <div class="column has-text-centered">
    <div class="header-text">
      2 inputs and 1 output<br/>
    </div>
    <Graph1Dataview on:message={handleMouseover}  input1={input1} input2={input2} weight1={weight1}
            weight2={weight2} output={output}/>
    <p>output:{output}</p>
    <button on:click={calculate}>new output</button>
  </div>
</div>