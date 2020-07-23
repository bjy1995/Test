<script>
  import Overview from "../overview/Overview.svelte";
  import Article from "../article/article.svelte";
  import { cnnStore } from "../stores.js";
  import { link } from "svelte-spa-router";

  // Enum to control the displaying view
  const View = {
    OVERVIEW: "overview",
    LAYERVIEW: "layerview",
    DETAILVIEW: "detailview"
  };

  let mainView = View.OVERVIEW;

  /* Example to read loaded cnn in other components:
  $: if ( $cnnStore.length != 0) {
    console.log($cnnStore);
  }
  */
</script>

<style>
  #article{
    border-bottom: solid 2px rgb(211, 208, 208);
  }
  #explainer {
    width: 100%;
    padding: 0;
    margin: auto;
    /* outline: 1px solid var(--g-dark-gray); */
  }
  #foot{
    margin-left: auto;
    margin-right: auto;
    max-width: 78ch;
    position:relative;
    border-top: solid 1px #eee;
  }
  #foot .lasttext p{
    padding-top: 10px;
    font-style: italic;
    font-size: 18px;
    color: rgb(226, 195, 90);
  }
  #foot .addition p{
    padding-top: 0px;
    font-size: 20px;
  }
  .return {
    position: absolute;
    right: -300px;
    top: 80%;
    font-size: 20px;
    font-style: italic;
    color: steelblue;
  }
  .title{
    padding-top: 20px;
    padding-bottom: 0px;
    font-style: italic;
    color: steelblue;
    font-size: 30px;
  }
</style>
<div id="article">
  <Article />
</div>
<div id="explainer">
  <Overview />
</div>
<footer id="foot">
    <p class='title'><b>扩展与补充</b></p>
    <div class="addition">
      <p>待补充</p>
    </div>
    <div class="lasttext">
      <p>待补充</p>
    </div>
    <a href="/" use:link rel="prefetch">
        <div class="return">返回</div>
    </a>
</footer>