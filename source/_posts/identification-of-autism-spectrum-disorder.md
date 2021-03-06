---
title: 【2018】Identification of autism spectrum disorder using deep learning and the ABIDE dataset
mathjax: true
date: 2020-06-25 20:04:51
updated: {{ date }}
tags: [MRI, 深度学习]
categories: [ASD, 图巻积网络]
---

# 摘要

本研究的目的是仅基于患者的大脑激活模式，应用深度学习算法从大型脑成像数据集中识别自闭症谱系障碍（ASD）患者。作者从一个由多个站点数据集组成名叫ABIDE的世界数据集调查ASD患者的大脑图像数据。ASD是一种以社会缺陷和重复行为为特征的大脑疾病。根据最近疾病控制中心的数据，美国每68名儿童中就有一名患有自闭症。作者研究了从功能性脑成像数据中客观识别ASD参与者的功能连接性模式，并试图揭示从分类中产生的神经模式。研究结果提高了最新水平，在数据集中，ASD患者与对照组患者的识别准确率达到70%。从分类中得出的模式显示了大脑前区和后区之间的脑功能的反相关性；该反相关性证实了目前的经验证据，即ASD中大脑连接性的前后位体被破坏。根据作者的深度学习模型，展示了结果并确定大脑中最有助于区分自闭症和典型发育控制的区域。

# 简介

精神病学神经影像学研究的主要目标是识别客观的生物标记物，这些生物标记物可能为脑基疾病的诊断和治疗提供信息。数据密集型的机器学习方法是研究大脑功能模式在更大、更异构的数据集上的可复制性的一个有希望的工具。本研究的第一个目标是利用静息状态功能磁共振成像（rs-fMRI）数据，根据自闭症谱系障碍（ASD）患者和对照组患者各自的神经功能连接模式对其进行分类。作者使用了一种结合有监督和无监督机器学习（ML）方法的深度学习方法。该方法应用于大样本脑成像数据（ABIDE I）。第二个目标是研究与ASD相关的神经模式，这对ASD的分类贡献最大；这些结果是根据大脑中区分ASD与对照组的区域网络以及之前对ASD大脑功能的研究来解释的。

ASD与一系列的表型相关，这些表型在社会、交流和感觉运动缺陷的严重程度上有所不同。ASD诊断工具是评估特征社会行为和语言技能（见作者关于真实世界分类准确性的结果）。然而，神经科学的研究有助于弥合自闭症行为改变谱的复杂性与其神经模式之间的鸿沟。无创性脑成像研究已经促进了对基于大脑疾病的神经基础及其相关行为的理解，如ASD及其社会和交流缺陷。ASD激活模式的识别以及这些模式与神经和心理成分的关联有助于理解精神障碍的病因。

脑部疾病的脑部影像学研究面临的挑战之一，是在更大、更具人口统计学异质性的数据集中找到反映临床人群异质性的发现。近年来，ML算法被应用于脑成像数据中，以提取可复制的脑功能模式。这些算法可以从精神病患者的脑成像数据中提取可复制的、健壮的神经模式。

# 机器学习与疾病状态预测：了解大脑和精神疾病的下一个前沿

机器学习方法与脑成像数据的结合使得人们能够对与语义类别、名词意义、情感和学习的表现相关的精神状态进行分类。在精神疾病的案例中，研究已经可以根据大脑活动确定精神分裂症、自闭症和抑郁症的患者。将ML算法应用于ASD脑成像数据的研究已可以从fMRI脑部活动状态来区分自闭症或对照组，并且单个部位的准确率高达97％。他们还可以标记与心理因素（自我表现）相关的大脑激活模式。该模式存在于对照患者中，而自闭症参与者几乎不存在。在另一项关于ASD患者的分类研究中，作者在178名ASD和与其智商匹配的参与者的样本中获得了76.67%的分类准确性。

将监督学习用于脑成像数据的研究的风险是其参与者较少。Arbabshirani等人的研究表明，ML研究的可靠分类准确度是在少于100名参与者上获得较高的分类准确度，即90%以上。在较大的人口样本和来自不同地点的数据的情况下，分类准确性显着下降。

大多数将脑成像和机器学习相结合的研究都采用了监督学习方法，如支持向量机（SVM）或高斯朴素贝叶斯（GNB）分类器。监督机器学习方法的特征选择过程的主观性可能是比较不同研究结果的障碍。在监督学习中，类标签被分配给一组用作训练数据集的数据；其他数据点（测试数据集）根据在训练数据中发现的模式（使用给定标签）进行分类。在监督方法中，类标签被分配给一组用作训练数据集的数据；其他数据点（测试数据集）根据在训练数据中发现的模式（使用给定标签）进行分类。换句话说，该算法对预先建立的标签进行分类（即，它们依赖于特征选择或特征工程）。这些标签和特征的选择取决于先验假设或探索的过程，因此它们就取决于主观的水平。例如，用于脑成像数据分类的体素数目是在100、200、400个及更多体素集合的基础上根据经验选择最适合分类的集合。在本研究中，作者通过使用大数据集和无监督机器学习方法对一种精神疾病进行分类来解决可泛化性和主观性的问题。减少特征提取中的主观性，可以提供一个新的了解大脑功能的窗口，从而减少实验的依赖性和更多的采用数据驱动。

# ABIDE数据集的类别

ADIDE数据集以前被Nielsen等人基于大脑连通性测量对自闭症和对照组进行分类。对于964位受试者，计算了由至少由5mm的种子体素间隔形成的非重叠灰质ROI（SPM8遮罩grey.nii）的BOLD信号。在这个ROI中包含了特定ROI种子体素的欧氏体素。Nielsen等人根据生成的7266个ROI数据计算了每个ROI之间的成对相关性，计算得到了大小为7266×7266的连接矩阵。采用留一法法对各组（ASD和对照组）进行线性模型拟合，将连接性矩阵与受试者相关变量（年龄、性别和惯用手）联系起来。每个连接的值都是根据变量估计得到，然后使用某个站点的连接平均值和另一个站点的相同连接平均值之间的差异来调整。此过程缓解了因站点之间的差异导致结果偏差，例如不同的扫描仪以及扫描参数和协议的变化。

作者试图适配ABIDE数据集中的差异和多站点数据。对于被忽略的受试者，从孤独症模型和对照模型得到的估计值中减去每个连接的实际值。计算所有7266个ROI的减法平均值，并将ROI的平均值相加。将正值分类为ASD，将负值分类为对照，Nielsen等人的分类分类准确率高达60％。最近，Abraham等人达到了本论文的分类最高分。通过构建参与者特定功能连接矩阵（connectomes），作者在ABIDE数据集中获得了67%的准确性。在本研究中，作者的目的是提高所获得的最高精度。

# 神经图像与深度学习算法

Koyamada等人使用深度神经网络（DNN）从可测量的大脑活动中获取大脑状态。他们训练了一个具有两个隐藏层和一个Softmax输出层的人工神经网络，将499名受试者的fMRI数据基于分类任务分为七类，分别是情感、赌博、语言、运动、关系、社交和工作记忆。与线性回归和支持向量机等有监督学习方法（平均精度47.97%）相比，Deep模型可以获得更好的结果（平均精度50.74%）。Plis等人使用深度学习和T1加权结构图像，以及来自四个不同地点的数据，将精神分裂症患者与匹配的健康对照组进行分类；作者还使用PREDICT-HD项目的数据，将亨廷顿病患者与健康对照组进行分类。首先，他们试图从约翰霍普金斯大学（JHU）、马里兰精神病研究中心（MPRC）、英国伦敦精神病研究所（IOP）和匹兹堡大学西方精神病研究所和诊所（WPIC）进行的四项不同研究中，对198名精神分裂症患者和191名对照者进行分类。Plis等人训练了3层深度信念网络（第一层50个隐藏单元，第二层50个隐藏单元，顶层100个隐藏单元）。他们使用从三个数据库中提取的特征实现了90%的分类精度，相比之下，使用支持向量机对原始数据实现了68%的分类精度。作者认为，深度学习在临床脑成像应用中具有巨大的潜力。

研究的第二个部分是使用来自PREDICT-HD项目的健康对照组和亨廷顿病患的者数据。这项研究旨在利用深度学习技术识别疾病，并评估疾病的严重程度（低、中、高）。这项研究使用了来自不同国家的32个地点的T1加权结构扫描数据集。该数据集包括2641张患者图片和859张健康对照图片。作者使用了一个三层深度的深度信任网络（DBM）：第一层为50个隐藏单元，第二层为50个隐藏单元，顶层为100个隐藏单元。采用T分布随机邻域嵌入（t-SNE）技术将所得数据还原为二维形式，结果显示患者和对照组之间存在线性可分离投影。

深度学习算法使脑成像数据的分类比严格监督的方法更进一步。算法在学习模型中使用复杂的数据表示。深度学习算法采用无监督学习方法，依靠最小的人工干预来提取相关特征。使用无监督方法对临床人群进行分类，可以探索性地搜索精神疾病的神经模式，这种模式对特征选择的假设依赖性较小；因此，它可能不易受到类别错误的影响。在监督方法中，假定的标签被用来训练分类器，并发现与标签相关的大脑激活或连接模式（例如，临床人群和对照人群样本）。在无监督的方法中，分类器探索群体样本中可能与临床群体相关的大脑模式；再次，避免了标签选择的主观性。研究表明，较低的主观性和可能更自由的深度学习算法有望应用于多站点的大数据集机器学习。

# 组件和方法

## 参与者

本研究使用ABIDE I的rs-fMRI资料进行研究。ABIDE是一个集合数据集，它提供了以前收集的ASD的rs-fMRI和匹配的对照组数据，以便在科学界进行数据分享的目的。作者使用了505名ASD患者和530名匹配对照（典型对照，TC）的数据。ABIDE数据集共收集17个不同的成像点的，包括每个患者的rs-fMRI图像、T1结构脑图像和表型信息，如表1所示。表1包含关键的表型信息，如按性别和年龄划分的ASD和TC分布，ASD受试者的ADOS评分，以及平均框架位移（FD）质量测量。

表1：表型数据汇总

{% asset_img phenotype-summary.png 表型数据汇总 %}

## 静止状态与特征选择

静息状态fMRI提供大脑各区域之间功能关系的神经测量。Rs-fMRI数据对临床人群的调查尤其有用。它允许在不增加与任务相关的大脑激活相关的变化复杂性的情况下，研究大脑网络的破坏。它可用于精神状态、记忆、事件的回忆和临床人群等的调查。rs-fMRI已经被证明是高度可重复的，并且提供了可以很容易地在研究中进行比较的数据集。静息状态fMRI低频波动的相关性来源于血氧或血流的波动。它是大脑功能连接的一种表现。为了研究大脑连接性，作者计算了感兴趣区域时间序列的平均值得到了其相关性，并且相关性用于建立连接矩阵。

## 数据预处理

已经预处理的rs-fMRI数据可以从Preprocessed Connectomes Project下载。数据使用C-PAC预处理管道进行处理，对fMRI数据进行两人切片时间校正、运动校正和体素强度归一化。用24个运动参数、5个分量的CompCor、低频漂移（线性和二次趋势）和全局信号作为回归函数进行干扰信号去除。对功能数据进行带通滤波（0.01–0.1Hz），并使用非线性方法对模板空间（MNI152）进行空间注册。

为每个受试者提取感兴趣区域的平均时间序列。使用CC200大脑功能分区图集来减少特征向量的大小（见下文）。这幅图集是由数据驱动的，将整个大脑分割成空间上相似的功能活动区域，共200个区域。

## 特征选择：ROIs的功能连通性

功能连接性被用于将受试者分为ASD和TC。功能连接性提供了基于rs-fMRI脑成像数据的时间序列的一个脑区激活水平的指标。连通矩阵中的每个单元都包含一个皮尔逊相关系数。该系数是大脑两个区域之间相关性的指数，其范围为[-1, 1]，接近1的值表示时间序列高度相关，接近-1的值表示时间序列反相关。为了使用相关矩阵中的值作为特征，删除了上三角值。这些值重复下三角形的值。作者还去掉了矩阵的主对角线，因为它表示一个与自身相关的区域。

然后，作者将剩下的三角形（即将其折叠成一维向量）展平以检索特征向量，目的是将其用于主题分类。结果特征的数量由以下等式定义：

$$S=\frac{(N-1) N}{2}$$

其中$N$表示相关体素或区域的数目。使用CC200感兴趣区图谱产生19900个特征。

## 分类方法

去噪自编码用于训练预测模型，以获得更好的泛化效果，即在参与者初始池之外对新对象进行准确分类。去噪自动编码器基于损坏的输入来重建输入：在训练模型之前，随机地从函数连接矩阵导出的向量中的某些位置设置为零。应用于损坏数据的损坏模块是基于二项分布。该技术的目标是使模型足够精确，以便使用新的数据进行预测。

在本研究中，作者使用两个堆叠式去噪自编码器，在无监督的预训练阶段，从ABIDE数据集中提取低维数据。作者使用重建损失（均方误差）实现了验证集的最佳优化；在交叉验证k-fold模式中使用了以下配置。输入和输出层有19900个特性完连接到隐藏层的1000个单位的瓶颈。第一个自动编码器的数据损坏概率设置为20%（对于二项分布：$n=1$，$p=0.8$）。第二个自动编码器将前一个自动编码器输出的1000个输入通过600个单位的隐藏层的输出。第二个自动编码器的数据损坏概率为30%（对于二项分布：$n=1$，$p=0.7$）。

自动编码器的无监督训练是逐层进行。为了利用自动编码器所提取的知识，作者将编码器的权重应用于配置为19900、1000、600、2的多层感知器（MLP）。换句话说，MLP假设输入空间为19900个特征，输出空间为2个数字，如下所述。在输入层和输出层之间，网络有两个隐藏层，分别有1000和600个单元。这个过程如图1、图2所示，蓝色和绿色权重包含无监督训练的编码器；图2包含有监督训练的多层感知器，该感知器使用来自自编码器训练的先验知识。MLP包含基于自动编码器编码后的权重；因此，其监督训练称为微调。微调的目的是调整MLP权值以输出期望的类，并最小化有监督任务的预测误差。输出层包含两个输出单元：每个单元表示来自ASD或TC对象的输入概率。这种类型的输出称为one-hot编码：在微调过程中，只有一个输出的激活值为1（其他为0）；输出是通过使用Softmax函数获得的。Softmax函数规范化了输出分布，因此输出表示一个类的互补概率（即ASD和TC的概率之和为1；例如，ASD的输出概率为80%，那么TC的输出概率为20%）。

{% asset_img two-autoencoders-structure.png 两种自编吗器结构 %}

图1：两种自编吗器结构。为了简化结构的可视化，作者减少了单元的数量。（a）19900-1000-19900；（b）1000-600-1000。（为了解释这个图中对颜色的引用，读者可以参考本文的web版本。）

{% asset_img transfer-learning-from-autoencoders.png 两种自编吗器结构 %}

图2：从自动编码器AE1和AE2转移到神经网络分类器。（为了解释这个图中对颜色的引用，读者可以参考本文的web版本。）

## 分类评价

为了评估深度学习的结果，将模型的性能与SVM和RF训练的分类器性能进行了比较。所有模型的评估都基于一个10折的交叉验证模式，该模式混合了来自所有17个站点的数据，同时保持不同站点之间的比例。数据降维是通过使用预先训练的无监督过程的编码器层来实现的。表2总结了作者在下面描述的结果。

作者报告所有分类器的准确性、敏感性和特异性，以及训练每个模型所需的总时间。

表2：对整个数据集在的深度神经网络（DNN）、随机森林（RF）和支持向量机（SVM）分类器的10折交叉验证训练上的比较。

{% asset_img classifier-evaluation.png 不同算法的分类结果 %}

# 结果和讨论

深度神经网络在交叉验证中的平均分类准确率为70%（敏感度74%，特异度63%），在单个折叠中的准确率范围为66%到71%。根据文献，这是迄今为止最高的分类。SVM分类器的平均准确率为65%（62%～72%，敏感度68%，特异度62%）；随机森林分类器的平均准确率为63%（敏感度69%，特异度58%）。结果表明，深度学习算法在多站点ABIDE数据中将ASD和典型参与者分类为高于机会。结果还表明，该算法的性能优于其他用于比较的有监督方法。尽管使用了专用的GPU来加速训练，但训练模型在保持精度的情况下在训练时间上付出了巨大的代价。整个模型的训练时间超过32小时，使用两个Intel Xeon E5-2620处理器，24个内核，运行频率为2GHz和48GB的RAM，1个Tesla K40 GPU，2880个CUDA内核和12GB的RAM。

结果表明，该算法应用于ABIDE多站点静态脑激活数据的孤独症谱系障碍患者识别效果优于以往的研究结果。补充材料中显示了使用其他脑分区的结果。通过使用自动编码器降低数据维数，SVM分类结果没有变化。作者将SVM应用于使用自动编码器学习的降维上，而不需要微调过程。降维降低了SVM分类结果（使用第一个自动编码器进行数据转换的精度为61%，使用第一个和第二个自动编码器进行数据转换的精度为63%）。降维后的模型可能过于复杂，无法用SVM和自动编码技术来识别数据集中的ASD和TC参与者。与SVM相比，深度学习分类方法的分类精度平均提高了5%。深度学习方法也显示，与之前尝试使用ABIDE多站点数据分类ASD的研究相比，分类精度提高了10%。

与试图用较小样本分类ASD的研究相比，本分类失去了特异性和敏感性。研究表明，分类准确率达到80%以上，甚至90%以上。为了评估作者的模型在真实临床中的实际表现，作者计算了两个指标：正预测值和负预测值（分别为PPV和NPV）。这些度量提供了模型泛化能力的评估。计算是基于敏感性、特异性和ASD患病率之间的关系。

该模型的PPV为4.3%，NPV为99%。根据疾病控制和预防中心（CDC）2014年的监测估计，考虑到美国ASD的患病率为2.24%，计算了PPV和NPV。高NPV是意料之中的，因为大多数人不是自闭症患者。PPV强调了机器学习方法在脑成像数据中的应用不是由诊断目的驱动的。相反，它是一种数据驱动的方法，以告知与疾病相关的神经模式最有可能是什么。

如果较少的站点变化或数据集中没有这样的变化则有利于分类精度；但是一旦使用监督方法出现了跨多个站点分类的挑战，例如ABIDE，那么就会精度下降。不同数据集之间维度的增加是脑成像ML研究面临的挑战。这些维度可能代表了增加临床相关信息以理解精神障碍的可变性，例如来自不同人口统计学的信息。

ABIDE数据集包含了影响站点之间数据一致性的变化。深度学习方法包含了这些变化，并且比浅层方法产生更好的结果。分类的改进可以解释为：自动编码器能够处理原始数据中复杂结构中的潜在因素；神经网络能够对数据中的变化进行编码以指导分类过程。研究结果表明，深度学习算法比SVM等更能处理多站点数据和大的脑成像数据集的复杂性。

为了进一步评估结果，作者对每种分类方法进行了Wilcoxon符号秩检验。具体来说，作者将每种分类方法的标签与基本事实进行了比较。对于SVM分类器，结果显示有统计学意义的标记（$Z=12.08$，$p<0.001$）。RF显示分类准确度略有提高（$Z=2.33$，$p=0.020$），但标签之间的统计差异仍然显著。当使用DNN分类器时，标签之间没有统计学上的显著差异（$Z=0.49$，$p=0.624$）。DNN是唯一一种在分类标签和基本事实之间没有统计学差异de 分类方法。

在本研究中获得了70%的准确度。到目前为止的文献表明，有监督的方法可以有效地对小样本中的高维空间进行分类；深度神经网络允许学习器表示更复杂的函数，特别是与自动编码器一起使用时。这些网络有效地降低了特征维数空间非常大的问题。然而，通过使用相同超参数和5折格式的站点内数据来训练作者的模型获得了52%的平均精度。ABIDE中的可用数据量有益于模型的泛化性，即站点可变性有助于避免跨站点过度拟合。

## 站点留一法分类

为了评估跨站点的分类器性能，我们使用了站点留一法分类的交叉验证，即从训练过程中排除一个站点的数据，并将该数据用作评估模型的测试集。其基本原理是测试模型对新的、不同地点的适用性。这些进一步分析的结果见表3。

表3：使用DNN算法的站点留一法分类结果

{% asset_img leave-site-out.png 站点留一法分类结果 %}

SVM和RF的结果分别见补充材料中的表1和表2

有五个网站的准确率明显低于全球结果：SBL、MAX_MUN、STANFORD、CALTECH和OHSU。结果表明，这些站点的数据具有其他站点不存在的可变性。将准确度得分与头部运动质量指标进行比较，没有发现头部运动对分类准确度的影响。

该分类将一个站点排除在外，测试了全局模型在不丧失训练数据特殊性的情况下合并测试数据和特定站点变化的能力。

# 神经模式：自闭症大脑的连通性

大脑区域的rs-fMRI数据之间的相关性研究结果表明，ASD的rs-fMRI数据中存在两组不同的区域，即欠连接（负相关）和高度连接（正相关）：（1）在rs-fMRI过程中激活呈负相关的前、后脑区的分布网络；（2）在rs-fMRI中激活的区域的后部网络高度相关。这些结果的假设性解释与现有的数据驱动的孤独症前后大脑欠连接理论有关。

ASD受试者大脑中显示出最高反相关的区域是：Paracingulate Gyrus（图3a）、Supramarginal Gyrus（图3b）和Middle Temporal Gyru（图3c）。这些区域的反相关模式是深度学习分类的最相关特征。表4总结了抗相关区域。图4显示了ASD受试者大脑中相关度最高的区域。相关度最高的区域都在大脑的后部：Occipital Pole（图4a）和Lateral Occipital Cortex上部（图4b）。这些区域的相关模式是反相关区域之后与深度学习分类最相关的特征。表5总结了相关领域。

{% asset_img anti-correlated-areas-for-asd-subjects.png ASD受试者的反相关（欠连接）区域 %}

图3：ASD受试者的反相关（欠连接）区域

{% asset_img highly-correlated-areas-for-asd-subjects.png ASD受试者的高度相关（相关）区域 %}

图4：ASD受试者的高度相关（相关）区域

表4：大脑中的反相关区域

{% asset_img anti-correlated-areas.png 大脑中的反相关区域 %}

表5：大脑的相关区域

{% asset_img correlated-areas.png 大脑的相关区域 %}

在ASD患者的任务相关研究和rs-fMRI研究中显示了连接的前后中断（激活时间序列之间的相关性）。孤独症患者的脑功能特征在以往的研究中重复出现，即前后连接减弱，与对照组大脑中的连接相比，后部区域之间的局部连接增强。这些研究是大脑成像数据驱动的自闭症欠连接理论的基础。前后过连接理论也与脑结构指标有关，更具体地说，与胼胝体形态计量学有关。

先前对ASD脑功能的研究表明，ASD患者的前后脑连接中断，同时后连接或局部连接增加。本研究的结果表明，前（paracingulate gyrus）和更多后区（supramarginal gyrus）以及额叶颞区（如middle temporal和inferior frontal、fusiform gyrus和orbital cortex）的功能存在反相关（说明见表4）。作者提出的解释是，这种反相关性反映了ASD大脑前部和后部之间的欠连接性，而正是这种欠连接性对目前的分类贡献最大。前后欠连接的特点支持自闭症的大脑功能，并有助于区分这两组。作者认为前后侧抗相关结果证实了其他研究中所描述的非典型ASD脑功能。

作者根据每个站点提供的表型数据计算网络与ADOS（孤独症诊断观察计划）结果的相关性，以确定ASD连接模式。ADOS是一种自闭症的评估工具。它提供了一系列与自闭症诊断相关的社交和沟通任务。ABIDE项目在没有事先协调的情况下汇编了17个地点的数据。因此，ADOS数据是ABIDE I中的351名受试者。分析表明，表4和表5中的网络与ADOS得分无关。

总之，研究结果表明，深度学习方法可以可靠地对大型多站点数据集进行分类。与单站点数据集相比，跨多个站点的分类必须适应受试者、扫描程序和设备带来的额外差异。这种变化给大脑成像数据增加了噪声，这就挑战了从大脑活动中提取信号的能力，这种信号可以对疾病状态进行分类；然而，尽管不同的设备和人口统计学产生了这样的噪声，但取得了可靠的分类精度，这表明机器学习应用于临床数据集，以及机器学习在帮助识别精神障碍方面的未来应用是有希望的。普利特等人指出到目前为止，利用静息状态fMRI数据对ASD分类的总体评价尚不符合生物标志物标准，这一障碍在本研究中尚未克服。然而，作者建议朝着取得更可靠结果的方向迈出一步。

# [原文](http://repositorio.pucrs.br/dspace/bitstream/10923/10759/2/Identification_of_autism_spectrum_disorder_using_deep_learning_and_the_ABIDE_dataset.pdf)，[在线原文](https://www.sciencedirect.com/science/article/pii/S2213158217302073?via%3Dihub)

