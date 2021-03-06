## 阿里云-天池数据分析竞赛：汽车产品聚类分析
### 1. 项目背景
基于项目提供的汽车相关数据，通过聚类分析的方法实现汽车产品聚类，以构建汽车产品画像、分析产品定位、完成汽车竞品分析等要求。
### 2. 项目数据
项目提供的汽车数据包括26个字段共205条数据，数据文件为“car_price.csv”
26个字段可以划分为类别型变量和数值型变量两种，包括汽车的长/宽/高、汽车净重、燃油系统、燃油类型、驱动类型、峰值转速、里程数、汽车价格等。
### 3. 项目要求
通过聚类的方法构建汽车产品画像、分析不同类别汽车的产品定位，寻找Volkswagen大众汽车的竞品品牌。
### 4. 项目思路
#### 第一步：数据字段理解
1. 根据项目所提供的数据，对数据中26个字段进行理解。结合汽车行业的相关知识，26个字段可以大致归为两类：第一类是车辆自身属性（如燃油系统、燃油类型、汽缸数、峰值转速、汽车长宽高等）；第二类是车辆的市场属性（如车辆名称、车辆价格、风险评估等级）。
2. 26个字段主要分为数值型变量和类别型变量两类。
#### 第二步：原始数据描述性统计及变量分布可视化
对原始数据进行描述性统计并对数据中的字段分布进行可视化（详情见主文档）。通过对原始数据的观察，数据不存在缺失值、不存在重复值，“CarName”字段存在部分车辆品牌名称错误的情况。
#### 第三步：确定聚类方法，明确聚类要求
1. 通过对原始数据的变量观察，该数据变量主要为数值型变量和类别型变量两类，且类别型变量数量较多，常用的K-means聚类只能分析数值型变量，无法考虑类别型变量所包含的信息。二阶段聚类法适用于包含数值型和类别型变量的混合数据，因此考虑使用二阶段聚类法分析数据。
2. 二阶段聚类法的要求是：类别型变量符合多项式分布（即变量的值分属几个类别）；数值型变量间要相互独立，且数值型变量近似服从正态分布。项目所给出的数据中，类别型变量符合多项式分布，因此仅需进一步观察并处理数值型变量。
#### 第四步：特征工程
1. 数据清洗与新变量生成。原始数据指给出了车辆的名称，没有给出车辆所属品牌，结合最终聚类分析的需要，根据“CarName”字段提取出车辆所属品牌信息，命名为“brand”。同时对品牌名称中的错误拼写进行清洗。
2. 变量相关性分析与可视化。由于二阶段聚类要求数值型变量间相互独立，所以需要对数值型变量间的相关性进行查看与处理。相关性分析结果表示14个数值型变量之间存在高相关性情况，需要结合汽车知识背景与变量特征进行进一步处理。
3. 高相关变量的处理——“highwaympg”和“citympg”呈高度正相关。其实不管是高速mpg还是城市mpg，其本质都是mpg指标，而且通过观察数据，二者之间的差异较小（极值、均值），因此考虑将二者合并为一个指标'mpg'，计算方式为取二者均值：mpg=(highwaympg+citympg)/2；
4. 高相关性变量的处理——“price”变量与其余变量产生高相关性的频数最多，可能是因为车辆自身属性和配置的变动会直接影响着车辆的市场价格。此外，与其他变量相比，price属性属于车辆的市场销售属性（而非车辆自身属性），在聚类中更适合作为类别型变量，对车辆的价位进行划分，因此，考虑将price变量转换为类别型变量，按照其价格分布划分为Low price（<=10000）, medium price(10000-20000), high price(>20000)三类;
5. 高相关性变量的处理——对于其余数值型变量，变量数目较多且多个变量之间存在相关性，因此考虑使用因子分析对数值型变量进行降维，以减少数值型变量的数目并使变量间相互独立。
#### 第五步：数值型变量因子分析结果（基于SPSS实现）
1. 利用SPSS对数值型变量进行因子分析，KMO值>0.8，巴特利球形检验p值=0，说明参与因子分析的变量间存在相关性，可以进行因子分析。最终得到两个因子。
2. 第一个因子包括：车长、车宽、车净重、引擎尺寸、车轴距、mpg、马力、车内径比。简单将该因子归纳为车辆截面与马力因子；
3. 第二个因子包括：车高、峰值转速、车压缩比。简单将该因子归纳为车辆垂面与转速因子；
#### 第六步：两阶段聚类及结果（基于SPSS实现）
1. 对处理后的数据进行两阶段聚类，最终将205辆车聚为两类。
2. 根据SPSS聚类结果，第一类中包含120条车辆数据，占总数据的58.5%；第二类中包含85条车辆数据，占总数据的41.5%。两类簇数据规模近似，没有过大或过小的类簇。
3. 根据SPSS聚类结果，聚类质量属于“良好”范围，仍有进一步改进和优化的空间。
4. 根据SPSS聚类结果，显著区分两类类簇的变量（重要性>0.6）按重要性大小排序依次是驱动类型、燃油系统、车辆截面与马力因子、价格范围。
#### 汽车产品画像与产品定位
1. 根据区分类簇的四个重要标签来对数据中的汽车产品进行产品画像与产品定位。
2. 第一类画像：驱动类型多为fwd（前轮驱动），燃油系统多为2bbl（双腔燃油系统）、车辆截面与马力因子（主要为车辆长/宽/高/马力）低于第二类、价格范围集中在中低价位。根据该类别画像可知，该类别中的汽车多采用经济车型常用的前轮驱动，传统的双腔燃油系统，车辆宽敞度和马力较为普通，价格多分布在中低价位区间，因此该类别属于“经济车型”，产品定位属于中低端经济型汽车。在该类别中车辆较多的品牌有：丰田、三菱、本田、斯巴鲁、尼桑、马自达、大众等。
3. 第二类画像：驱动类型多为rwd（后轮驱动），燃油系统多为mpfi（多点燃油系统）、车辆截面与马力因子（主要为车辆长/宽/高/马力）高于于第一类、价格范围集中在中高价位。根据该类别画像可知，该类别中的汽车多采用豪华车型/跑车车型常用的后轮驱动，多点燃油系统，车辆宽敞度和马力较为优秀，价格多分布在中高价位区间，因此该类别属于“豪华车型”，产品定位属于中高端豪华汽车/跑车。在该类别中车辆较多的品牌有宝马、保时捷、捷豹、阿尔法等。
#### 大众品牌汽车竞品品牌
从聚类结果来看，项目数据中的大众品牌汽车多分布在“经济车型”类，因此在本项目背景下，大众品牌车辆的竞品多属于丰田、三菱、本田、斯巴鲁、尼桑、马自达等，而不是宝马、保时捷等高端车品牌。
