<script>
  import Knn from './Knn.svelte'
  import NN1 from './Graph1_view.svelte'
</script>
<style>
  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto,
      Oxygen-Sans, Ubuntu, Cantarell, Helvetica Neue, sans-serif;
  }
  .Content-module--content__title--2nRHa {
    font-size: 2.5rem;
    line-height: 3.25rem;
    margin-top: 2.4375rem;
    font-weight: 500;
  }
  .Content-module--content__subtitle--34jJU {
    font-size: 1.875rem;
    line-height: 2.4375rem;
  }
  .Content-module--content__spacer--2kN9Y {
    margin-bottom: 1.625rem;
  }
  .katex-display {
    margin: 1em auto;
    margin-top: 1em;
    margin-right: auto;
    margin-bottom: 1em;
    margin-left: auto;
    display: block;
    text-align: center;
  }
  .katex {
    font: normal 1.21em KaTeX_Main, Times New Roman, serif;
    line-height: 1.2;
    text-indent: 0;
    text-rendering: auto;
  }
  .katex .katex-mathml {
    position: absolute;
    clip: rect(1px, 1px, 1px, 1px);
    padding: 0;
    border: 0;
    height: 1px;
    width: 1px;
    overflow: hidden;
  }
  .katex-display > .katex > .katex-html {
    display: block;
    position: relative;
  }
  .katex .base {
    position: relative;
    white-space: nowrap;
    width: min-content;
  }
  .katex .base,
  .katex .strut,
  .katex .mspace {
    display: inline-block;
  }
  .inline-square {
    margin-left: 5px;
    width: 12px;
    height: 12px;
    display: inline-block;
  }
  .left_tree {
    position: fixed;
    top: 90px;
    width: 16vw;
    height: 100%;
    background: rgba(103, 103, 120, 1);
  }
  .left_tree ul {
    list-style: none;
  }
  .left_tree li a {
    color: white;
  }
  .right_content {
    margin-left: auto;
    margin-right: auto;
    max-width: 100ch;
    height: calc(100% - 90px);
  }
  .name_md {
    font-size: 20px;
    font-weight: 600;
  }
  /* 这是用来固定锚点位置的 具体样式在.name_md里写 */
  .extra {
    border-top: solid 200px #fff;
    margin-top: -200px;
  }
  .picture1 {
    width: 200px;
    height: 200px;
    background-image: url("tmp/Picture1.png");
    background-size: 100% auto;
    color: transparent !important;
  }
</style>

<div class="right_content">
  <div class="name_md extra" id="Zero"></div>
  <div>
    <h2 class="Content-module--content__title--2nRHa ">
      1.决策树——“分而治之”
    </h2>
    <br/>
    <div class="Content-module--content__spacer--2kN9Y" />
    <h5>
      你想起之前有一个学园艺的哥哥，你问他：“这是哪一类的鸢尾花？”他向你询问更多的细节：你先看“它的花萼长吗？”，如果是“长花萼”，我们再看“它的花瓣是怎样的形态？”，是“窄花瓣”，那我们得到最终决策：这是山鸢尾花。
    </h5>
  </div>

  <!-- More -->
  <div class="more">
    <!-- <button class="mdl-button mdl-js-button mdl-button--icon"><i class="material-icons">keyboard_arrow_down</i></button> -->
    <button class="mdl-button mdl-js-button mdl-button--fab">
      <i class="material-icons">keyboard_arrow_down</i>
    </button>
  </div>
  <div class="name_md extra" id="One"><h2>1. 遇到分类问题</h2></div>
  <div>
    <h5>
      你观察到有一块集中的部分的鸢尾有较大的花瓣，而它旁边的一块的花瓣却很小。你想搞清楚他们究竟属于哪一类鸢尾花，于是你上网搜索，发现网上说可以根据一些特征值来刻画事物的某些方面的特点，比如通过花萼的长、花萼的宽、花瓣的长和花瓣的宽来辨别他们。于是你测量了一些花的长和宽。
    </h5>
   
    <h5>***********************一些鸢尾花图片***********************</h5>
  </div>

    <!-- More -->
  <div class="more">
    <!-- <button class="mdl-button mdl-js-button mdl-button--icon"><i class="material-icons">keyboard_arrow_down</i></button> -->
    <button class="mdl-button mdl-js-button mdl-button--fab">
      <i class="material-icons">keyboard_arrow_down</i>
    </button>
  </div>

  <div class="name_md extra" id="One"><h2>2.决策树-简单分析</h2></div>
  <h5>
    网上说宽大于2cm，长大于5cm的可能是山鸢尾。而小于的可能是变色鸢尾，你发现了很多这样的判断，
    你想把他们整理一下，于是你借鉴树的结构将他们分成了这种判断：
  </h5>
  <div>
    <a target="_blank" href="/dt/index.html">尝试下决策树吧</a>
  </div>

  <!-- More -->
  <div class="more">
    <!-- <button class="mdl-button mdl-js-button mdl-button--icon"><i class="material-icons">keyboard_arrow_down</i></button> -->
    <button class="mdl-button mdl-js-button mdl-button--fab">
      <i class="material-icons">keyboard_arrow_down</i>
    </button>
  </div>

  <div class="name_md extra" id="One"><h2>3.初步构造分类器</h2></div>
  <div>
  <h5>
      然后你又换了一种思路
      你将花瓣的长作为x轴，花瓣的宽作为y轴，在坐标轴上画出了他们的分布。

    </h5>
    <h5>***************一些样简单交互*********************</h5>
    <NN1 />
    <h5>
      这就是人工智能中的分类问题。
那么我们要怎么分开他们呢 你想到了用一条直线来分开他们。在直线上方的属于山鸢尾，在直线下方的属于变色鸢尾。这样的方式叫作分类器
但是你也发现了，这里有无数直线都可以的可能来讲他们分开。你想到现在大家都在谈用计算机来帮我们完成一些任务，那么要怎么才能让分类器自己学习得到分类直线呢。
试一试：
    </h5>
    <li>
        <a target="_blank" href="/dist/indexnn.html">尝试下神经网络吧</a>
    </li>
  </div>

    <!-- More -->
  <div class="more">
    <!-- <button class="mdl-button mdl-js-button mdl-button--icon"><i class="material-icons">keyboard_arrow_down</i></button> -->
    <button class="mdl-button mdl-js-button mdl-button--fab">
      <i class="material-icons">keyboard_arrow_down</i>
    </button>
  </div>

  <div class="name_md extra" id="One"><h2>4. KNN</h2></div>
  <div>
    <h5>
      你觉得这样好像还不错。于是你在更远处的一块地，发现那里的鸢尾花好像和前面两种也有不同之处，它们长得更高，更细长。
      于是你认为植物园的工作人员们可能将某一类鸢尾花集中地种在了某一块地方，于是你测量更多的数据重新绘制了一张坐标图。
      但是新的问题来了，要怎么判断中间一些点的种类到底是什么呢。
      于是你想到了一句谚语：“近朱者赤近墨者黑。”
      试一试：
    </h5>
    <li>
        <a target="_blank" href="/dt/index.html">尝试下KNN吧</a>
    </li>
  </div>

  <div id='knnss'>
    <Knn />
  </div>

<div class="name_md extra" id="One"><h2>5.CNN-一气呵成</h2></div>
  <div>
    <h5>
      然后你发现这几种方法各有好坏，也有不足。
而且测量起来特别麻烦。
你想到了我们都在用的一些识别植物的APP，于是你打开了一个软件，拍了一张照片，立马就将它分为了山鸢尾花。这是怎么一回事呢。
这个又用的哪些特征呢。
这个和上面的方法有什么不同之处呢。
试一试：

    </h5>
    <li>
        <a target="_blank" href="/cnn">尝试下CNN吧</a>
    </li>
  </div>

  <!-- More -->
  <div class="more">
    <!-- <button class="mdl-button mdl-js-button mdl-button--icon"><i class="material-icons">keyboard_arrow_down</i></button> -->
    <button class="mdl-button mdl-js-button mdl-button--fab">
      <i class="material-icons">keyboard_arrow_down</i>
    </button>
  </div>

  <div class="name_md extra" id="One"><h2>6.不同模型比较</h2></div>
  <div>
    <h5>
      *****************一些交互***********************
    </h5>
  </div>

</div>