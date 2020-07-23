<script>
  import Knn from "./Knn.svelte";
  import Tree from "./Tree.svelte";
  import { onMount } from "svelte";
  import { tick } from "svelte";
  import { link } from "svelte-spa-router";

  var cont;
  var elsArr;
  var closeBtnsArr;
  var dcont;
  var selected = 0;
  var dynamiccss = "cont s--inactive";
  // 不加onMount  cont取不到值
  onMount(async () => {
    cont = document.querySelector(".cont");
    elsArr = [].slice.call(document.querySelectorAll(".el"));
    closeBtnsArr = [].slice.call(document.querySelectorAll(".el__close-btn"));

    setTimeout(function() {
      // cont.classList.remove('s--inactive');
      dynamiccss = "cont";
    }, 200);

    elsArr.forEach(function(el, index) {
      el.addEventListener(
        "click",
        function() {
          selected = index + 1;
          // if (this.classList.contains('s--active')) return;
          // cont.classList.add('s--el-active');
          // this.classList.add('s--active');
        },
        true
      );
    });

    closeBtnsArr.forEach(function(btn) {
      btn.addEventListener(
        "click",
        function(e) {
          e.stopPropagation();
          selected = 0;
          // cont.classList.remove('s--el-active');
          // document.querySelector('.el.s--active').classList.remove('s--active');
        },
        true
      );
    });
  });
</script>

<style>
  #begin {
    margin-top: 60px;
    margin-bottom: 60px;
    margin-left: auto;
    margin-right: auto;
    max-width: 78ch;
  }
  .t {
    font-size: 20px;
  }
  .flower {
    color: steelblue;
  }
</style>

<div id="begin">
  <p>
    “UnboxAI打造了一个全套的教学生态系统来辅助智慧校园中开设的人工智能的课程，可以脱离纸质书籍教学，所有的内容均在电脑或者平板上完成。UnboxAI以第一人称的角色，通过算法可视化，用户可交互性带给同学们更强的体验感，有助于更好地理解人工智能算法背后的原理并激发同学们继续深入学习机器学习、深度学习的相关数学、编程知识。”
  </p>
  <br/>
  <h1 style="color: peru;">欢迎来到UnboxAI，在这里我们将以情节的方式，在不同的故事线中，由浅入深地带你学习人工智能的相关知识。</h1>
  <br/>
  <br/>
  <div id="one">
    <h1 style="font-weight:500 ">故事线——植物园（分类任务）</h1>
  </div>
  <div id="article">
    <p class="t">
      学校组织去植物园春游，一片特别好看的蓝紫色的花海映入你眼帘，像一只只穿梭在绿草中的美丽蝴蝶。
    </p>
    <p class="t">
      你看到牌上的介绍说：这是鸢尾花，常见的有三大类:
      <b class="flower">山鸢尾</b>
      (Iris-setosa)，
      <b class="flower">变色鸢尾</b>
      (Iris-versicolor)和
      <b class="flower">维吉尼亚鸢尾</b>
      (Iris-virginica)。
    </p>
    <p class="t">
      它们有着形状与色彩相似的花瓣和萼片。你观察到有一块集中的部分的鸢尾有较大的花瓣，而它旁边的一块的花瓣却很小。你想搞清楚它们究竟属于哪一类鸢尾花。
    </p>
    <p class="t">
      于是你上网搜索，发现网上说可以根据一些特征值来刻画事物的某些方面的特点，比如通过花萼的长、花萼的宽、花瓣的长和花瓣的宽来辨别他们。
    </p>
    <p class="t">
      <b>让我们从section1开始吧！</b>
    </p>
  </div>
</div>

<div class={selected > 0 ? 'cont s--el-active' : dynamiccss}>
  <!-- cont inner start -->
  <div class="cont__inner">
    <!-- el start -->
    <div class={selected === 1 ? 'el s--active' : 'el'}>
      <div class="el__overflow">
        <div class="el__inner">
          <div class="el__bg" />
          <div class="el__preview-cont">
            <h2 class="el__heading" style="color:#f6f6f6">Section 1</h2>
          </div>
          <div class="el__content">
            <a href="/old" use:link rel="prefetch">
              <div class="el__text">Enter Section 1</div>
            </a>
            <div class="el__close-btn" />
          </div>
        </div>
      </div>
      <div class="el__index">
        <div class="el__index-back">1</div>
        <div class="el__index-front">
          <div class="el__index-overlay" data-index="1">1</div>
        </div>
      </div>
    </div>
    <!-- el end -->
    <!-- el start -->
    <div class={selected === 2 ? 'el s--active' : 'el'}>
      <div class="el__overflow">
        <div class="el__inner">
          <div class="el__bg" />
          <div class="el__preview-cont">
            <h2 class="el__heading" style="color:#f6f6f6">Section 2</h2>
          </div>
          <div class="el__content">
            <a href="/knn" use:link rel="prefetch">
              <div class="el__text">Enter Section 2</div>
            </a>
            <div class="el__close-btn" />
          </div>
        </div>
      </div>
      <div class="el__index">
        <div class="el__index-back">2</div>
        <div class="el__index-front">
          <div class="el__index-overlay" data-index="2">2</div>
        </div>
      </div>
    </div>
    <!-- el end -->
    <!-- el start -->
    <div class={selected === 3 ? 'el s--active' : 'el'}>
      <div class="el__overflow">
        <div class="el__inner">
          <div class="el__bg" />
          <div class="el__preview-cont">
            <h2 class="el__heading" style="color:#f6f6f6">Section 3</h2>
          </div>
          <div class="el__content">
            <a href="/tree" use:link rel="prefetch">
              <div class="el__text">Enter Section 3</div>
            </a>
            <div class="el__close-btn" />
          </div>
        </div>
      </div>
      <div class="el__index">
        <div class="el__index-back">3</div>
        <div class="el__index-front">
          <div class="el__index-overlay" data-index="3">3</div>
        </div>
      </div>
    </div>
    <!-- el end -->
    <!-- el start -->
    <div class={selected === 4 ? 'el s--active' : 'el'}>
      <div class="el__overflow">
        <div class="el__inner">
          <div class="el__bg" />
          <div class="el__preview-cont">
            <h2 class="el__heading" style="color:#f6f6f6">Section 4</h2>
          </div>
          <div class="el__content">
            <a href="/cnn" use:link rel="prefetch">
              <div class="el__text">Enter Section 4</div>
            </a>
            <div class="el__close-btn" />
          </div>
        </div>
      </div>
      <div class="el__index">
        <div class="el__index-back">4</div>
        <div class="el__index-front">
          <div class="el__index-overlay" data-index="4">4</div>
        </div>
      </div>
    </div>
    <!-- el end -->
    <!-- el start -->
    <div class={selected === 5 ? 'el s--active' : 'el'}>
      <div class="el__overflow">
        <div class="el__inner">
          <div class="el__bg" />
          <div class="el__preview-cont">
            <h2 class="el__heading" style="color:#f6f6f6">Section 5</h2>
          </div>
          <div class="el__content">
            <a href="/" use:link rel="prefetch">
              <div class="el__text">Enter Section 5</div>
            </a>
            <div class="el__close-btn" />
          </div>
        </div>
      </div>
      <div class="el__index">
        <div class="el__index-back">5</div>
        <div class="el__index-front">
          <div class="el__index-overlay" data-index="5">5</div>
        </div>
      </div>
    </div>
    <!-- el end -->
  </div>
  <!-- cont inner end -->
</div>
<footer style="background-color: #37474f">
  <span
    style="text-align: right;color: #f6f6f6;font-weight:300;;font-size: 13px">
    Deecamp2020 4-2 UnboxTherapy
  </span>
</footer>
