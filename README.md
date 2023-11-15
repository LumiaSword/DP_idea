# DP_idea
使用DateP项目中脚本的顺序说明

## 1. TXimport_DESeq2
1. 使用tximport,配合tx2gene文件,将salmon生成的quant.sf文件中transcripts水平数据转换成gene水平数据.
2. 依据DPP区别,使用DESeq2将矩阵标准化.

## 2. DesignMatrix
进一步处理标准化的矩阵.
1. 行名(gene)转列.
2. 宽矩阵转长(pivot_longer).
3. 分割处理identifier列,得到variety,DPP,replicates列
4. 处理raw value.对于每个variety中同一DPP下的每个gene的replicates,取平均值和标准差.对所有raw value取对数(+1后取以10为底的对数值)后再取平均值.

得到一个六列的df: gene,development_time,variety,mean_expression_level,standard_deviation,mean_log_expression

## 3. GO_enrichment_preparation
使用topGO之前的准备工作,得到gene2go
1. 从Uniprot获得指定物种下的所有genenames,entry name(即Accesion),GO_ID,GO_term
2. 处理NA值,处理重复信息(对应多个accession的同一gene,对符合的GO_ID和GO_term取并集) 注:因为项目更关注gene之间的区别而非蛋白质,所以可以这样操作.
3. 只取gene列和GO_ID列.转为list

得到gene2go
