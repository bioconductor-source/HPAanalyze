``` r
library(BiocStyle)
library(HPAanalyze)
library(tibble)
library(dplyr)
library(ggplot2)
library(hpar)
```

Summary
=======

-   **Background:** The Human Protein Atlas program aims to map human proteins via multiple technologies including imaging, proteomics and transcriptomics.
-   **Results:** `HPAanalyze` is an R package for retreiving and performing exploratory data analysis from HPA. It provides functionality for importing data tables and xml files from HPA, exporting and visualizing data, as well as download all staining images of interest. The package is free, open source, and available via Github.
-   **Conclusions:** `HPAanalyze` intergrates into the R workflow via the `tidyverse` philosophy and data structures, and can be used in combination with Bioconductor packages for easy analysis of HPA data.

**Keywords:** Human Protein Atlas, Proteomics, Homo Sapiens, Visualization, Software

Background
==========

The Human Protein Atlas (HPA) is a comprehensive resource for exploration of human proteome which contains a vast amount of proteomics and transcriptomics data generated from antibody-based tissue micro-array profiling and RNA deep-sequencing.

The program has generated protein expression profiles in human normal tissues with cell type-specific expression patterns, cancer and cell lines via an innovative immunohistochemistry-based approach. These profiles are accompanied by a large collection of high quality histological staining images, annotated with clinical data and quantification. The database also includes classification of protein into both functional classes (such as transcription factors or kinases) and project-related classes (such as candidate genes for cancer). Starting from version 4.0, the HPA includes subcellular location profiles generated based on confocal images of immunofluorescent stained cells. Together, these data provide a detailed picture of protein expression in human cells and tissues, facilitating tissue-based diagnostis and research.

Data from the HPA are freely available via proteinatlas.org, allowing scientists to access and incorporate the data into their research. Previously, the R package *hpar* has been created for fast and easy programmatic access of HPA data. Here, we introduce *HPAanalyze*, an R package aims to simplify exploratory data analysis from those data, as well as provide other complementary functionality to *hpar*.

Implementation
==============

*HPAanalyze* is and R package with a MIT licence and is designed for easy retreiving and performing exploratory data analysis from HPA. It provides functionality for importing data tables and xml files from HPA, exporting and visualizing data, as well as download all staining images of interest.

The different HPA data formats
------------------------------

The Human Protein Atlas project provides data via two main mechanisms: *Full datasets* in the form of downloadable compressed tab-separated files (.tsv) and *individual entries* in XML, RDF and TSV formats. The full downloadable datasets includes normal tissue, pathology (cancer), subcellular location, RNA gene and RNA isoform data. For individual entries, the XML format is the most comprehensive, providing information on the target protein, antibodies, summary for each tissue and detailed data from each sample including clinical data, IHC scoring and image download links.

*HPAanalyze* overview
---------------------

*HPAanalyze* is designed to fullfill 3 main tasks: (1) Import, subsetting and export downloadable datasets; (2) Visualization of downloadable datasets for exploratory analysis; and (3) Working with the individual XML files. This package aims to serve researchers with little programming experience, but also allow power users to use the imported data as desired.

### Obtaining *HPAanalyze*

The development version of *HPAanalyze* is available on Github can be installed with:

`devtools::install_github("trannhatanh89/HPAanalyze")`

### Full dataset import, subsetting and export

The `hpaDownload()` function downloads full datasets from HPA and imports them into R as a list of tibbles, the standard object of *tidyverse*, which can subsequently be subset with `hpaSubset()` and export into .xmlx files with `hpaExport()`. The standard object allow the imported data to be further processed in a traditional R workflow. The ability to quickly subset and export data gives researchers the option to use other non-R downstream tools, such as GraphPad for creating publication-quality graphics, or share a subset of data containing only proteins of interest.

### Visualization

The `hpaVis` function family take the output of `hpaDownload()` (or `hpaSubset()`) provides quick visualization of the data, with the intention of aiding exploratory analysis. Nevertheless, the standard `ggplot` object output of these functions give users the option to further customize the plots for publication. All `hpaVis` functions share the same syntax for arguments: subsetting, specifying colors and opting to use custom themes.

The first release of the *HPAanalyze* package includes three functions: `hpaVisTissue()` for the *normal tissue*, `hpaVisPatho()` for the *pathology/cancer*, and `hpaVisSubcell()` for the *subcellular location* datasets.

### Individual xml import and image downloading

The `hpaXml` function family import and extract data from individual XML entries from HPA. The `hpaXmlGet()` function downloads and imports data as "xml\_document"/"xml\_node" object, which can subsequently be processed by other `hpaXml` functions. The XML format from HPA contains a wealth of information that may not be covered by this package. However, users can extract any data of interest from the imported XML file using the xml2 package.

In the first release, *HPAanalyze* includes four functions for data extraction from HPA XML files: `hpaXmlProtClass()` for protein class information, `hpaTissueExprSum()` for summary of protein expression in tissue, `hpaXmlAntibody()` for a list of antibody used to stain for the protein of interest, and `hpaTissueExpr()` for a detailed data from each sample including clinical data and IHC scoring.

`hpaTissueExprSum` and `hpaTissueExpr` provide download links to download relevant staining images, with the former function also gives the options to automate the downloading process.

### Compatibility with `hpar` Bioconductor package

<table>
<caption>(#tab:table) Complementary functionality between hpar and HPAanalyze</caption>
<colgroup>
<col width="17%" />
<col width="41%" />
<col width="41%" />
</colgroup>
<thead>
<tr class="header">
<th align="right">Functionality</th>
<th align="left">hpar</th>
<th align="left">HPAanalyze</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="right">Datasets</td>
<td align="left">Included in package</td>
<td align="left">Download from server or Import from hpar</td>
</tr>
<tr class="even">
<td align="right">Query</td>
<td align="left">Ensembl id</td>
<td align="left">HGNC symbol for datasets, Ensembl id for XML</td>
</tr>
<tr class="odd">
<td align="right">Data version</td>
<td align="left">One stable version</td>
<td align="left">Latest by default, option to download older</td>
</tr>
<tr class="even">
<td align="right">Release info</td>
<td align="left">Access via functions</td>
<td align="left">N/A</td>
</tr>
<tr class="odd">
<td align="right">View relevant browser page</td>
<td align="left">Via <code>getHPA</code> function</td>
<td align="left">N/A</td>
</tr>
<tr class="even">
<td align="right">Visualization</td>
<td align="left">N/A</td>
<td align="left">Exploratory via <code>hpaVis</code> functions</td>
</tr>
<tr class="odd">
<td align="right">XML</td>
<td align="left">N/A</td>
<td align="left">Download and import via <code>hpaXml</code> functions</td>
</tr>
<tr class="even">
<td align="right">Histology image</td>
<td align="left">View by loading browser page</td>
<td align="left">Extract links via <code>hpaXml</code> functions</td>
</tr>
</tbody>
</table>

Working with HPA downloadable datasets
--------------------------------------

A typical workflow with HPA downloadable datasets using *HPAanalyze* consist of the following steps: (1) Download and import data into R with `hpaDownload()`; (2) View available parameter for subseting with `hpaListParam()`; (3) Subset data with `hpaSubset()`; and (4) (optional) Export data with `hpaExport`.

### Download and import data with `hpaDownload()`

``` r
downloadedData <- hpaDownload(downloadList='all')
summary(downloadedData)

#>                          Length Class  Mode
#> normal_tissue             6     tbl_df list
#> pathology                11     tbl_df list
#> subcellular_location     11     tbl_df list
#> rna_tissue                5     tbl_df list
#> rna_cell_line             5     tbl_df list
#> transcript_rna_tissue     4     tbl_df list
#> transcript_rna_cell_line  4     tbl_df list
```

The `normal_tissue` dataset contains information about protein expression profiles in human tissues based on IHC staining. The datasets contain six columns: `ensembl` (Ensembl gene identifier); `gene` (HGNC symbol), `tissue` (tissue name); `cell_type` (annotated cell type); `level` (expression value); `reliability` (the gene reliability of the expression value).

``` r
downloadedData <- hpaDownload(downloadList='histology', version='example')
tibble::glimpse(downloadedData$normal_tissue, give.attr=FALSE)
#> Observations: 1,053,330
#> Variables: 5
#> $ ensembl   <chr> "ENSG00000000003", "ENSG00000000003", "ENSG000000000...
#> $ gene      <chr> "TSPAN6", "TSPAN6", "TSPAN6", "TSPAN6", "TSPAN6", "T...
#> $ tissue    <chr> "adrenal gland", "appendix", "appendix", "bone marro...
#> $ cell_type <chr> "glandular cells", "glandular cells", "lymphoid tiss...
#> $ level     <chr> "Not detected", "Medium", "Not detected", "Not detec...
```

The `pathology` dataset contains information about protein expression profiles in human tumor tissue based on IHC staining. The datasets contain eleven columns: `ensembl` (Ensembl gene identifier); `gene` (HGNC symbol); `cancer` (cancer type); `high`, `medium`, `low`, `not_detected` (number of patients annotated for different staining levels); `prognostic_favorable`, `unprognostic_favorable`, `prognostic_unfavorable`, `unprognostic_unfavorable` (log-rank p values for patient survival and mRNA correlation).

``` r
tibble::glimpse(downloadedData$pathology, give.attr=FALSE)
#> Observations: 392,260
#> Variables: 7
#> $ ensembl      <chr> "ENSG00000000003", "ENSG00000000003", "ENSG000000...
#> $ gene         <chr> "TSPAN6", "TSPAN6", "TSPAN6", "TSPAN6", "TSPAN6",...
#> $ cancer       <chr> "breast cancer", "carcinoid", "cervical cancer", ...
#> $ high         <int> 1, 0, 11, 0, 10, 0, 0, 4, 8, 0, 0, 8, 11, 5, 2, 9...
#> $ medium       <int> 7, 1, 1, 6, 2, 0, 3, 5, 4, 0, 1, 3, 1, 6, 4, 2, 1...
#> $ low          <int> 2, 1, 0, 2, 0, 0, 1, 1, 0, 0, 2, 0, 0, 0, 1, 0, 0...
#> $ not_detected <int> 2, 2, 0, 2, 0, 11, 0, 0, 0, 11, 9, 0, 0, 0, 5, 0,...
```

The `subcellular_location` dataset contains information about subcellular localization of proteins based on IF stanings of normal cells. The datasets contain eleven columns: `ensembl` (Ensembl gene identifier); `gene` (HGNC symbol); `reliability` (gene reliability score); `enhanced` (enhanced locations); `supported` (supported locations); `approved` (approved locations); `uncertain` (uncertain locations); `single_cell_var_intensity` (locations with single-cell variation in intensity); `single_cell_var_spatial` (locations with spatial single-cell variation); `cell_cycle_dependency` (locations with observed cell cycle dependency); `go_id` (Gene Ontology Cellular Component term identifier).

``` r
tibble::glimpse(downloadedData$subcellular_location, give.attr=FALSE)
#> Observations: 12,073
#> Variables: 8
#> $ ensembl     <chr> "ENSG00000000003", "ENSG00000000457", "ENSG0000000...
#> $ gene        <chr> "TSPAN6", "SCYL3", "C1orf112", "FGR", "CFH", "GCLC...
#> $ reliability <chr> "Approved", "Uncertain", "Approved", "Approved", "...
#> $ enhanced    <chr> NA, NA, NA, NA, NA, NA, "Nucleoplasm", NA, NA, NA,...
#> $ supported   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, "Nucleoplasm",...
#> $ approved    <chr> "Cytosol", NA, "Mitochondria", "Aggresome;Plasma m...
#> $ uncertain   <chr> NA, "Microtubules;Nuclear bodies", NA, NA, NA, NA,...
#> $ go_id       <chr> "Cytosol (GO:0005829)", "Microtubules (GO:0015630)...
```

The `rna_tissue` and `rna_cell_line` datasets contain RNA expression levels of 37 tissues and 64 cell lines based on RNA-seq. These datasets contain four columns each: `ensembl` (Ensembl gene identifier); `gene` (HGNC symbol); `tissue`/`cell_line` (type of sample); `value` + `unit` (expression level measured by transcripts per million).

``` r
downloadedData <- hpaDownload(downloadList='rna', version='hpar')

tibble::glimpse(downloadedData$rna_tissue, give.attr=FALSE)
#> Observations: 725,681
#> Variables: 5
#> $ ensembl <fct> ENSG00000000003, ENSG00000000003, ENSG00000000003, ENS...
#> $ gene    <fct> TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6...
#> $ tissue  <fct> "adipose tissue", "adrenal gland", "appendix", "bone m...
#> $ value   <dbl> 31.5, 26.4, 9.2, 0.7, 53.4, 18.5, 54.2, 48.5, 27.1, 38...
#> $ unit    <fct> TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM,...
```

``` r
tibble::glimpse(downloadedData$rna_cell_line, give.attr=FALSE)
#> Observations: 1,255,232
#> Variables: 5
#> $ ensembl   <fct> ENSG00000000003, ENSG00000000003, ENSG00000000003, E...
#> $ gene      <fct> TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPA...
#> $ cell_line <fct> A-431, A549, AF22, AN3-CA, ASC diff, ASC TERT1, BEWO...
#> $ value     <dbl> 27.8, 37.6, 108.1, 51.8, 32.3, 17.7, 42.7, 14.9, 22....
#> $ unit      <fct> TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TPM, TP...
```

Similarly, the `transcript_rna_tissue` and `transcript_rna_cell_line` datasets contain RNA isoform levels. These datasets contain four columns each: `ensembl` (Ensembl gene identifier); `transcript` (Ensembl transcript identifier); `tissue`/`cell_line` (type of sample); `value` (expression level measured by transcripts per million). Note that these datasets are significantly larger than others and should only be downloaded when necessary.

``` r
downloadedData <- hpaDownload(downloadList='isoform', version='v18')

tibble::glimpse(downloadedData$transcript_rna_tissue, give.attr=FALSE)

#> Observations: 27,535,996
#> Variables: 4
#> $ ensembl    <chr> "ENSG00000000003", "ENSG00000000003", "ENSG0000000...
#> $ transcript <chr> "ENST00000373020", "ENST00000494424", "ENST0000049...
#> $ tissue     <chr> "adipose tissue.V1", "adipose tissue.V1", "adipose...
#> $ value      <dbl> 27.3577003, 0.0000000, 1.9341500, 1.6059300, 0.000...
```

``` r
tibble::glimpse(downloadedData$transcript_rna_cell_line, give.attr=FALSE)

#> Observations: 20,972,183
#> Variables: 4
#> $ ensembl    <chr> "ENSG00000000003", "ENSG00000000003", "ENSG0000000...
#> $ transcript <chr> "ENST00000373020", "ENST00000494424", "ENST0000049...
#> $ cell_line  <chr> "A-431.C35", "A-431.C35", "A-431.C35", "A-431.C35"...
#> $ value      <dbl> 29.406799, 0.000000, 0.992916, 0.398387, 0.239204,...
```

Optionally, data can be imported from the *hpar* package. Please note that the *hpar* package does not contain the RNA isoform datasets, which are very large and should be downloaded only when necessary.

For data from *hpar*, release information can be accessed with `getHpaVersion`, `getHpaDate` and `getHpaEnsembl`.

``` r
hpaDownload('all', 'hpar')
```

``` r
hpar::getHpaVersion()
#> version 
#>    "18"

hpar::getHpaDate()
#>         date 
#> "2017.12.01"

hpar::getHpaEnsembl()
#> ensembl 
#> "88.38"
```

### List available parameter for subsetting with `hpaListParam()`

To see what parameters are available for subsequent subsetting/visualizing, HPAanalyze includes the function `hpaListParam()`. The input for this function is the output of `hpaDownload`.

``` r
downloadedData <- hpaDownload(downloadList='all')
str(hpaListParam(downloadedData))

#> List of 6
#>  $ normal_tissue       : chr [1:58] "adrenal gland" "appendix" "bone marrow" "breast" ...
#>  $ normal_cell         : chr [1:82] "glandular cells" "lymphoid tissue" "hematopoietic cells" "adipocytes" ...
#>  $ cancer              : chr [1:20] "breast cancer" "carcinoid" "cervical cancer" "colorectal cancer" ...
#>  $ subcellular_location: chr [1:32] "Cytosol" "Mitochondria" "Aggresome" "Plasma membrane" ...
#>  $ normal_tissue_rna   : chr [1:37] "adipose tissue" "adrenal gland" "appendix" "bone marrow" ...
#>  $ cell_line_rna       : chr [1:64] "A-431" "A549" "AF22" "AN3-CA" ...
```

### Subset data with `hpaSubset()`

`hpaSubset()` filters the output of `hpaDownload()` for desirable target genes, tissues, cell types, cancer, and cell lines. The data will be subset only where applicable (i.e. `normal_tissue` will not be subset by *cancer*). The main purpose of `hpaSubset` is to prepare a manageable set of data to be exported. However, this function may also be useful for other data table manipulation purposes. The input for `targetGene` argument is a vector of strings of HGNC symbols.

``` r
downloadedData <- hpaDownload(downloadList='histology', version='example')
sapply(downloadedData, nrow)
#>        normal_tissue            pathology subcellular_location 
#>              1053330               392260                12073
```

``` r
geneList <- c('TP53', 'EGFR', 'CD44', 'PTEN', 'IDH1', 'IDH2', 'CYCS')
tissueList <- c('breast', 'cerebellum', 'skin 1')
cancerList <- c('breast cancer', 'glioma', 'melanoma')
cellLineList <- c('A-431', 'A549', 'AF22', 'AN3-CA')

subsetData <- hpaSubset(data=downloadedData,
                         targetGene=geneList,
                         targetTissue=tissueList,
                         targetCancer=cancerList,
                         targetCellLine=cellLineList)
sapply(subsetData, nrow)
#>        normal_tissue            pathology subcellular_location 
#>                   70                   21                    7
```

To subset by Ensemble gene id, use `hpar::getHPA()`.

``` r
id <- c("ENSG00000000003", "ENSG00000000005")
hpar::getHpa(id, hpadata="hpaNormalTissue") %>%
    tibble::glimpse()
#> Observations: 80
#> Variables: 6
#> $ Gene        <fct> ENSG00000000003, ENSG00000000003, ENSG00000000003,...
#> $ Gene.name   <fct> TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TSPAN6, TS...
#> $ Tissue      <fct> "adrenal gland", "appendix", "appendix", "bone mar...
#> $ Cell.type   <fct> glandular cells, glandular cells, lymphoid tissue,...
#> $ Level       <fct> Not detected, Medium, Not detected, Not detected, ...
#> $ Reliability <fct> Approved, Approved, Approved, Approved, Approved, ...
```

### Export data with `hpaExport()`

As the name suggests, `hpaExport()` exports the output of `hpaSubset()` to one .xlsx file. Each dataset is placed in a separate sheet. More formats such as .csv and .tsv might be added in future release; hence, the `fileType` argument is included.

``` r
hpaExport(subsetData, fileName='subset.xlsx', fileType='xlsx')
```

Visualize downloaded data
-------------------------

`HPAanalyze` provides the ability to quickly visualize data from downloaded HPA datasets with the `hpaVis` function family. The goal of these functions is to aid exploratory analysis of a group of target genes, which maybe particularly useful for gaining insights into pathways or gene signatures of interest.

The `hpaVis` functions share a common syntax, where the input (`data` argument) is the output of `hpaDownload()` or `hpaSubset()` (although they do their own subseting so it is not necessary to use `hpaSubset()` unless you want to reduce the size of your data object). Depending on the function, the `target` arguments will let you choose to visualize your vectors of genes, tissue, cell types, etc. (See the help files for more details.) All of `hpaVis` functions generate standard `ggplot2` plots, which allow you to further customize colors and themes. Colors maybe changed via the `color` argument, while the default theme maybe overriden by setting the `customTheme` argument to `FALSE`.

Currently, the `normal_tissue`, `pathology` and `subcellular_location` data can be visualized, with more functions planned for future releases.

### Visualize tissue data with `hpaVisTissue()`

`hpaVisTissue()` generates a "heatmap", in which the expression of proteins of interest (quantified IHC staining) are plotted for each cell type of each tissue.

``` r
geneList <- c('TP53', 'EGFR', 'CD44', 'PTEN', 'IDH1', 'IDH2', 'CYCS')
tissueList <- c('breast', 'cerebellum', 'skin 1')

hpaVisTissue(downloadedData,
             targetGene=geneList,
             targetTissue=tissueList)
```

![](HPAanalyze_basics_files/figure-markdown_github/visTissue,%20fig.wide-1.png)

### Visualize expression in cancer with `hpaVisPatho()`

`hpaVisPatho()` generates an arrays of column graphs showing the expression of proteins of interest in each cancer.

This example also demonstrate how the colors of the graphs could be customized, which is a common functionality of the `hpaVis` family.

``` r
geneList <- c('TP53', 'EGFR', 'CD44', 'PTEN', 'IDH1', 'IDH2', 'CYCS')
cancerList <- c('breast cancer', 'glioma', 'lymphoma', 'prostate cancer')
colorGray <- c('slategray1', 'slategray2', 'slategray3', 'slategray4')

hpaVisPatho(downloadedData,
            targetGene=geneList,
            targetCancer=cancerList,
            color=colorGray)
```

![](HPAanalyze_basics_files/figure-markdown_github/visPatho,%20fig.wide-1.png)

### Visualize subcellular location data with `hpaVisSubcell()`

`hpaVisSubcell()` generates a tile chart showing the subcellular locations (approved and supported) of proteins of interest.

This example also demonstrate the customization of the output plot with ggplot2 functions, which is applicable to all `hpaVis` functions. Notice that the `customTheme` argument is set to `TRUE`.

``` r
geneList <- c('TP53', 'EGFR', 'CD44', 'PTEN', 'IDH1', 'IDH2', 'CYCS')

hpaVisSubcell(downloadedData,
              targetGene=geneList,
              customTheme=TRUE) +
    ggplot2::theme_minimal() +
    ggplot2::ylab('Subcellular locations') +
    ggplot2::xlab('Protein') +
    ggplot2::theme(axis.text.x=element_text(angle=45, hjust=1))  +
    ggplot2::theme(legend.position="none") +
    ggplot2::coord_equal()
```

![](HPAanalyze_basics_files/figure-markdown_github/visSubcell-1.png)

Working with individual xml files
---------------------------------

`hpaXml` function family supports importing and extracting data from individual XML files provided by HPA for each protein.

A typical workflow for working with XML files includes the following steps: (1) Download and import XML file with `hpaXmlGet()`; (2) Extract the desired information with other `hpaXml` functions; and (3) Download histological staining pictures, which is currently supported by the `hpaXmlTissurExpr()` and `hpaXmlTissueExprSum()` functions.

### Import xml file with `hpaXmlGet()`

The `hoaXmlGet()` function takes a Ensembl gene id (start with *ENSG*) and import the perspective XML file into R. This function calls the `xml2::read_xml()` under the hood, hence the resulting object may be processed further with *xml2* functions if desired.

``` r
CCNB1xml <- hpaXmlGet('ENSG00000134057')
```

To view the web page for the protein of interest, which contains similar information in a pretty presentation, use *hpar* function `getHPA()` with argument `type="details"`.

``` r
hpar::getHpa('ENSG00000134057', type="details")
```

### View protein classes with `hpaXmlProtClass()`

Protein class of queried protein can be extracted from the imported XML with `hpaXmlProtClass()`. The output of this function is a tibble of 4 columns: `id`, `name`, `parent_id` and `source`

``` r
hpaXmlProtClass(CCNB1xml)
#> # A tibble: 10 x 4
#>    id    name                              parent_id source                
#>    <chr> <chr>                             <chr>     <chr>                 
#>  1 Ja    Transporters                      <NA>      TCDB                  
#>  2 Jb    Transporter channels and pores    Ja        TCDB                  
#>  3 Mi    SPOCTOPUS predicted membrane pro~ <NA>      SPOCTOPUS             
#>  4 Za    Predicted intracellular proteins  <NA>      HPA                   
#>  5 Pp    Plasma proteins                   <NA>      Plasma Proteome Datab~
#>  6 Ca    Cancer-related genes              <NA>      <NA>                  
#>  7 Cb    Candidate cancer biomarkers       Ca        Plasma Proteome Insti~
#>  8 Ua    UniProt - Evidence at protein le~ <NA>      UniProt               
#>  9 Ea    Protein evidence (Kim et al 2014) <NA>      Kim et al 2014        
#> 10 Eb    Protein evidence (Ezkurdia et al~ <NA>      Ezkurdia et al 2014
```

### Get summary and images of tissue expression with `hpaXmlTissueExprSum()`

The function `hpaXmlTissueExprSum()` extract the summary of expression of protein of interest in normal tissue. The output of this function is a list of (1) a string contains one-sentence summary and (2) a dataframe of all tissues in which the protein was stained positive and a histological stain images of those tissue.

``` r
hpaXmlTissueExprSum(CCNB1xml)
#> $summary
#> [1] "Cytoplasmic expression in proliferating cells."
#> 
#> $img
#>            tissue
#> 1 cerebral cortex
#> 2      lymph node
#> 3           liver
#> 4           colon
#> 5          kidney
#> 6          testis
#> 7        placenta
#>                                                               imageUrl
#> 1 http://v18.proteinatlas.org/images/3804/10580_B_8_5_rna_selected.jpg
#> 2 http://v18.proteinatlas.org/images/3804/10580_A_8_8_rna_selected.jpg
#> 3 http://v18.proteinatlas.org/images/3804/10580_A_7_4_rna_selected.jpg
#> 4 http://v18.proteinatlas.org/images/3804/10580_A_9_3_rna_selected.jpg
#> 5 http://v18.proteinatlas.org/images/3804/10580_A_9_5_rna_selected.jpg
#> 6 http://v18.proteinatlas.org/images/3804/10580_A_4_6_rna_selected.jpg
#> 7 http://v18.proteinatlas.org/images/3804/10580_A_2_7_rna_selected.jpg
```

Those images can be downloaded automatically by setting the `downloadImg` argument to `TRUE`. Eg. `hpaXmlTissueExprSum(CCNB1xml, downloadImg=TRUE)`

### Get details of individual IHC samples with `hpaXmlAntibody()` and `hpaXmlTissueExpr()`

More importantly, the XML files are the only format of HPA programmatically accesible data which contains information about each antibody and each tissue sample used in the project.

`hpaXmlAntibody()` extract the antibody information and return a tibble with one row for each antibody.

``` r
hpaXmlAntibody(CCNB1xml)
#> # A tibble: 4 x 4
#>   id        releaseDate releaseVersion RRID      
#>   <chr>     <chr>       <chr>          <chr>     
#> 1 CAB000115 2006-03-13  1.2            <NA>      
#> 2 CAB003804 2006-10-30  2              AB_562272 
#> 3 HPA030741 2013-12-05  12             AB_2673586
#> 4 HPA061448 2016-12-04  16             AB_2684522
```

`hpaXmlTissueExpr()` extract information about all samples for each antibody above and return a list of tibbles. If antibody has not been used for IHC staining, the returned tibble with be empty.

``` r
tissueExpression <- hpaXmlTissueExpr(CCNB1xml)
summary(tissueExpression)
#>      Length Class  Mode
#> [1,] 18     tbl_df list
#> [2,] 18     tbl_df list
#> [3,]  0     tbl_df list
#> [4,] 16     tbl_df list
```

Each tibble contain clinical data (`patientid`, `age`, `sex`), tissue information (`snomedCode`, `tissueDescription`), staining results (`staining`, `intensity`, `location`) and one `imageUrl` for each sample. However, due to the large amount of data and the relatively large size of each image, `hpaXmlTissueExpr` does not provide an automated download option.

``` r
tissueExpression[[1]]
#> # A tibble: 331 x 18
#>    patientId age   sex   staining intensity quantity location imageUrl
#>    <chr>     <chr> <chr> <chr>    <chr>     <chr>    <chr>    <chr>   
#>  1 1653      53    Male  <NA>     <NA>      <NA>     <NA>     http://~
#>  2 1721      60    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#>  3 1725      57    Male  <NA>     <NA>      <NA>     <NA>     http://~
#>  4 598       7     Male  <NA>     <NA>      <NA>     <NA>     http://~
#>  5 666       82    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#>  6 2637      36    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#>  7 2667      75    Male  <NA>     <NA>      <NA>     <NA>     http://~
#>  8 2671      72    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#>  9 1447      45    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#> 10 1452      44    Fema~ <NA>     <NA>      <NA>     <NA>     http://~
#> # ... with 321 more rows, and 10 more variables: snomedCode1 <chr>,
#> #   snomedCode2 <chr>, snomedCode3 <chr>, snomedCode4 <chr>,
#> #   snomedCode5 <chr>, tissueDescription1 <chr>, tissueDescription2 <chr>,
#> #   tissueDescription3 <chr>, tissueDescription4 <chr>,
#> #   tissueDescription5 <chr>
```

Future developments
-------------------

The package will continue to follow new updates of the HPA projects to ensure compatibility and functionality. More functions of the `hpaVis` and `hpaXml` family may also be added in future versions.

Conclusion
==========

TBA

Availability and requirements
=============================

-   Project name: HPAanalysis
-   Project home page: <https://github.com/trannhatanh89/HPAanalyze>
-   Operating system(s): All platforms where R is available, including Windows, Linux, OS X
-   Programming language: R
-   Other requirements: R 3.5.0 or higher, and the R packages dplyr, XLConnect, ggplot2, readr, tibble, xml2, reshape2, tidyr, stats, utils, and hpar
-   License: MIT
-   Any restrictions to use by non-academics: Freely available to everyone

Abbreviations
=============

HPA: The Human Protein Atlas; XML: Extensible Markup Language

Declarations
============

Acknowledgements
----------------

We appreciate the support of the National institutes of Health National Cancer Institute R01 CA151522 and funds from the Department of Cell, Developmental and Integrative Biology at the University of Alabama at Birmingham.

Authors' contributions
----------------------

ANT created the R package and drafted the manuscript. ABH supervised the project and revised the manuscript. All authors read and approved the final manuscript.

References
==========

TBA

Copyright
=========

Â© Anh Tran et al. 2018

Session info
============

    #> R version 3.5.1 (2018-07-02)
    #> Platform: x86_64-w64-mingw32/x64 (64-bit)
    #> Running under: Windows 10 x64 (build 17134)
    #> 
    #> Matrix products: default
    #> 
    #> locale:
    #> [1] LC_COLLATE=English_United States.1252 
    #> [2] LC_CTYPE=English_United States.1252   
    #> [3] LC_MONETARY=English_United States.1252
    #> [4] LC_NUMERIC=C                          
    #> [5] LC_TIME=English_United States.1252    
    #> 
    #> attached base packages:
    #> [1] stats     graphics  grDevices utils     datasets  methods   base     
    #> 
    #> other attached packages:
    #> [1] bindrcpp_0.2.2   hpar_1.24.0      ggplot2_3.1.0    dplyr_0.7.8     
    #> [5] tibble_2.0.1     HPAanalyze_1.1.0 BiocStyle_2.10.0
    #> 
    #> loaded via a namespace (and not attached):
    #>  [1] zip_1.0.0          Rcpp_1.0.0         pillar_1.3.1      
    #>  [4] compiler_3.5.1     BiocManager_1.30.4 plyr_1.8.4        
    #>  [7] bindr_0.1.1        tools_3.5.1        digest_0.6.18     
    #> [10] evaluate_0.12      gtable_0.2.0       pkgconfig_2.0.2   
    #> [13] rlang_0.3.1        openxlsx_4.1.0     cli_1.0.1         
    #> [16] curl_3.3           yaml_2.2.0         xfun_0.4          
    #> [19] withr_2.1.2        stringr_1.3.1      knitr_1.21        
    #> [22] xml2_1.2.0         hms_0.4.2          grid_3.5.1        
    #> [25] tidyselect_0.2.5   glue_1.3.0         R6_2.3.0          
    #> [28] fansi_0.4.0        rmarkdown_1.11     reshape2_1.4.3    
    #> [31] tidyr_0.8.2        readr_1.3.1        purrr_0.2.5       
    #> [34] magrittr_1.5       scales_1.0.0       htmltools_0.3.6   
    #> [37] assertthat_0.2.0   colorspace_1.4-0   labeling_0.3      
    #> [40] utf8_1.1.4         stringi_1.2.4      lazyeval_0.2.1    
    #> [43] munsell_0.5.0      crayon_1.3.4
