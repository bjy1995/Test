<script>
	import HyperparameterView from '../detail-view/Hyperparameterview.svelte';

	let softmaxEquation = `$$\\text{Softmax}(x_{i}) = \\frac{\\exp(x_i)}{\\sum_j \\exp(x_j)}$$`;
	let reluEquation = `$$\\text{ReLU}(x) = \\max(0,x)$$`;

  let currentPlayer;
</script>

<style>
	#description {
    margin-bottom: 60px;
    margin-left: auto;
    margin-right: auto;
    max-width: 78ch;
  }

  #description h2 {
    color: #444;
    font-size: 40px;
    font-weight: 450;
    margin-bottom: 12px;
    margin-top: 60px;
  }

  #description h4 {
    color: #444;
    font-size: 32px;
    font-weight: 450;
    margin-bottom: 8px;
    margin-top: 44px;
  }

  #description h6 {
    color: #444;
    font-size: 24px;
    font-weight: 450;
    margin-bottom: 8px;
    margin-top: 44px;
  }

  #description p {
    margin: 16px 0;
  }

  #description p img {
    vertical-align: middle;
  }

  #description .figure-caption {
    font-size: 13px;
    margin-top: 5px;
  }

  #description ol {
    margin-left: 40px;
  }

  #description p, 
  #description div,
  #description li {
    color: #555;
    font-size: 17px;
    line-height: 1.6;
  }

  #description small {
    font-size: 12px;
  }

  #description ol li img {
    vertical-align: middle;
  }

</style>

<body>
  <div id="description">
    <h2><strong>什么是卷积神经网络？</strong></h2>
    <p>
		在机器学习中，分类器将类别标签分配给数据点。例如，一个 <em>图像分类器</em> 针对图像中存在哪些对象产生类别标签（例如：鸟，飞机）。一个 <em>卷积神经网络</em>，或CNN作简称，是一种非常适合解决这种问题类型的分类器！
	 </p>
  	<p>
  		CNN是神经网络: 一种用于识别数据模式的算法。通常，神经网络由神经元组成，这些神经元分层组织，每个神经元都有自己可通过学习获的权重和误差。 让我们将CNN进一步分解为更基本的构建：
  	</p>
  	<ol>
  		<li><strong>张量：</strong> 可以被认为是一个n维矩阵。在上面的CNN中，张量将是3维的，输出层除外。</li>
  		<li><strong>神经元：</strong> 可以被看作是发生在多个输入并且产生一个输出的功能。神经元的输出在上面表示为 <span style="color:#FF7577;">红色</span> &rarr; <span style="color:#60A7D7;">蓝色</span> <strong> 蓝色激活图</strong>。</li>
  		<li><strong>层：</strong> 是简单地用相同的操作的神经元，包括相同超参数的集合。</li>
  		<li><strong>内核权重和偏差：</strong>虽然对于每个神经元而言都是唯一的， 它们在训练阶段进行了调整，来使分类器适应所提供的问题和数据集。它们在可视化中以 <span style="color:#BC8435;">黄色</span> &rarr; <span style="color:#39988F;">绿色</span> 的发散色标来进行编码。通过单击神经元或将鼠标悬停在 <em>卷积弹性解释视图</em>中的内核/偏差上，可以在 <em>交互式公式视图</em> 中查看特定值。</li>
  		<li>CNN 传达了一个 <strong>微分函数</strong>，该函数在输出层的可视化中表示为 <strong>类分数</strong>。</li>
  	</ol> 
  	<p>
  		如果您以前学习过神经网络，那么您可能会觉得这些术语很熟悉。那么，什么使CNN与众不同？CNN利用一种特殊类型的层，恰当地称为卷积层，使它们处于适当的位置以从图像和类似图像的数据中学习。关于图像数据，CNN可以用于许多不同的计算机视觉任务，例如 <a href="http://ijcsit.com/docs/Volume%207/vol7issue5/ijcsit20160705014.pdf" title="CNN Applications">图像处理，分类，分割和对象检测</a>。
  	</p>  
  	<p>
  		在CNN Explainer中，您可以看到如何使用简单的CNN进行图像分类。由于网络的简单性，它的性能并不理想，但这没关系！CNN Explainer中使用的网络架构， <a href="http://cs231n.stanford.edu/" title="Tiny VGG Net presented by Stanford's CS231n">Tiny VGG</a>包含了当今最先进的CNN所使用的许多相同的层和操作，但是规模较小。这样，将更容易理解入门。
      </p>     

      <h2><strong>这个网络的每一层做什么？</strong></h2>
      <p>
      让我们遍历网络中的每一层。在阅读时，单击并悬停在上面的可视化中，即可与上面的可视化进行交互。
      </p>
      <h4 id='article-input'> <strong>输入层</strong></h4>
      <p>
      输入层（最左边的层）代表输入到CNN中的图像。因为我们使用RGB图像作为输入，所以输入层具有三个通道，分别对应于该层中显示的红色，绿色和蓝色通道。单击 <img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/network_details.png" alt="network details icon"/> 上方的图标以显示详细信息（在此层以及其他层上）。
      </p>
      <h4 id='article-convolution'> <strong>卷积层</strong></h4>
      <p>
      卷积层是CNN的基础，因为它们包含学习的内核（权重），这些内核提取出可将不同图像彼此区分开的特征-这就是我们想要的分类！与卷积层交互时，您会注意到先前各层与卷积层之间的链接。每个链接代表一个唯一的内核，该内核用于卷积操作以生成当前卷积神经元的输出或激活图。
      </p>
  	<p>
  		卷积神经元执行具有唯一内核和上一层相应神经元输出的元素点积。这将产生与唯一内核一样多的中间结果。卷积神经元是所有中间结果的总和与学习偏差的总和。
  	</p>
  	<p>
      例如，让我们看一下上面的Tiny VGG架构中的第一个卷积层。请注意，此层中有10个神经元，但上一层中只有3个神经元。在Tiny VGG架构中，卷积层是完全连接的，这意味着每个神经元都与上一层中的每个其他神经元相连。着眼于第一卷积层最顶部的卷积神经元的输出，当我们将鼠标悬停在激活图上时，我们将看到有3个唯一的内核。
  	</p>
    <div class="figure">
      <img src="PUBLIC_URL/assets/figures/convlayer_overview_demo.gif" alt="clicking on topmost first conv. layer activation map" width=60% height=60% align="middle"/>
      <div class="figure-caption">
        图1: 将鼠标悬停在第一个卷积层上最顶层节点的激活图上时，您会看到已应用3个内核来生成此激活图。单击此激活图后，您可以看到每个唯一内核发生的卷积操作。
      </div>
    </div>

  	<p>
      这些内核的大小是由网络体系结构的设计人员指定的超参数。为了产生卷积神经元（激活图）的输出，我们必须与上一层的输出以及网络学习到的唯一内核一起执行元素逐点积。在TinyVGG中，点积运算使用的跨度为1，这意味着内核将每个点积移出1个像素，但这是网络架构设计人员可以调整以使其更适合其数据集的超参数。我们必须对所有3个内核执行此操作，这将产生3个中间结果。
  	</p>
    <div class="figure">
      <img src="PUBLIC_URL/assets/figures/convlayer_detailedview_demo.gif" alt="clicking on topmost first conv. layer activation map" />
      <div class="figure-caption">
        图2:内核用于生成上述激活图的最高中间结果。
      </div>
    </div>
  	<p>
  		然后，执行元素和，包含所有3个中间结果以及网络已获悉的偏差。此后，生成的二维张量将是在第一卷积层中最顶层神经元的上方界面上可见的激活图。必须应用相同的操作来生成每个神经元的激活图。
  	</p>
  	<p>
      通过一些简单的数学运算，我们可以推断出在第一个卷积层中应用了3 x 10 = 30个唯一的内核，每个内核的大小为3x3。卷积层与上一层之间的连通性是构建网络体系结构时的设计决策，它将影响每个卷积层的内核数量。单击可视化文件以更好地了解卷积层背后的操作。看看你是否可以理解上面的示例！
    </p>
    
    <h4><strong>激活层函数</strong></h4>
    <h6 id='article-relu'><strong>ReLU函数</strong></h6>
    <p>
      神经网络在现代技术中极为盛行-因为它们是如此的精确！当今性能最高的CNN包含大量荒谬的图层，可以学习越来越多的功能。这些突破性的CNN能够实现如此 <a href="https://arxiv.org/pdf/1512.03385.pdf" title="ResNet">强大的准确性</a> 是因为它们的非线性。  ReLU将急需的非线性应用到模型中。非线性是产生非线性决策边界所必需的，因此输出不能写成输入的线性组合。与其他非线性函数（例如Sigmoid）相反，ReLU激活函数专门用作非线性激活函数，因为从经验上已经观察到，使用ReLU的CNN的训练速度要快于它们的对应函数。
    </p>
    <p>
      ReLU激活函数是一对一的数学运算
    </p>
    <div class="figure">
    <img src="PUBLIC_URL/assets/figures/relu_graph.svg" alt="relu graph" width="30%" height="30%"/>
      <div class="figure-caption">
        图3: 绘制的ReLU激活函数，该函数忽略了所有负面数据。
      </div>
    </div>
    
    <h6 id='article-softmax'><strong>Softmax函数</strong></h6>
    <p>
      softmax操作的主要作用是：确保CNN输出总和为1。因此，softmax操作可用于将模型输出缩放为概率。单击最后一层将显示网络中的softmax操作。通过softmax函数后，每个类现在对应一个适当的概率！
    </p>
    <div class="figure">
    <img src="PUBLIC_URL/assets/figures/softmax_animation.gif" alt="softmax interactive formula view"/>
      <div class="figure-caption">
        图4：Softmax交互式公式视图允许用户与颜色编码的logit和公式进行交互，以了解在将扁平层之后的预测分数如何归一化以产生分类分数。</div>
    </div>
    <h4 id='article-pooling'><strong>池化层</strong></h4>
    <p>
      在不同的CNN架构中，池化层的类型很多，但是它们的目的都是要逐渐减小网络的空间范围，从而减少网络的参数和总体计算。上面的Tiny VGG架构中使用的池类型为Max-Pooling。
    </p>
    <p>
      最大池操作需要在体系结构设计期间选择内核大小和步幅长度。选择后，该操作将以指定的步幅在输入上滑动内核，同时仅从输入中选择每个内核切片上的最大值以产生输出值。通过单击上方网络中的合并神经元，可以查看此过程。
    </p>
    <p>
      在上面的Tiny VGG体系结构中，池化层使用的是2x2内核，步幅为2。使用这些规范进行此操作将导致75％的激活被丢弃。通过丢弃这么多值，Tiny VGG的计算效率更高，并且避免了过拟合。
    </p>
    <h4 id='article-flatten'><strong>展平层</strong></h4>
    <p>      
      该层将网络中的三维层转换为一维向量，以适合全连接层的输入以进行分类。例如，将一个5x5x2张量转换为大小为50的向量。网络的先前卷积层从输入图像中提取了特征，但是现在该对这些特征进行分类了。我们使用softmax函数对这些功能进行分类，这需要一维输入。这就是为什么需要平坦层的原因。可以通过单击任何输出类来查看此层。
    </p>

    <h2><strong>互动功能</strong></h2>
    <ol>
    	<li><strong>上传自己的图像：</strong> 通过选择 <img class="icon is-rounded" src="PUBLIC_URL/assets/figures/upload_image_icon.png" alt="upload image icon"/> 了解您的图像如何分为10类。通过分析整个网络中的神经元，您可以了解激活图和提取的特征。</li>
    	<li><strong>更改激活图色标：</strong> to better understand the impact of activations at different levels of abstraction by adjusting <img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/heatmap_scale.png" alt="heatmap"/>.</li>
    	<li><strong>了解网络详细信息：</strong> 单击 <img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/network_details.png" alt="network details icon"/> 图标，来进一步了解图层尺寸和色标。 </li>
    	<li><strong>模拟网络操作：</strong> 通过点击 <img class="icon is-rounded" src="PUBLIC_URL/assets/figures/play_button.png" alt="play icon"/> 按钮，或通过将鼠标悬停在输入或输出的各个部分上 <em>交互式公式窗口</em> 来了解映射和基础操作</li>
      <li><strong>了解图层功能：</strong> 通过单击 <img class="icon is-rounded" src="PUBLIC_URL/assets/figures/info_button.png" alt="info icon"/> 按钮，并从 <em>交互式公式窗口</em> 了解层的详细信息。</li>
    </ol> 

  </div>
</body>