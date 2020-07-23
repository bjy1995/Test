<script type="text/javascript">
    import { onMount } from 'svelte';
    import { link } from "svelte-spa-router";
    var m = [20, 120, 20, 120],
        w = 1500 ,
        h = 500 ,
        i = 0,
        rect_width = 100,
        rect_height = 20,
        max_link_width = 20,
        min_link_width = 1.5,
        char_to_pxl = 8,
        root;

    const irislabel = ["花萼长度", "花萼宽度", "花瓣长度", "花瓣宽度"];
    const iristype = ["山鸢尾", "杂色鸢尾", "维吉尼亚鸢尾"];
    var tree;
    var diagonal;
    var vis;
    var gLink;
    var gNode;
    var gtext, plusText;
    var link_stoke_scale;
    var color_map;
    var stroke_callback = "#ccc";

    onMount(async () =>{

        window.scrollTo(0,0);

        const jdata =await d3.json("PUBLIC_URL/assets/data/iris.json");


        tree = d3.tree()
            .size([1000, 1500]);

        diagonal = d3.linkHorizontal()
            .x(d => d.x)
            .y(d => d.y);

        vis = d3.select("#tree-svg")
                .attr("viewBox", [-200, -20, w , h]);

        gLink = vis.append("g")
                .attr("fill", "none")
                .attr("stroke", "#555")
                .attr("stroke-opacity", 0.4);

        gNode = vis.append("g")
                .attr("pointer-events", "all");

        gtext = vis.append("g")
                    .attr('class', 'root-text');
        plusText = gtext.append('text')
            .attr('x', 350)
            .attr('y', 0)
            .attr('class', 'annotation-text')
            .style('dominant-baseline', 'hanging')
            .style('text-anchor', 'middle');
        plusText.append('tspan')
            .style('dominant-baseline', 'hanging')
            .text('点击结点展开');
        plusText.append('tspan')
            .attr('x', 350)
            .attr('dy', '1.5em')
            .style('dominant-baseline', 'hanging')
            .text('决策树');
        plusText.append('tspan')
            .attr('x', 350)
            .attr('dy', '1.5em')
            .style('dominant-baseline', 'hanging')
            .text('当满足结点条件时');
        plusText.append('tspan')
            .attr('x', 350)
            .attr('dy', '1.5em')
            .style('dominant-baseline', 'hanging')
            .text('决策前往');
        plusText.append('tspan')
            .attr('fill', 'green')
            .style('dominant-baseline', 'hanging')
            .text('左分支');
        drawArrow({
            group: gtext,
            sx: 380,
            sy: 15,
            tx: 440,
            ty: 0,
            dr: 50,
            hFlip: 0,
        });

    
        color_map = d3.scaleOrdinal(d3.schemeCategory10);

        load_dataset(jdata);
    })



    function load_dataset(json) {
        root = d3.hierarchy(json);
        root.x0 = 500;
        root.y0 = 1000;

        var n_samples = json.samples;
        var n_labels = json.value.length;

        if (n_labels >= 2) {
            stroke_callback = mix_colors;
        } else if (n_labels === 1) {
            stroke_callback = mean_interpolation(root);
        }

        link_stoke_scale = d3.scaleLinear()
                                    .domain([0, n_samples])
                                    .rangeRound([min_link_width, max_link_width]);

        root.descendants().forEach((d, i) => {
            d.id = i;
            d._children = d.children;
            d.children = null;
            if (d.data.type === "leaf") d.children = null;
        });

        update(root);
    }

    function update(source) {
        var duration = d3.event && d3.event.altKey ? 5000 : 500;
        const nodes = root.descendants().reverse();
        const links = root.links();

        // d3计算树的结构
        tree(root);


        // 设置固定深度
        let ntop = root;
        let nbottom = root;
        nodes.forEach(function(d) { 
            d.y = d.depth * 180;
            if(d.y < ntop.y) ntop = d;
            if(d.y > nbottom.y) nbottom = d;   
        });

        const transition = vis.transition()
            .duration(duration)
            .attr("viewBox", [-200, -20, w , Math.max(300, nbottom.y - ntop.y + 100)])
            .tween("resize", window.ResizeObserver ? null : () => () => vis.dispatch("toggle"));

        const node = gNode.selectAll("g")
                        .data(nodes, d => d.id)

        const nodeEnter = node.enter().append("g")
            .attr("class", "node")
            .attr("transform", d => `translate(${source.y0},${source.x0})`)
            .attr("fill-opacity", 0)
            .attr("stroke-opacity", 0)
            .on("click", d => {
                d.children = d.children ? null : d._children;
                update(d);
                d3.select('.root-text')
                .transition()
                .duration(200)
                .attr("transform", d => `translate(-150, 0)`)
            });
                

        nodeEnter.append("rect")
            .attr("class", d => {return d.data.type === "split" ? "cursor" : "no"})
            .attr("x", function(d) {
                var label = node_label(d);
                var text_len = label.length * char_to_pxl;
                var width = d3.max([rect_width, text_len])
                return -width / 2;
            })
            .attr("rx", function(d) { return d.data.type === "split" ? 2 : 0;})
            .attr("ry", function(d) { return d.data.type === "split" ? 2 : 0;})
            .style("stroke", function(d) { return d.data.type === "split" ? "steelblue" : "olivedrab";})
            .style("fill", function(d) { return d._children ? "#f7f7f8" : "#fff"; });

        nodeEnter.append("text")
            .attr("class", d => { return d._children ? 'cursor': 'no'})
            .attr("dy", "14px") 
            .attr("font-size", "12px")
            .attr("text-anchor", "middle")
            .append('tspan')
            .style('fill', d => { return d._children ? 'red': 'steelblue'})
            .text(node_label);

        // 更新结点位置

        const nodeUpdate = node.merge(nodeEnter).transition()
            .duration(duration)
            .attr("transform", d => `translate(${d.x},${d.y - 5})`)
            .attr("fill-opacity", 1)
            .attr("stroke-opacity", 1);


        nodeUpdate.selectAll("rect")
            .attr("width", rect_width)
            .attr("height", rect_height)
            .attr("fill", function(d) { return d._children ? "#fff" : "#fff"; })
            .style("stroke-width", "1.5px");
        
        d3.selectAll(".cursor")
            .style("cursor", "pointer");

        nodeUpdate.select("text")
            .style("fill-opacity", 1);

        const nodeExit = node.exit().transition().remove()
            .attr("transform", d => `translate(${source.x},${source.y})`)
            .attr("fill-opacity", 0)
            .attr("stroke-opacity", 0);

        // 更新连线
        let link = gLink.selectAll("path")
            .data(links, d => d.target.id);


        const linkEnter = link.enter().append("path")
            .attr("class", "link")
            .attr("d", d => {
            const o = {x: source.x0, y: source.y0};
            return diagonal({source: o, target: o});
            });


        link.merge(linkEnter).transition()
            .attr("d", diagonal)          
            .style("stroke", stroke_callback)
            .style("stroke-width", "5");

        
        link.exit().transition()
            .attr("d", d => {
            const o = {x: source.x, y: source.y};
            return diagonal({source: o, target: o});
            }).remove();


        nodes.forEach(function(d) {
            d.x0 = d.x;
            d.y0 = d.y;
        });
    }

    // 结点标签
    function node_label(d) {
        d = d.data;
        if (d.type === "leaf") {
            // 叶子
            var formatter = d3.format(".2f");
            var vals = [];
            d.value.forEach(function(v) {
                vals.push(v);
            });
            var maxvalue = Math.max(...vals);
            var index = vals.indexOf(maxvalue);
            return iristype[vals.indexOf(Math.max(...vals))];
        } else {
            // 非叶结点
            var label = d.label;
            label = label.replace("X[0]", irislabel[0]);
            label = label.replace("X[1]", irislabel[1]);
            label = label.replace("X[2]", irislabel[2]);
            label = label.replace("X[3]", irislabel[3]);
            return label;
        }
    }

    function mix_colors(d) {
        var value = d.target.data.value;
        var sum = d3.sum(value);
        var col = d3.rgb(0, 0, 0);
        value.forEach(function(val, i) {
            var label_color = d3.rgb(color_map(i));
            var mix_coef = val / sum;
            col.r += mix_coef * label_color.r;
            col.g += mix_coef * label_color.g;
            col.b += mix_coef * label_color.b;
        });
        return col;
    }


    /**
     设置线段颜色.
     */
    function mean_interpolation(root) {

        var max = 1e-9,
            min = 1e9;

        function recurse(node) {
            if (node.value[0] > max) {
            max = node.value[0];
            }

            if (node.value[0] < min) {
            min = node.value[0];
            }

            if (node.children) {
            node.children.forEach(recurse);
            }
        }
        recurse(root);

        var scale = d3.scale.linear().domain([min, max])
                                    .range(["#2166AC","#B2182B"]);

        function interpolator(d) {
            return scale(d.target.data.value[0]);
        }

        return interpolator;
    }

    const drawArrow = (arg) => {
        let group = arg.group,
            sx = arg.sx,
            sy = arg.sy,
            tx = arg.tx,
            ty = arg.ty,
            dr = arg.dr,
            hFlip = arg.hFlip,
            marker = arg.marker === undefined ? 'marker' : arg.marker;
        
        let arrow = group.append('g')
            .attr('class', 'arrow-group');

        arrow.append('path')
            .attr("d", `M${sx},${sy}A${dr},${dr} 0 0,${hFlip ? 0 : 1} ${tx},${ty}`)
            .attr('marker-end', 'url(#markerArrow)')
            .style('stroke', 'gray')
            .style('fill', 'none');
    }

</script>


<style type="text/css">

    #article{
        margin-top:60px;
        margin-bottom: 60px;
        margin-left: auto;
        margin-right: auto;
        max-width: 78ch;
    }
    #treebody{
        position:relative;
        width: 100%;
        /* height: 680px; */
        margin-bottom: 10px;
        font-family: "Helvetica", "Arial", sans-serif;
        background-color: #f7f7f7;
    }
    #foot{
        margin-left: auto;
        margin-right: auto;
        max-width: 78ch;
        position:relative;
        border-top: solid 1px #eee;
        padding: 20px 20px;
    }
    #foot p{
        padding-top: 20px;
    }
    #foot .lasttext{
        font-style: italic;
        font-size: 20px;
        color: rgb(226, 195, 90);
    }
    #treebody {
        width: 100%;
        margin: auto;
        font-family: "Helvetica Neue", Helvetica;
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
        font-style: italic;
        color: steelblue;
    }
    .l--body h2{
        padding-top: 20px;
    }
    .l--body p{
        font-size: 18px;
    }
</style>

<div id="article">
    <div id="One"><h1>Section: 决策树</h1></div>
    <div>
        <h5>
            你想起之前有一个学园艺的哥哥，你问他：“这是哪一类的鸢尾花？”他向你询问更多的细节：你先看“它的花萼长吗？”，如果是“长花萼”，我们再看“它的花瓣是怎样的形态？”，是“窄花瓣”，那我们得到最终决策：这是山鸢尾花。 
        </h5>
    </div>
    <div class="l--body">
      <h2 class='title'>决策树模型</h2>
        <ul> 
            <li>每个“非叶结点”对应于某属性上的一次决策</li> 
            <li>每个结点之后分支对应于该决策的一种可能结果</li>
            <li>每个“叶结点”对应于一个预测结果</li> 
        </ul>
      <p>决策树的学习过程即通过对训练样本的分析来确定“划分属性”（即内部结点所对应的属性），而预测过程就是将测试示例从根结点开始，沿着划分属性所构成的“判定测试序列”下行，直到叶结点</p>
    </div>

    <div class="l--body">
      <h2 class='title'>再来看看这棵树</h2>
        <p>当你在选取判断的时候，你可能像之前那样判断鸢尾花花瓣的长宽。花瓣的长度和宽度是两个不同的“特征”。</p>
        <p>这个特征能否很好的区分鸢尾花的类型呢？你要如何衡量这个判断指标呢？</p>
        <p>统计学里有几种算法可以帮助我们寻找好的切分点，通过划分算法来找到当前最佳的结点划分方式。简单来讲，每一个特征针对训练数据集的前后信息变化的影响是不一样的，当这些衡量参数在划分前后变化越大，即代表这种影响越大。而影响越大，就表明该特征更加重要。</p>
    </div>

    <div class="l--body">
      <h2 class='title'>决策树生长</h2>
      <p>当你点开各个分节点，更多的分支会给决策树增加新的信息，从而增强决策树的预测准确性。加多一层，决策树的准确性增加到84%。再加几层，准确性增加到96%。我们甚至可以继续加分支直到决策树的分类正确率达到100%，这所有决策树末梢的分类结果会是纯粹山鸢尾花或者变色鸢尾，即将两类鸢尾花完全分开。</p>
      <p>决策树的末节通常又叫做叶节点(leaf nodes)。决策树的预测取决于叶节点内哪一类的样本相对较多。</p>
    </div>
    <div class="l--body">
      <h2 class='title'>尝试一下</h2>
      <p>小明现在已经训练好了一个决策树的分类模型了，你要如何帮助他来使用这个模型呢？如果你再测量一朵鸢尾花的值，决策树是怎样做出预测的呢？这一朵新的鸢尾花，从根节点开始，跟着决策树分支的分叉顺序走到叶节点。叶节点是那一类，就预测那一类。每条路径构成了做出预测的规则。这样走到最后便能对这朵鸢尾花的类别做出预测。</p>
    </div>
</div>
<div id="treebody">
    <svg id="tree-svg"></svg>
</div>

<footer id="foot">
    <div class="l--body">
      <h3 class='title'>扩展与补充</h3>
      <p>决策树算法广泛应用于：语音识别、医疗诊断、客户关系管理、模式识别、专家系统等，在实际工作中，必须根据数据类型的特点及数据集的大小，选择合适的算法。想了解更多的相关知识，可以在机器学习相关的书籍中更详细的决策树原理。</p>
    </div>
    <div class="lasttext">
      <p>小明是一个想法很多的同学，他觉得决策树的分类结果并不能够让他十分满意，让我们看看他又尝试了那些方法吧！</p>
    </div>
    <a href="/" use:link rel="prefetch">
        <div class="return">返回</div>
    </a>
</footer>