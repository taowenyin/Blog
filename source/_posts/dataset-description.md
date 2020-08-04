---
title: 各类数据集说明
mathjax: true
date: 2020-02-18 19:46:01
updated: {{ date }}
top: true
tags:
categories: [数据集]
---

# [UCI](http://archive.ics.uci.edu/ml/datasets.php)

## [SMS Spam Collection Data Set（垃圾短信数据集）](http://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection)

| 特点 | 正例 | 反例 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 文本数据 | 4825 | 747 | 5574 | 分类、聚类 |

## [Internet Advertisements Data Set（网络广告）](http://archive.ics.uci.edu/ml/datasets/Internet+Advertisements)

| 特点 | 正例 | 反例 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 图片数据 | 2821 | 458 | 3279 | 分类 |

## [Iris Data Set（鸢尾花数据集）](http://archive.ics.uci.edu/ml/datasets/Iris)

| 特点 | 类别数 | 总数 | 特征数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 文本数据 | 3 | 150 | 4 | 分类 |

# [kaggle](https://www.kaggle.com/)

## [Sentiment Analysis on Movie Reviews（影评中的情感分析）](https://www.kaggle.com/c/sentiment-analysis-on-movie-reviews/overview)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 文本数据 | 5 | 156060 | 分类、聚类 |

# [LIBSVM Dataset](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/)

## [scene-classification（多标签场景分类）](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/multilabel.html#scene-classification)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 图像数据 | 6 | 2407 | 多标签分类 |

| 编号 | 类别 | 训练图片 | 测试图片 | 合计 |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 海滩（Beach） | 194 | 175 | 369 |
| 1 | 日落（Sunset） | 165 | 199 | 364 |
| 2 | 落叶（Fall foliage） | 184 | 176 | 360 |
| 3 | 田（Field） | 161 | 166 | 327 |
| 0,3 | 海滩+田（Beach+Field） | 0 | 1 | 1 |
| 2,3 | 落叶+田（Fall foliage+Field） | 8 | 16 | 23 |
| 4 | 山（Mountain） | 223 | 182 | 405 |
| 0,4 | 海滩+山（Beach+Mountain） | 21 | 17 | 38 |
| 2,4 | 落叶+山（Fall foliage+Mountain） | 5 | 8 | 13 |
| 3,4 | 田+山（Field+Mountain） | 26 | 49 | 75 |
| 2,3,4 | 田+落叶+山（Field+Fall foliage+Mountain） | 1 | 0 | 1 |
| 5 | 城市（Urban） | 210 | 195 | 405 |
| 0,5 | 海滩+城市（Beach+Urban） | 12 | 7 | 19 |
| 3,5 | 田+城市（Field+Urban） | 1 | 5 | 6 |
| 4,5 | 山+城市（Mountain+Urban） | 1 | 5 | 6 |
|  | 合计 | 1211 | 1196 | 2407 |

# ASD

## [ABIDE](http://fcon_1000.projects.nitrc.org/indi/abide/)

### [ABIDE I的预处理](http://preprocessed-connectomes-project.org/abide/)

ABIDE I数据集包括539名自闭症患者和573名对照组，共计1112名的大脑神经影像数据，该数据包含结构信息和rs-fMRI数据，以及大量的表型数据。

该数据集有4种**功能数据预处理**的方法，分别是连接组计算系统（CCS）、用于分析连接组的可配置管道（CPAC）、re-fMRI数据处理助手（DPARSF）、神经图像分析工具（NIAK）。由于带通滤波和全局信号校准存在争议，因此每种数据预处理方法有四种不同的**预处理策略**，即有带通滤波的全局信号校准（filt_global）、没有带通滤波的全局信号校准（nofilt_global）、有带通滤波但没有全局信号校准（filt_noglobal）和没有带通滤波也没有全局信号校准（nofilt_noglobal）。

该数据集有3种**结构和皮层测量预处理**的方法，分别是ANTS、CIVET和FreeSurfer。

ABIDE的预处理结果可在通过亚马逊S3获得，S3上的数据是以单个文件进行保存，通过传递参与者、预处理方法和预处理策略等参数进行下载。

ABIDEde表型数据可以从[这里](https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Phenotypic_V1_0b_preprocessed1.csv)下载。该文件包含了所有能提供的信息，以及预处理后的额外信息，表型数据字段说明可以从[这里](http://fcon_1000.projects.nitrc.org/indi/abide/ABIDE_LEGEND_V1.02.pdf)下载，额外的信息包括：

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | FILE_ID | 文件ID |  |
| 2 | anat_cnr、anat_efc、anat_fber、anat_fwhm、anat_qi1、anat_snr | PCP质量评估协议中的解剖信息 | [信息](http://preprocessed-connectomes-project.org/abide/quality_assessment.html) |
| 3 | func_efc、func_fber、func_fwhm、func_dvars、func_outlier、func_quality、func_mean_fd、func_num_fd、func_perc_fd、func_gsr | PCP质量评估协议中的功能信息 | [信息](http://preprocessed-connectomes-project.org/abide/quality_assessment.html) |
| 4 | qc_func_rater_2、qc_func_rater_3、qc_func_notes_rater_2、qc_func_notes_rater_3 | 手动质量评估注释 |  |
| 5 | qc_anat_rater_2、qc_anat_rater_3、qc_anat_notes_rater_2、qc_anat_notes_rater_3 | 手动质量评估注释 |  |
| 6 | qc_rater_1、qc_notes_rater_1 | 手动质量评估注释 |  |

> 功能数据文件的下载

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/[pipeline]/[strategy]/[derivative]/[file identifier]_[derivative].[ext]
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | pipeline | 数据预处理方法 | ccs、cpac、dparsf、niak |
| 2 | strategy | 数据预处理策略 | filt_global、filt_noglobal、nofilt_global、nofilt_noglobal |
| 3 | file identifier | 文件ID |  |
| 4 | derivative | 派生类型 | alff、degree_binarize、degree_weighted、dual_regression、igenvector_binarize、eigenvector_weighted、falff、func_mask、func_mean、func_preproc、lfcd、reho、rois_aal、rois_cc200、rois_cc400、rois_dosenbach160、rois_ez、rois_ho、rois_tt、vmhc |
| 5 | ext | 文件扩展名 | 1D | nii.gz |

数据文件的后缀名由派生类型决定。大部分数据文件都已.nii.gz结尾，但是如果要包含ROI时序信息，那么就以.1D结尾，并且派生类型以rois_开头。除了func_preproc和dual_regression，以nii.gz结尾的数据文件的大小大约在256KB至512KB。dual_regression数据大约是其他数据的10倍，约为2.5MB到5MB，func_preproc的数据更大，大约为30MB-200MB，1D结尾的文件大约在0.5MB-1MB之间。

1、对于CCS、CPAC和DPARSF，下面列出的派生类型首先使用非平滑功能数据在原始空间中计算，然后将结果写入MNI152空间，并用6mm半高宽高斯核进行空间平滑。使用每个管道的特定步骤执行注册和平滑。

（1）低频振荡幅度（Amplitude of low frequency fluctuations，ALFF）和低频振荡的分数振幅（Fractional ALFF，fALFF）：ALFF反应的是脑区通过与之有连接的脑区间的信息交互产生自身节律性活动模式，所以ALFF作为反应大脑的特征。fALFF是ALFF值除以整个频段振幅均值得到的。

（2）区域一致性（Regional homogeneity，REHO）：一种基于体素的脑活动测量方法，它评估给定体素的时间序列与其最近邻之间的相似性或同步性。使用肯德尔和谐系数计算体素与周围体素的一致性。这项测量是基于这样一个假设，即大脑的内在活动是由一组体素而不是单个体素来表现的。利用Kendall的一致性系数作为一个指标来评价给定体素及其最近邻的聚类内时间序列的相似性。ReHo不需要事先定义roi，它可以提供有关整个大脑区域局部/区域活动的信息。

（3）使用双回归提取10个内部连接网络（10 Intrinsic Connectivity Networks extracted using Dual Regression）：

2、与1正好相反，下面列出的派生类是非平滑功能数据在MNI152空间中计算后，然后再用6mm半高宽高斯核进行平滑处理。

节点是网络的基本单元，通过边进行连接。在构成整个大脑网络（也称为功能性连接体）的许多节点中，有些节点起着至关重要的作用，可能被认为是网络中的中心。这些中心节点可以通过应用各种基于图论的网络分析技术来识别，这些网络分析技术提供了每个节点的中心性或功能重要性的度量。不同的中心度度量捕获了连接的不同方面。C-PAC能够计算出三个常用的中心度指标：度中心度（DC）、特征向量中心度（EC）和局部功能连通度密度（LFCD）。

度中心度（Degree centrality）：度中心度是局部网络连通性的一种度量，它通过计算到所有其他节点的直接连接（边）的数量来识别连接最多的节点。因此，具有高DC的节点将直接连接到网络中的许多其他节点。度中心性分析倾向于强调高阶皮质联合区，同时显示边缘区和皮质下区的敏感性降低。

特征向量中心度（Eigenvector centrality）：特征向量中心度是衡量全球网络连通性的指标。给定节点的EC反映了它与具有高中心性的其他节点的直接连接数。因此，给定节点的EC不仅取决于其自身的中心性，而且取决于它所连接的节点的中心性。一个具有高EC的节点与许多其他节点有连接，这些节点本身高度连接并且在网络中处于中心位置。与DC相比，EC对边缘区和皮质下区更敏感

（1）加权二值化度中心性（Weighted and binarized degree centrality）：

（2）加权二值化特征向量中心性（Weighted and binarized eigenvector centrality）：

（3）局部功能连通密度（Local functional connectivity density，LFCD）：局部功能连接密度是局部网络连接的另一个度量。一个给定的种子必须是基于体素的掩模，不像DC和EC可以计算roi。LFCD映射查找给定种子的邻居和邻居的邻居，直到边变得弱于给定的阈值。

（4）体素镜像一致性连接（Voxel-mirrored homotopic connectivity，VMHC）：功能一致性是大脑功能结构的一个基本特征，即每个半球的一致性（几何对应）区域之间自发活动模式的同步性。虽然在整个大脑中观察到静息功能连接的一致性模式，但这种连接的强度在不同区域之间可能有所不同。这种变化被认为反映了半球和区域在信息处理方面的专门性。

3、在NIAK预处理方法中，功能数据被写入模板空间，并在计算统计导数之前使用6mm半高宽高斯核进行空间平滑。

感兴趣区域

提取了多组感兴趣区域的平均时间序列。在每种情况下，平均时间序列都是从每个预处理方法的标准空间中提取的功能数据中获取。提取了七个ROI图谱的时间序列：

（1）Automated Anatomical Labeling（AAL）：基于蒙特利尔神经病学研究所（MNI）的大脑解剖结构得到的图谱。该图谱被广泛使用于各类人脑神经科学研究中，用来获取3维空间中某些大脑功能的神经解剖标记，是最常见的图谱之一。AAL图谱共有116个区，其中90个脑区属于大脑（左右各45个），26个脑区属于小脑。

（2）Eickhoff-Zilles（EZ）：一种基于细胞构造分割获得的脑图谱，伴随着SPM预处理软件工具箱被同时发布，EZ图谱将大脑共划分为116个区域。

（3）Harvard-Oxford（HO）：一种基于概率得到的脑图谱，HO图谱将大脑划分为皮质和皮质下区域，HO图谱将大脑共划分为110个区域。

（4）Talaraich-Tournoux（TT）：一种基于骨髓构造分割获得的脑图谱，TT 图谱将大脑共划分为110个区域。

（5）Dosenbach 160：一种基于大脑功能提取的图谱，覆盖了大脑皮层和、小脑的部分，将整个大脑分成160个区域。

（6）Craddock 200（CC200）：一种利用归一化聚类方法得到的，通过聚类结果构造关联矩阵，而后将整个大脑划分为200个区域。

（7）Craddock 400（CC200）：用400个区域描重新述200个区域。

> 最小预处理功能数据文件的下载

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/cpac/func_minimal/[file identifier]_func_minimal.nii.gz
```

只有采用了CPAC方法对数据进行处理才有最小预处理功能数据文件。

> 结构数据文件的下载

由于结构数据的预处理方法的多样性，因此每个预处理方法都使用不同的工具来得到不同的格式。

1、下载使用ANTS处理得到的皮质厚度数据

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/ants/
```

该下载地址还需要加上两个可用的皮质厚度计算方法

（1）以体素测量的皮层厚度的三维体积

```url
anat_thickness/[file identifier]_anat_thickness.nii.gz   
```

（2）包含感兴趣的皮质区域的平均皮质厚度值的数据

```url
roi_thickness/[file identifier]_roi_thickness.txt
```

2、下载使用CIVET处理得到的数据

（1）表面数据（Surfaces）

在立体空间中使用CIVET生成的表面数据。

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/civet/surfaces_[surface]/[file identifier]_[surface].obj
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | file identifier | 文件ID |  |
| 2 | surface | 表面类型 | gray_surface_rsl_left_81920、gray_surface_rsl_right_81920、mid_surface_rsl_left_81920、mid_surface_rsl_right_81920、white_surface_rsl_left_81920、white_surface_rsl_right_81920 |

（2）基于顶点的度量（Vertex-based Measures）

在立体空间中基于顶点的度量数据。

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/civet/surfaces_[derivative]/[file identifier]_[derivative].txt
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | file identifier | 文件ID |  |
| 2 | derivative | 派生类型 | mid_surface_rsl_left_native_area_40mm、mid_surface_rsl_right_native_area_40mm、surface_rsl_left_native_volume_40mm、surface_rsl_right_native_volume_40mm |

（3）基于区域的度量（Region-based Measures）

基于区域度量的数据。

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/civet/surfaces_[derivative]/[file identifier]_[derivative].dat
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | file identifier | 文件ID |  |
| 2 | derivative | 派生类型 | gi_left、gi_right、lobe_areas_40mm_left、lobe_areas_40mm_right、lobe_native_cortex_area_left、lobe_native_cortex_area_right、lobe_thickness_tlink_30mm_left、lobe_thickness_tlink_30mm_right、lobe_volumes_40mm_left、lobe_volumes_40mm_right |

（4）皮质厚度图（Cortical Thickness Maps）

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/civet/thickness_[derivative]/[file identifier]_[derivative].txt
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | file identifier | 文件ID |  |
| 2 | derivative | 派生类型 | cerebral_volume、native_rms_rsl_tlink_30mm_asym_hemi、native_rms_rsl_tlink_30mm_left、native_rms_rsl_tlink_30mm_right |

（5）Freesurfer URL Template

可使用以下链接下载每个主题的全部Freesurfer输出文件夹

```url
https://s3.amazonaws.com/fcp-indi/data/Projects/ABIDE_Initiative/Outputs/freesurfer/5.1/[file identifier]/[sub directory]/[output file]
```

| 编号 | 字段名 | 说明 | 参考 |
| :---: | :---: | :---: | :---: |
| 1 | file identifier | 文件ID |  |
| 2 | sub directory | Freesurfer类型 | label、mri、scripts、stats、surf |
| 3 | output file | 输出文件 | 所需输出文件的名称 |

每个主题有284个数据文件。使用-qcache标记可以重建不同表面度量的版本，这些度量在0、5、10、15、20和25mm处使用半高宽（FWHM）进行平滑。

# 其他

## [20 Newsgroups（新闻组文档）](http://qwone.com/~jason/20Newsgroups/)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 文本数据 | 20 | 20000 | 分类 |