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

得到一个六列的design matrix: gene,development_time,variety,mean_expression_level,standard_deviation,mean_log_expression

## 3. GO_enrichment_preparation
使用topGO之前的准备工作.
1. 从Uniprot获得指定物种下的所有genenames,entry name(即Accesion),GO_ID,GO_term
2. 处理NA值,处理重复信息(对应多个accession的同一gene,对符合的GO_ID和GO_term取并集) 注:因为项目更关注gene之间的区别而非蛋白质,所以可以这样操作.
3. 只取gene列和GO_ID列.转为list

得到geneID2GO (list).

## 4. EC_number
引入Enzyme Comission Number.
1. 获得Uniprot中指定物种下的所有genenames,entry name(即Accesion),EC.
2. 导入kumar2019中的EC.Number以及对应的分类(Class_A,Class_B)
3. 获得KEGG中指定物种下的所有gene,kegg_id,org_info和EC.Number

对kegg的结果进行筛选就可以得到一个六列的df(kegg_kumar):Gene,kegg_id,org_info,EC.Number,Class_A,Class_B

## 5. Function
引入两个function.
1. 由variety specific的design matrix得到该variety经过SimpleTidy_GeneCoEx流程后得到的模组分配df以及resolution和>5gene的modules的关系图.
2. 对模组分配df,测试不同gene sets对不同模组的enrichment程度

## 6. Variety
对于具体Variety的分析.

## 7. TOPGO
使用topGO进行的GO_enrichment
1. 根据需求,将筛选的结果因子化,得到1,2两个水平.
2. 构建topGOobejct. annot选择annFUN.gene2GO. 在第三部分得到的geneID2GO将作为gene2GO参数的输入
3. 运行不同的统计方法(fisher,KS,weigth等).尝试classic/elim算法.

得到Gentable.

## 8. ALL
对所有variety进行分析.

## 9. interest_genes
我们感兴趣的一些gene sets

## 10. DGE
具体的DGE analysis
1. across timepoints
2. across varieties
3. volcano plots function
