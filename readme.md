 
使用新兴的机器学习技术，使用户体验设计得到提升，用户的行为产生大量的数据，这些数据通过一些复杂的算法和机器学习反馈于界面而再次通过用户对界面的行为重新获得数据这是一个正反馈的循环。
 
举个例子，网易云音乐用户的每日歌曲推荐是网易云的数据引擎驱动的，每天为每个用户个性化定制30首歌曲，算法在https://www.zhihu.com/question/26743347这个知乎页面上有详细的解释，答者的对算法的理解大概分为两类：
1.	以人为本，寻找与你相似的用户，看看他们听什么类型的音乐，这类算法最经典的实现就是“多维空间中两个向量夹角的余弦公式”。
2.	以物为本，构建商品之间的相似度关系矩阵。这个算法最简单暴力的例子是亚马逊的商品推荐“购买了此商品的用户也购买了XXX”。
此外，在音乐推荐中还有一些更复杂的算法，在其他用户创建的歌单中寻找目标用户喜欢的歌曲，然后歌单李其他的歌曲，也就是上面两类算法的杂交“以物和以人”为本当然，算法不可以盲目地推荐，这样会导致推荐“不准”的情况，这样就要对数据进行加权运算，对喜欢歌曲的程度进行一定的量化(比如： 单曲循环=5, 分享=4, 收藏=3, 主动播放=2 , 听完=1, 跳过=-1 , 拉黑=-5 )。那么，这种算法在实际用户体验的设计中会有什么用呢？或者说如何将用户体验设计与数据和算法紧密地结合起来？
我在（https://www.sumaarts.com/share/636.html）人工智能ai机器学习时代的新型用户体验设计 这篇文章中找到了答案，作者在机器学习和用户体验设计的研究当中得出了6个设计的目标分别为：1. 为探索而设计 2. 为决策而设计 3. 为不确定性而设计 4. 为参与而设计 5. 为时间高效利用而设计 6. 为内心平和而设计。

当我回过头来分析雅思听力与ai学习之间的关系的时候，发现ai学习对于雅思听力的用处大概在1.为探索而设计 2.为决策而设计 4.为参与而设计和 5.为时间高效利用而设计这五点内容。
由于学习类app对于用户而言是一个学习工具，不存在喜欢与否，所以采用上述设计类型中的<为决策而设计>下面是我针对这些设计目标所做的ai雅思原型设计


AI算法的思想是这样：每个用户（user）都有自己的雅思薄弱点，比如A对于某些类型的雅思听力如科技介绍、 校园对话、 邀请电话存在弱点则对应推荐类似的听力练习，也就是用元素去连接用户和雅思听力。每个人对不同的元素偏好不同，而每个听力包含的元素也不一样。我们希望能找到这样两个矩阵：
一，	用户-潜在因子矩阵Q，表示不同的用户对于不用元素的偏好程度，1代表很困难，0代表简单。比如下面这样：
        类型
用户	科技英语	校园英语	独白	对话
用户A	0.3	0.7	0.2	0.5
用户B	0.1	0.5	0.2	0.3
……				

二，	听力-潜在因子矩阵P，表示每种听力含有各种元素的成分：
        类型
听力	科技英语	校园英语	独白	对话
听力1	0.7	0.2	1	0
听力2	0.5	0.3	0	1
……				
	利用这两个矩阵，我们能得出用户A对听力1的掌握程度是：用户A对科技英语的掌握程度*听力1含有科技英语的成分+用户A对校园英语的掌握程度*听力1含有校园英语的成分+……
即0.3*0.7 + 0.7*0.2 + 0.2*1 + 0.5*0 = 0.55
每个用户对每段英语听力都这样计算可以得到不同用户对不同英语听力的掌握程度矩阵：
用户
听力	用户A	用户B
听力1	0.55	0.42
听力2	0.86	0.70
当然，类似于“科技英语”，“校园英语” 这类的因子我们不能靠直觉去猜，而这些因子可以通过特征值分解（eigendecomposition）与奇异值分解（Singular value decomposition，SVD）等矩阵算法对用户-听力表进行预测，当用户听力的数量达到一定程度时就可以根据得出的表来预测从而推荐出听力文章。（考虑到篇幅，这里就不过多介绍矩阵算法了，从https://zhuanlan.zhihu.com/p/34497989这个文章中可以了解到详细的算法内容）。

在原型设计的主页面中，有每日一句和用户学习进度分析：
 
每日一句模块通过对用户在app中的行为产生的数据通过算法得出（为探索而设计），下方的用户分析则是由用户设计的目标结合算法得出（为决策而设计）。在进行决策的算法中可以大量参考前面提到的ai算法内容，通过用户对听力掌握的预测值来进行用户学习计划的策划。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/首页.png)
在首页中，展示出用户的信息（名字，个性签名）
之下显示用户最近阅读的文章信息，下方为三个听力模块和一个词汇模块，在这些模块中，用户可以主动选择听什么文章和跳过文章等操作，这样就能通过AI来处理这些数据提供给每日推荐和智能推荐等。注意，用户点击听力是出于对听力内容的喜好程度，前面提到的ai算法是为了决策用户的学习计划。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/课程.png)
在课程页面中提供了一些可供下载的雅思课程，并提供了一些收费的课程，收费课程也是软件运营的一个过程。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/发现.png)
发现页面是对于这个app是一个高度与AI结合的页面，发现页面对用户在课程页面和主页面中进行的一系列操作进行前面提到的AI算法的分析后“动态”推荐数据，这也是我在发现页面中使用一些无意义的图片，文字的原因。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/我的.png)
	最后，在主页面中最后的一个页面是“我的”页面，里面提供了一些用户自定义app的选项和一些app的设置，在这个页面中，用户所作的修改是强烈主观性的，对于推荐算法来说，用户在这个页面中的喜好厌恶是权重相比其他页面都要高得多（类似音乐app中对音乐主管地点红星或者拉黑推荐），权重为1。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/听力页面-全文.png)
在点击从ai算法中推荐出来的文章后，用户页面跳转到该页面（精听页）， 在此页面，算法可以通过用户所阅读这篇文章的时间和循环次数等显性因子来获得用户对当前文章的掌握程度，从而针对用户推荐更有难度的文章。
即：数据——>算法——>数据——>用户——>数据

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/听力页面-单句.png)
在单句精听页面，用户所采取的操作跟精听页面类似，如果推荐文章的形式对用户无效则可以把上面模型（数据——>算法——>数据——>用户——>数据）中的数据的单位改成句子。这样能更精细地推荐出文章。

![image](https://github.com/benny1213/AI-IELTS/blob/master/原型设计/听力页面-收藏.png)
在收藏页面中，显示出的信息于ai决策算法无关但是收藏页面中用户行为产生的数据可以提供给ai喜好算法，来推荐用户更感兴趣的内容。


注：以上原型设计使用Balsamiq Mockups 3 完成，保存在此文件夹下的“Ai雅思.bmpr”，截图保存至“原型设计”文件夹中

参考：

推荐系统中的矩阵分解技术
https://zhuanlan.zhihu.com/p/34497989

网易云音乐的歌单推荐算法是怎样的？
https://www.zhihu.com/question/26743347

人工智能ai机器学习时代的新型用户体验设计
https://www.sumaarts.com/share/636.html

如何为AI产品设计用户体验？
http://www.woshipm.com/ai/1032787.html

具有设计感的 app：一场方寸之间的完美瞬时体验（多图）
http://daily.zhihu.com/story/4501939

Balsamiq Mockups 3 简介
https://wiki.jikexueyuan.com/project/balsamiq-mockups-doc/introduction.html
