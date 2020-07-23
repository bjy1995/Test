<script>
  import Graph1Animator from './Graph1_Animator.svelte';
  let inputNode1 ;
  let inputNode2 ;
  let weight_1 ;
  let weight_2 ;

  function handleClickPause() {
    isPaused = !isPaused;
  }

  function handlePauseFromInteraction(event) {
    isPaused = event.detail.text;
  }

</script>

<style>
  .control-button {
    position: absolute;
    top: 5px;
    right: 15px;
    color: gray;
    font-size: 22px;
    opacity: 0.4;
    cursor: pointer;
  }

  .control-button:hover {
    opacity: 0.8;
  }

  .box {
    padding: 5px 30px 20px 30px;
    position: relative;
  }

  .left-part {
    display: flex;
    flex-direction: column;
    margin-top: 30px;
  }

  .right-part {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .content-container {
    display: flex;
    justify-content: space-around;
  }

  .field {
    padding-top: 5px;
  }

  .annotation {
    display: flex;
    align-items: center;
    padding-left : 10px;
  }

  .annotation-text-hyper {
    font-size: 15px;
    font-style: italic;
  }

  .annotation > img {
    width: 20px;
    margin-right: 5px;
  }

  .is-very-small {
    font-size: 12px; 
  }

  .field {
    align-items: center;
  }

  .field-label.is-normal {
    padding-top: 0;
  }

  .field:not(:last-child) {
    margin-bottom: 7px;
  }

  label {
    display: inline-block;
    width: 105px;
    text-align: right;
    font-weight: 500;
    color: #4a4a4a;
  } 

  input[type=number] {
    width: 50px;
  }

  input[type=range] {
    width: 160px;
  }
</style>

<div class="container has-text-centered" id="detailview-container">
  <div class="box">

    <div class="content-container">
      <div class="left-part">
        <div class="input-row">
          <div class="field is-horizontal">
            <div class="field-label is-normal">
              <label class="label">Input Node1:</label>
            </div>
            <input class="input is-very-small" type="number" bind:value={inputNode1}>
          </div>
        </div>

      <div class="input-row">
          <div class="field is-horizontal">
            <div class="field-label is-normal">
              <label class="label">Input Node2:</label>
            </div>
            <input class="input is-very-small" type="number" bind:value={inputNode2}>
          </div>
        </div>

        <div class="input-row">
          <div class="field is-horizontal">
            <div class="field-label is-normal">
              <label class="label">Weight1:</label>
            </div>
            <input class="input is-very-small" type="number" bind:value={weight_1}>
          </div>
        </div>

        <div class="input-row">
          <div class="field is-horizontal">
            <div class="field-label is-normal">
              <label class="label">Weight2:</label>
            </div>
            <input class="input is-very-small" type="number" bind:value={weight_2}>
          </div>
        </div>

      </div>

        <div class="right-part">
          <Graph1Animator on:message={handlePauseFromInteraction} 
            input1={inputNode1} input2={inputNode2} weight1={weight_1}
            weight2={weight_2}/>       
        </div>

    </div>

  </div>
</div>