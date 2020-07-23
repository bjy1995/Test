<script>
  import Router from "svelte-spa-router";
  import { link } from "svelte-spa-router";
  import { routes } from "./route";
</script>

<style>
  #app-page {
    height: 100%;
  }
  #app-page .content {
    padding-top: 90px;
    height: 100%;
    font-family: 'Open Sans', Helvetica, Arial, sans-serif;
  }
  #app-page .page_header {
    position: fixed;
    height: 62px;
    top: 0;
    left: 0;
    width: 100%;
    padding: 0 5vw;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: white;
    z-index: 100;
    box-shadow: 0 -0.4rem 0.9rem 0.2rem rgba(0, 0, 0, 0.5);
    background-color: #37474f;
  }
  #app-page .page_header a {
    text-decoration: none;
    cursor: pointer;
    color: #f6f6f6;
    font-size: 15px;
    font-weight: 500;
  }
  .menu_ul {
    width: 300px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
</style>

<div id="app-page">
  <div class="page_header">
    <a href="/" use:link rel="prefetch" style="font-size: 50px;">UnboxAI</a>
    
    <!-- <ul class="menu_ul">
      <li>
        <a target="_blank" href="/dt/index.html">决策树</a>
      </li>
      <li>
        <a target="_blank" href="/dist/indexnn.html">神经网络</a>
      </li>
      <li>
        <a target="_blank" href="/">KNN</a>
      </li>
      <li>
        <a href="/cnn" use:link rel="prefetch">卷积神经网络</a>
      </li>
    </ul> -->
  </div>

  <div class="content">
    <Router {routes} />
  </div>
</div>

