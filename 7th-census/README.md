# 7th-census

## 数据结构

第七次全国人口普查（七普）于2020年展开，普查标准时点为2020年11月1日零时，普查对象是普查标准时点在中华人民共和国境内的自然人以及在中华人民共和国境外但未定居的中国公民，不包括在中华人民共和国境内短期停留的境外人员。

普查原始统计数据和整理后的可读取数据存储在[第七次人口普查分县数据](https://china-data-team-bucket-public.s3.cn-northwest-1.amazonaws.com.cn/7th_census/%E7%AC%AC%E4%B8%83%E6%AC%A1%E4%BA%BA%E5%8F%A3%E6%99%AE%E6%9F%A5%E5%88%86%E5%8E%BF%E6%95%B0%E6%8D%AE.xlsx)中，数据表包括：
- **人口普查分县数据**：普查原始数据
- **变量**：普查原始数据变量清单
- **variable**：整理后变量清单与编码
- **data**: 整理后的全国、省级、市级、县级人口统计数据
- **data_province**: 整理后的全国、省级人口统计数据
- **data_city**: 整理后的市级人口统计数据
- **data_county**: 整理后的县级人口统计数据

## 边界数据连接

七普数据整理后，我们将七普数据与省、市、县级行政边界数据进行对应链接。省级与市级七普数据可实现与行政边界数据的一一对应链接，结果存储在[China_POP_province](https://github.com/wri-china/data-sharing/tree/main/7th-census/shp_pop)与[China_POP_city](https://github.com/wri-china/data-sharing/tree/main/7th-census/shp_pop)中。

在整合县级行政区七普数据和行政边界数据的过程中，七普统计数据涵盖了2991个县级行政区，而[China_POP_county](https://github.com/wri-china/data-sharing/tree/main/7th-census/shp_pop)的边界数据则包含了2849个县级行政边界。主要原因是七普统计数据，将经济开发区和高新技术开发区等区域，与县级行政区作为同级，独立计数统计。而县级行政区的边界没有单独划分经济开发区和高新技术开发区等区域。

## 县级数据连接

为了实现七普统计数据与行政边界的连接，我们特别关注了七普统计数据中的142个没有边界信息的经济开发区和高新技术开发区等。通过人工比对，将这些开发区的行政公共单位所在地与[China_POP_county](https://github.com/wri-china/data-sharing/tree/main/7th-census/shp_pop)的边界数据中的2849个县级行政区进行了对比，并使用 `Code_SHP` 作为连接属性列。其他行政边界，则直接通过名称进行比对，同样使用 `Code_SHP` 作为连接属性列。

## 特殊情况

在整合过程中，`福建省泉州市金门县`存在边界数据但无普查数据。另外，`河北省唐山市芦台经济开发区`的位置位于`天津市宁河区`，但行政区划属于`河北省唐山市`。

## 人口属性整合

除了基本边界数据的整合外，在统计数据表中对**含有开发区的行政区**进行了行政区开发区人口的归并，即：行政区（含开发区）人口 = 行政区人口+开发区人口。所有人口统计数据中，共有17个属性列为比例数据，不能进行属性加和：

- `population_4` [性别比]
- `population_5` [少数民族人口比重（%）]
- `population_11` [户规模（人/户）]
- `age_39` [0-14岁人口比重（%）]
- `age_40` [15-64岁人口比重（%）]
- `age_41` [65岁及以上人口比重（%）]
- `age_42` [15-49岁育龄妇女比重（%）]
- `education_15` [平均受教育年限（年）]
- `education_16` [男性平均受教育年限（年）]
- `education_17` [女性平均受教育年限（年）]
- `education_21` [文盲人口占15岁以上人口比重（%）]
- `education_22` [男性占比（%）]
- `education_23` [女性占比（%）]
- `marriage_16` [平均活产子女数（人）]
- `marriage_17` [平均存活子女数（人）]
- `housing_2` [平均每户住房间数（间/户）]
- `housing_3` [人均住房建筑面积（平方米/人）]

##### Note: This dataset processing was done by WRI China Data Team - Weiqi Zhou & Yuchen Guo, and intern Haixia Xu.
