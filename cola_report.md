cola Report for Consensus Partitioning
==================

**Date**: 2019-12-05 03:49:49 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 15837    56
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:hclust](#SD-hclust)     |          6| 1.000|           1.000|       1.000|** |2,3,4,5    |
|[SD:kmeans](#SD-kmeans)     |          2| 1.000|           0.997|       0.996|** |           |
|[SD:skmeans](#SD-skmeans)   |          6| 1.000|           0.990|       0.977|** |2,5        |
|[SD:pam](#SD-pam)           |          6| 1.000|           1.000|       1.000|** |2,3,5      |
|[SD:mclust](#SD-mclust)     |          6| 1.000|           0.965|       0.980|** |4,5        |
|[CV:kmeans](#CV-kmeans)     |          2| 1.000|           0.983|       0.979|** |           |
|[CV:skmeans](#CV-skmeans)   |          6| 1.000|           0.989|       0.979|** |           |
|[CV:pam](#CV-pam)           |          6| 1.000|           1.000|       1.000|** |2,3,5      |
|[CV:NMF](#CV-NMF)           |          6| 1.000|           0.996|       0.946|** |2,3,4,5    |
|[MAD:hclust](#MAD-hclust)   |          6| 1.000|           1.000|       1.000|** |2,3,4,5    |
|[MAD:skmeans](#MAD-skmeans) |          6| 1.000|           0.997|       0.993|** |2,5        |
|[MAD:pam](#MAD-pam)         |          6| 1.000|           1.000|       1.000|** |2,4,5      |
|[MAD:mclust](#MAD-mclust)   |          6| 1.000|           1.000|       1.000|** |4,5        |
|[MAD:NMF](#MAD-NMF)         |          6| 1.000|           0.999|       0.990|** |2,3,4,5    |
|[ATC:hclust](#ATC-hclust)   |          6| 1.000|           0.984|       0.993|** |2,3        |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:pam](#ATC-pam)         |          6| 1.000|           0.963|       0.985|** |2,3,4,5    |
|[ATC:mclust](#ATC-mclust)   |          3| 1.000|           0.984|       0.984|** |2          |
|[ATC:NMF](#ATC-NMF)         |          2| 1.000|           1.000|       1.000|** |           |
|[SD:NMF](#SD-NMF)           |          6| 0.965|           0.997|       0.964|** |2,4,5      |
|[CV:hclust](#CV-hclust)     |          6| 0.933|           0.970|       0.969|*  |2          |
|[CV:mclust](#CV-mclust)     |          6| 0.933|           0.975|       0.975|*  |5          |
|[ATC:skmeans](#ATC-skmeans) |          6| 0.911|           0.816|       0.751|*  |2,3        |
|[MAD:kmeans](#MAD-kmeans)   |          5| 0.717|           0.813|       0.788|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; CV:NMF      2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; MAD:NMF     2 1.000           1.000       1.000          0.300 0.701   0.701
#&gt; ATC:NMF     2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; SD:skmeans  2 1.000           0.998       0.999          0.300 0.701   0.701
#&gt; CV:skmeans  2 0.751           0.880       0.947          0.353 0.701   0.701
#&gt; MAD:skmeans 2 1.000           1.000       1.000          0.445 0.556   0.556
#&gt; ATC:skmeans 2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; SD:mclust   2 0.751           0.862       0.941          0.359 0.701   0.701
#&gt; CV:mclust   2 0.751           0.953       0.975          0.324 0.701   0.701
#&gt; MAD:mclust  2 0.751           0.943       0.970          0.330 0.701   0.701
#&gt; ATC:mclust  2 1.000           0.996       0.997          0.302 0.701   0.701
#&gt; SD:kmeans   2 1.000           0.997       0.996          0.299 0.701   0.701
#&gt; CV:kmeans   2 1.000           0.983       0.979          0.296 0.701   0.701
#&gt; MAD:kmeans  2 0.844           0.957       0.963          0.321 0.701   0.701
#&gt; ATC:kmeans  2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; SD:pam      2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; CV:pam      2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; MAD:pam     2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; ATC:pam     2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; SD:hclust   2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; CV:hclust   2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; MAD:hclust  2 1.000           1.000       1.000          0.299 0.701   0.701
#&gt; ATC:hclust  2 1.000           1.000       1.000          0.299 0.701   0.701
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.862           0.892       0.950          0.795 0.803   0.719
#&gt; CV:NMF      3 0.917           0.966       0.980          0.989 0.688   0.556
#&gt; MAD:NMF     3 0.906           0.965       0.979          1.045 0.688   0.556
#&gt; ATC:NMF     3 0.734           0.884       0.930          0.671 0.766   0.667
#&gt; SD:skmeans  3 0.803           0.921       0.955          0.759 0.803   0.719
#&gt; CV:skmeans  3 0.647           0.941       0.963          0.707 0.688   0.556
#&gt; MAD:skmeans 3 0.657           0.765       0.864          0.452 0.771   0.589
#&gt; ATC:skmeans 3 1.000           0.984       0.993          0.683 0.803   0.719
#&gt; SD:mclust   3 0.575           0.897       0.904          0.579 0.735   0.622
#&gt; CV:mclust   3 0.621           0.836       0.897          0.694 0.803   0.719
#&gt; MAD:mclust  3 0.831           0.871       0.941          0.880 0.688   0.556
#&gt; ATC:mclust  3 1.000           0.984       0.984          0.675 0.803   0.719
#&gt; SD:kmeans   3 0.532           0.737       0.839          0.915 0.688   0.556
#&gt; CV:kmeans   3 0.460           0.739       0.813          0.893 0.688   0.556
#&gt; MAD:kmeans  3 0.418           0.678       0.805          0.817 0.688   0.556
#&gt; ATC:kmeans  3 0.553           0.888       0.911          0.804 0.766   0.667
#&gt; SD:pam      3 1.000           1.000       1.000          0.659 0.803   0.719
#&gt; CV:pam      3 1.000           1.000       1.000          0.659 0.803   0.719
#&gt; MAD:pam     3 0.651           0.748       0.847          0.879 0.803   0.719
#&gt; ATC:pam     3 1.000           0.975       0.984          0.810 0.766   0.667
#&gt; SD:hclust   3 1.000           1.000       1.000          0.659 0.803   0.719
#&gt; CV:hclust   3 0.803           0.957       0.977          0.721 0.803   0.719
#&gt; MAD:hclust  3 1.000           1.000       1.000          0.659 0.803   0.719
#&gt; ATC:hclust  3 1.000           0.992       0.997          0.876 0.735   0.622
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 1.000           0.985       0.988         0.3387 0.771   0.546
#&gt; CV:NMF      4 1.000           0.999       0.999         0.2166 0.803   0.542
#&gt; MAD:NMF     4 1.000           0.987       0.989         0.1762 0.803   0.542
#&gt; ATC:NMF     4 0.597           0.457       0.765         0.3018 0.735   0.512
#&gt; SD:skmeans  4 0.875           0.939       0.967         0.3709 0.771   0.546
#&gt; CV:skmeans  4 0.803           0.913       0.946         0.1908 0.803   0.542
#&gt; MAD:skmeans 4 0.764           0.866       0.925         0.1263 0.948   0.841
#&gt; ATC:skmeans 4 0.799           0.956       0.924         0.3074 0.782   0.567
#&gt; SD:mclust   4 1.000           0.971       0.986         0.2742 0.777   0.522
#&gt; CV:mclust   4 0.751           0.887       0.921         0.2854 0.771   0.546
#&gt; MAD:mclust  4 1.000           0.998       0.999         0.1685 0.886   0.707
#&gt; ATC:mclust  4 0.677           0.580       0.788         0.3632 0.797   0.598
#&gt; SD:kmeans   4 0.556           0.773       0.765         0.2109 0.803   0.542
#&gt; CV:kmeans   4 0.490           0.653       0.724         0.2081 0.958   0.893
#&gt; MAD:kmeans  4 0.550           0.757       0.773         0.1765 0.803   0.542
#&gt; ATC:kmeans  4 0.577           0.558       0.701         0.2693 0.831   0.639
#&gt; SD:pam      4 0.809           0.940       0.956         0.4463 0.766   0.536
#&gt; CV:pam      4 0.830           0.900       0.925         0.4197 0.766   0.536
#&gt; MAD:pam     4 1.000           0.963       0.986         0.2947 0.766   0.536
#&gt; ATC:pam     4 0.997           0.952       0.979         0.3470 0.800   0.572
#&gt; SD:hclust   4 1.000           0.991       0.993         0.4534 0.771   0.546
#&gt; CV:hclust   4 0.855           0.977       0.973         0.2654 0.844   0.691
#&gt; MAD:hclust  4 1.000           1.000       1.000         0.4598 0.771   0.546
#&gt; ATC:hclust  4 0.972           0.957       0.943         0.0299 0.984   0.964
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.942           0.935       0.938         0.0999 0.844   0.508
#&gt; CV:NMF      5 0.917           0.957       0.963         0.1000 0.844   0.508
#&gt; MAD:NMF     5 0.985           0.980       0.978         0.1031 0.844   0.508
#&gt; ATC:NMF     5 0.565           0.690       0.763         0.0908 0.730   0.406
#&gt; SD:skmeans  5 0.917           0.890       0.907         0.1000 0.844   0.508
#&gt; CV:skmeans  5 0.875           0.869       0.916         0.1059 0.844   0.508
#&gt; MAD:skmeans 5 0.917           0.901       0.937         0.0968 0.844   0.508
#&gt; ATC:skmeans 5 0.821           0.885       0.885         0.1483 0.984   0.945
#&gt; SD:mclust   5 1.000           0.965       0.984         0.1017 0.927   0.736
#&gt; CV:mclust   5 0.908           0.945       0.962         0.1187 0.891   0.628
#&gt; MAD:mclust  5 1.000           1.000       1.000         0.1000 0.927   0.736
#&gt; ATC:mclust  5 0.748           0.875       0.872         0.1381 0.849   0.540
#&gt; SD:kmeans   5 0.655           0.749       0.773         0.0969 0.938   0.774
#&gt; CV:kmeans   5 0.646           0.786       0.766         0.1130 0.840   0.553
#&gt; MAD:kmeans  5 0.717           0.813       0.788         0.1133 0.927   0.736
#&gt; ATC:kmeans  5 0.645           0.742       0.763         0.1077 0.808   0.435
#&gt; SD:pam      5 1.000           0.983       0.992         0.0997 0.938   0.769
#&gt; CV:pam      5 0.981           0.934       0.970         0.1099 0.938   0.769
#&gt; MAD:pam     5 1.000           0.960       0.985         0.0912 0.938   0.769
#&gt; ATC:pam     5 0.989           0.945       0.973         0.0993 0.927   0.728
#&gt; SD:hclust   5 1.000           0.992       0.993         0.1051 0.927   0.736
#&gt; CV:hclust   5 0.886           0.969       0.973         0.2084 0.855   0.582
#&gt; MAD:hclust  5 1.000           1.000       1.000         0.1002 0.927   0.736
#&gt; ATC:hclust  5 1.000           0.975       0.985         0.0396 0.979   0.951
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.965           0.997       0.964         0.0445 0.958   0.795
#&gt; CV:NMF      6 1.000           0.996       0.946         0.0453 0.958   0.795
#&gt; MAD:NMF     6 1.000           0.999       0.991         0.0521 0.958   0.795
#&gt; ATC:NMF     6 0.697           0.826       0.819         0.0869 0.917   0.709
#&gt; SD:skmeans  6 1.000           0.990       0.977         0.0515 0.958   0.795
#&gt; CV:skmeans  6 1.000           0.989       0.979         0.0549 0.958   0.795
#&gt; MAD:skmeans 6 1.000           0.997       0.993         0.0518 0.958   0.795
#&gt; ATC:skmeans 6 0.911           0.816       0.751         0.0720 0.849   0.477
#&gt; SD:mclust   6 1.000           0.965       0.980         0.0454 0.969   0.846
#&gt; CV:mclust   6 0.933           0.975       0.975         0.0586 0.958   0.795
#&gt; MAD:mclust  6 1.000           1.000       1.000         0.0390 0.969   0.846
#&gt; ATC:mclust  6 0.872           0.869       0.898         0.0638 0.958   0.795
#&gt; SD:kmeans   6 0.738           0.909       0.778         0.0612 0.932   0.694
#&gt; CV:kmeans   6 0.708           0.901       0.748         0.0529 0.944   0.735
#&gt; MAD:kmeans  6 0.728           0.887       0.693         0.0495 0.958   0.795
#&gt; ATC:kmeans  6 0.681           0.725       0.754         0.0674 0.930   0.693
#&gt; SD:pam      6 1.000           1.000       1.000         0.0624 0.932   0.690
#&gt; CV:pam      6 1.000           1.000       1.000         0.0724 0.932   0.690
#&gt; MAD:pam     6 1.000           1.000       1.000         0.0560 0.932   0.690
#&gt; ATC:pam     6 1.000           0.963       0.985         0.0496 0.945   0.731
#&gt; SD:hclust   6 1.000           1.000       1.000         0.0521 0.958   0.795
#&gt; CV:hclust   6 0.933           0.970       0.969         0.0623 0.958   0.795
#&gt; MAD:hclust  6 1.000           1.000       1.000         0.0521 0.958   0.795
#&gt; ATC:hclust  6 1.000           0.984       0.993         0.2801 0.829   0.571
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>



 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2     1           1.000       1.000         0.2994 0.701   0.701
#> 3 3     1           1.000       1.000         0.6587 0.803   0.719
#> 4 4     1           0.991       0.993         0.4534 0.771   0.546
#> 5 5     1           0.992       0.993         0.1051 0.927   0.736
#> 6 6     1           1.000       1.000         0.0521 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3
#&gt; SRR1655002     1       0          1  1  0  0
#&gt; SRR1655003     1       0          1  1  0  0
#&gt; SRR1655004     1       0          1  1  0  0
#&gt; SRR1655005     1       0          1  1  0  0
#&gt; SRR1655006     1       0          1  1  0  0
#&gt; SRR1655007     1       0          1  1  0  0
#&gt; SRR1655008     1       0          1  1  0  0
#&gt; SRR1655009     1       0          1  1  0  0
#&gt; SRR1655010     1       0          1  1  0  0
#&gt; SRR1655011     1       0          1  1  0  0
#&gt; SRR1655012     2       0          1  0  1  0
#&gt; SRR1655013     2       0          1  0  1  0
#&gt; SRR1655014     2       0          1  0  1  0
#&gt; SRR1655015     2       0          1  0  1  0
#&gt; SRR1655016     2       0          1  0  1  0
#&gt; SRR1655017     2       0          1  0  1  0
#&gt; SRR1655018     2       0          1  0  1  0
#&gt; SRR1655019     2       0          1  0  1  0
#&gt; SRR1655020     2       0          1  0  1  0
#&gt; SRR1655021     2       0          1  0  1  0
#&gt; SRR1655022     2       0          1  0  1  0
#&gt; SRR1655023     2       0          1  0  1  0
#&gt; SRR1655024     2       0          1  0  1  0
#&gt; SRR1655025     2       0          1  0  1  0
#&gt; SRR1655026     2       0          1  0  1  0
#&gt; SRR1655027     2       0          1  0  1  0
#&gt; SRR1655028     2       0          1  0  1  0
#&gt; SRR1655029     2       0          1  0  1  0
#&gt; SRR1655030     2       0          1  0  1  0
#&gt; SRR1655031     2       0          1  0  1  0
#&gt; SRR1655032     2       0          1  0  1  0
#&gt; SRR1655033     2       0          1  0  1  0
#&gt; SRR1655034     3       0          1  0  0  1
#&gt; SRR1655035     3       0          1  0  0  1
#&gt; SRR1655036     3       0          1  0  0  1
#&gt; SRR1655037     3       0          1  0  0  1
#&gt; SRR1655038     3       0          1  0  0  1
#&gt; SRR1655039     3       0          1  0  0  1
#&gt; SRR1655040     3       0          1  0  0  1
#&gt; SRR1655041     3       0          1  0  0  1
#&gt; SRR1655042     2       0          1  0  1  0
#&gt; SRR1655043     2       0          1  0  1  0
#&gt; SRR1655044     2       0          1  0  1  0
#&gt; SRR1655045     2       0          1  0  1  0
#&gt; SRR1655046     2       0          1  0  1  0
#&gt; SRR1655047     2       0          1  0  1  0
#&gt; SRR1655048     2       0          1  0  1  0
#&gt; SRR1655049     2       0          1  0  1  0
#&gt; SRR1655050     2       0          1  0  1  0
#&gt; SRR1655051     2       0          1  0  1  0
#&gt; SRR1655052     2       0          1  0  1  0
#&gt; SRR1655053     2       0          1  0  1  0
#&gt; SRR1655054     2       0          1  0  1  0
#&gt; SRR1655055     2       0          1  0  1  0
#&gt; SRR1655056     2       0          1  0  1  0
#&gt; SRR1655057     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655013     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655014     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655015     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655016     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655017     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655018     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655019     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655020     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655021     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655022     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655023     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655024     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655025     2  0.0000      0.998  0 1.000  0 0.000
#&gt; SRR1655026     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655027     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655028     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655029     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655030     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655031     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655032     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655033     4  0.1302      0.970  0 0.044  0 0.956
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655043     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655044     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655045     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655046     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655047     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655048     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655049     2  0.0188      0.997  0 0.996  0 0.004
#&gt; SRR1655050     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655051     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655052     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655053     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655054     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655055     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655056     4  0.0000      0.970  0 0.000  0 1.000
#&gt; SRR1655057     4  0.0000      0.970  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3    p4    p5
#&gt; SRR1655002     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655003     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655004     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655005     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655006     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655007     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655008     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655009     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655010     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655011     1    0.00      1.000  1  0  0 0.000 0.000
#&gt; SRR1655012     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655013     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655014     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655015     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655016     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655017     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655018     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655019     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655020     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655021     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655022     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655023     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655024     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655025     2    0.00      1.000  0  1  0 0.000 0.000
#&gt; SRR1655026     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655027     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655028     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655029     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655030     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655031     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655032     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655033     5    0.12      0.973  0  0  0 0.048 0.952
#&gt; SRR1655034     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655035     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655036     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655037     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655038     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655039     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655040     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655041     3    0.00      1.000  0  0  1 0.000 0.000
#&gt; SRR1655042     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655043     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655044     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655045     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655046     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655047     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655048     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655049     4    0.00      1.000  0  0  0 1.000 0.000
#&gt; SRR1655050     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655051     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655052     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655053     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655054     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655055     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655056     5    0.00      0.974  0  0  0 0.000 1.000
#&gt; SRR1655057     5    0.00      0.974  0  0  0 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     6       0          1  0  0  0  0  0  1
#&gt; SRR1655049     6       0          1  0  0  0  0  0  1
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.997       0.996         0.2989 0.701   0.701
#> 3 3 0.532           0.737       0.839         0.9145 0.688   0.556
#> 4 4 0.556           0.773       0.765         0.2109 0.803   0.542
#> 5 5 0.655           0.749       0.773         0.0969 0.938   0.774
#> 6 6 0.738           0.909       0.778         0.0612 0.932   0.694
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655003     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655004     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655005     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655006     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655007     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655008     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655009     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655010     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655011     1  0.0938      1.000 0.988 0.012
#&gt; SRR1655012     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655013     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655014     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655015     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655016     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655017     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655018     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655019     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655020     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655021     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655022     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655023     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655024     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655025     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655026     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655027     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655028     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655029     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655030     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655031     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655032     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655033     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655034     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655035     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655036     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655037     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655038     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655039     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655040     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655041     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655042     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655043     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655044     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655045     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655046     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655047     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655048     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655049     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655050     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655051     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655052     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655053     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655054     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655055     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655056     2  0.0000      0.998 0.000 1.000
#&gt; SRR1655057     2  0.0000      0.998 0.000 1.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1  0.2066      0.972 0.940 0.000 0.060
#&gt; SRR1655003     1  0.0892      0.974 0.980 0.000 0.020
#&gt; SRR1655004     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1655005     1  0.0892      0.974 0.980 0.000 0.020
#&gt; SRR1655006     1  0.1964      0.973 0.944 0.000 0.056
#&gt; SRR1655007     1  0.1289      0.978 0.968 0.000 0.032
#&gt; SRR1655008     1  0.1163      0.976 0.972 0.000 0.028
#&gt; SRR1655009     1  0.1163      0.976 0.972 0.000 0.028
#&gt; SRR1655010     1  0.2356      0.973 0.928 0.000 0.072
#&gt; SRR1655011     1  0.2356      0.973 0.928 0.000 0.072
#&gt; SRR1655012     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655013     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655014     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655015     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655016     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655017     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655018     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655019     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655020     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655021     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655022     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655023     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655024     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655025     2  0.2878      0.796 0.000 0.904 0.096
#&gt; SRR1655026     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655027     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655028     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655029     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655030     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655031     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655032     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655033     2  0.6008      0.216 0.000 0.628 0.372
#&gt; SRR1655034     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655035     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655036     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655037     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655038     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655039     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655040     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655041     3  0.3267      0.800 0.000 0.116 0.884
#&gt; SRR1655042     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655043     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655044     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655045     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655046     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655047     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655048     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655049     2  0.0000      0.777 0.000 1.000 0.000
#&gt; SRR1655050     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655051     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655052     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655053     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655054     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655055     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655056     3  0.5905      0.757 0.000 0.352 0.648
#&gt; SRR1655057     3  0.5905      0.757 0.000 0.352 0.648
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1655002     1  0.2796      0.946 0.892 0.000 0.016 0.092
#&gt; SRR1655003     1  0.0469      0.954 0.988 0.000 0.000 0.012
#&gt; SRR1655004     1  0.0336      0.955 0.992 0.000 0.000 0.008
#&gt; SRR1655005     1  0.0469      0.954 0.988 0.000 0.000 0.012
#&gt; SRR1655006     1  0.2730      0.947 0.896 0.000 0.016 0.088
#&gt; SRR1655007     1  0.2385      0.955 0.920 0.000 0.028 0.052
#&gt; SRR1655008     1  0.1182      0.954 0.968 0.000 0.016 0.016
#&gt; SRR1655009     1  0.1182      0.954 0.968 0.000 0.016 0.016
#&gt; SRR1655010     1  0.3143      0.947 0.876 0.000 0.024 0.100
#&gt; SRR1655011     1  0.3143      0.947 0.876 0.000 0.024 0.100
#&gt; SRR1655012     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655013     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655014     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655015     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655016     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655017     2  0.0469      0.809 0.000 0.988 0.000 0.012
#&gt; SRR1655018     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.812 0.000 1.000 0.000 0.000
#&gt; SRR1655026     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655027     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655028     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655029     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655030     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655031     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655032     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655033     4  0.6552      0.697 0.000 0.228 0.144 0.628
#&gt; SRR1655034     3  0.1890      0.975 0.000 0.056 0.936 0.008
#&gt; SRR1655035     3  0.1890      0.975 0.000 0.056 0.936 0.008
#&gt; SRR1655036     3  0.2751      0.978 0.000 0.056 0.904 0.040
#&gt; SRR1655037     3  0.2751      0.978 0.000 0.056 0.904 0.040
#&gt; SRR1655038     3  0.1557      0.977 0.000 0.056 0.944 0.000
#&gt; SRR1655039     3  0.1557      0.977 0.000 0.056 0.944 0.000
#&gt; SRR1655040     3  0.2751      0.978 0.000 0.056 0.904 0.040
#&gt; SRR1655041     3  0.2751      0.978 0.000 0.056 0.904 0.040
#&gt; SRR1655042     2  0.5611      0.516 0.000 0.564 0.024 0.412
#&gt; SRR1655043     2  0.5611      0.516 0.000 0.564 0.024 0.412
#&gt; SRR1655044     2  0.5628      0.503 0.000 0.556 0.024 0.420
#&gt; SRR1655045     2  0.5628      0.503 0.000 0.556 0.024 0.420
#&gt; SRR1655046     2  0.5628      0.503 0.000 0.556 0.024 0.420
#&gt; SRR1655047     2  0.5628      0.503 0.000 0.556 0.024 0.420
#&gt; SRR1655048     2  0.5250      0.626 0.000 0.660 0.024 0.316
#&gt; SRR1655049     2  0.5250      0.626 0.000 0.660 0.024 0.316
#&gt; SRR1655050     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655051     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655052     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655053     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655054     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655055     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655056     4  0.6844      0.595 0.000 0.100 0.444 0.456
#&gt; SRR1655057     4  0.6844      0.595 0.000 0.100 0.444 0.456
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.2331      0.913 0.900 0.000 0.020 0.080 0.000
#&gt; SRR1655003     1  0.2796      0.922 0.868 0.000 0.008 0.116 0.008
#&gt; SRR1655004     1  0.2237      0.929 0.904 0.000 0.004 0.084 0.008
#&gt; SRR1655005     1  0.2796      0.922 0.868 0.000 0.008 0.116 0.008
#&gt; SRR1655006     1  0.2144      0.917 0.912 0.000 0.020 0.068 0.000
#&gt; SRR1655007     1  0.0404      0.928 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1655008     1  0.2830      0.924 0.884 0.000 0.016 0.080 0.020
#&gt; SRR1655009     1  0.2830      0.924 0.884 0.000 0.016 0.080 0.020
#&gt; SRR1655010     1  0.1901      0.918 0.928 0.000 0.012 0.056 0.004
#&gt; SRR1655011     1  0.1901      0.918 0.928 0.000 0.012 0.056 0.004
#&gt; SRR1655012     2  0.2835      0.786 0.000 0.868 0.016 0.112 0.004
#&gt; SRR1655013     2  0.2835      0.786 0.000 0.868 0.016 0.112 0.004
#&gt; SRR1655014     2  0.2886      0.784 0.000 0.864 0.016 0.116 0.004
#&gt; SRR1655015     2  0.2886      0.784 0.000 0.864 0.016 0.116 0.004
#&gt; SRR1655016     2  0.2835      0.786 0.000 0.868 0.016 0.112 0.004
#&gt; SRR1655017     2  0.2835      0.786 0.000 0.868 0.016 0.112 0.004
#&gt; SRR1655018     2  0.0566      0.807 0.000 0.984 0.000 0.004 0.012
#&gt; SRR1655019     2  0.0566      0.807 0.000 0.984 0.000 0.004 0.012
#&gt; SRR1655020     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655021     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655022     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655023     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655024     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655025     2  0.0404      0.808 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1655026     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655027     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655028     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655029     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655030     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655031     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655032     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655033     5  0.2127      0.551 0.000 0.108 0.000 0.000 0.892
#&gt; SRR1655034     3  0.2772      0.955 0.000 0.028 0.896 0.032 0.044
#&gt; SRR1655035     3  0.2772      0.955 0.000 0.028 0.896 0.032 0.044
#&gt; SRR1655036     3  0.2853      0.971 0.000 0.028 0.892 0.044 0.036
#&gt; SRR1655037     3  0.2853      0.971 0.000 0.028 0.892 0.044 0.036
#&gt; SRR1655038     3  0.1750      0.968 0.000 0.028 0.936 0.000 0.036
#&gt; SRR1655039     3  0.1750      0.968 0.000 0.028 0.936 0.000 0.036
#&gt; SRR1655040     3  0.2853      0.971 0.000 0.028 0.892 0.044 0.036
#&gt; SRR1655041     3  0.2853      0.971 0.000 0.028 0.892 0.044 0.036
#&gt; SRR1655042     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655043     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655044     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655045     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655046     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655047     4  0.6792      1.000 0.000 0.296 0.000 0.380 0.324
#&gt; SRR1655048     2  0.6856     -0.826 0.000 0.380 0.004 0.368 0.248
#&gt; SRR1655049     2  0.6856     -0.826 0.000 0.380 0.004 0.368 0.248
#&gt; SRR1655050     5  0.7145      0.634 0.000 0.044 0.204 0.244 0.508
#&gt; SRR1655051     5  0.7145      0.634 0.000 0.044 0.204 0.244 0.508
#&gt; SRR1655052     5  0.7163      0.635 0.000 0.044 0.204 0.248 0.504
#&gt; SRR1655053     5  0.7163      0.635 0.000 0.044 0.204 0.248 0.504
#&gt; SRR1655054     5  0.7107      0.635 0.000 0.044 0.204 0.236 0.516
#&gt; SRR1655055     5  0.7107      0.635 0.000 0.044 0.204 0.236 0.516
#&gt; SRR1655056     5  0.7163      0.635 0.000 0.044 0.204 0.248 0.504
#&gt; SRR1655057     5  0.7163      0.635 0.000 0.044 0.204 0.248 0.504
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1655002     1  0.3217      0.870 0.848 0.000 0.004 0.028 0.096 0.024
#&gt; SRR1655003     1  0.3901      0.888 0.812 0.000 0.008 0.044 0.096 0.040
#&gt; SRR1655004     1  0.3403      0.896 0.848 0.000 0.008 0.040 0.068 0.036
#&gt; SRR1655005     1  0.3901      0.888 0.812 0.000 0.008 0.044 0.096 0.040
#&gt; SRR1655006     1  0.2738      0.879 0.876 0.000 0.004 0.020 0.084 0.016
#&gt; SRR1655007     1  0.0405      0.895 0.988 0.000 0.000 0.000 0.004 0.008
#&gt; SRR1655008     1  0.3424      0.891 0.848 0.000 0.008 0.040 0.060 0.044
#&gt; SRR1655009     1  0.3424      0.891 0.848 0.000 0.008 0.040 0.060 0.044
#&gt; SRR1655010     1  0.2230      0.879 0.904 0.000 0.000 0.016 0.064 0.016
#&gt; SRR1655011     1  0.2230      0.879 0.904 0.000 0.000 0.016 0.064 0.016
#&gt; SRR1655012     2  0.3065      0.794 0.000 0.820 0.000 0.028 0.152 0.000
#&gt; SRR1655013     2  0.3065      0.794 0.000 0.820 0.000 0.028 0.152 0.000
#&gt; SRR1655014     2  0.3102      0.793 0.000 0.816 0.000 0.028 0.156 0.000
#&gt; SRR1655015     2  0.3102      0.793 0.000 0.816 0.000 0.028 0.156 0.000
#&gt; SRR1655016     2  0.3065      0.794 0.000 0.820 0.000 0.028 0.152 0.000
#&gt; SRR1655017     2  0.3065      0.794 0.000 0.820 0.000 0.028 0.152 0.000
#&gt; SRR1655018     2  0.2520      0.839 0.000 0.872 0.000 0.012 0.008 0.108
#&gt; SRR1655019     2  0.2520      0.839 0.000 0.872 0.000 0.012 0.008 0.108
#&gt; SRR1655020     2  0.2408      0.839 0.000 0.876 0.000 0.012 0.004 0.108
#&gt; SRR1655021     2  0.2408      0.839 0.000 0.876 0.000 0.012 0.004 0.108
#&gt; SRR1655022     2  0.2312      0.839 0.000 0.876 0.000 0.012 0.000 0.112
#&gt; SRR1655023     2  0.2312      0.839 0.000 0.876 0.000 0.012 0.000 0.112
#&gt; SRR1655024     2  0.2312      0.839 0.000 0.876 0.000 0.012 0.000 0.112
#&gt; SRR1655025     2  0.2312      0.839 0.000 0.876 0.000 0.012 0.000 0.112
#&gt; SRR1655026     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655027     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655028     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655029     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655030     4  0.3960      0.986 0.000 0.052 0.000 0.760 0.008 0.180
#&gt; SRR1655031     4  0.3960      0.986 0.000 0.052 0.000 0.760 0.008 0.180
#&gt; SRR1655032     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655033     4  0.3646      0.995 0.000 0.052 0.000 0.776 0.000 0.172
#&gt; SRR1655034     3  0.1887      0.912 0.000 0.008 0.932 0.020 0.016 0.024
#&gt; SRR1655035     3  0.1887      0.912 0.000 0.008 0.932 0.020 0.016 0.024
#&gt; SRR1655036     3  0.3226      0.929 0.000 0.008 0.860 0.048 0.052 0.032
#&gt; SRR1655037     3  0.3226      0.929 0.000 0.008 0.860 0.048 0.052 0.032
#&gt; SRR1655038     3  0.0767      0.925 0.000 0.008 0.976 0.004 0.000 0.012
#&gt; SRR1655039     3  0.0767      0.925 0.000 0.008 0.976 0.004 0.000 0.012
#&gt; SRR1655040     3  0.3134      0.929 0.000 0.008 0.864 0.048 0.056 0.024
#&gt; SRR1655041     3  0.3134      0.929 0.000 0.008 0.864 0.048 0.056 0.024
#&gt; SRR1655042     6  0.3354      0.959 0.000 0.128 0.000 0.060 0.000 0.812
#&gt; SRR1655043     6  0.3354      0.959 0.000 0.128 0.000 0.060 0.000 0.812
#&gt; SRR1655044     6  0.3370      0.959 0.000 0.124 0.000 0.064 0.000 0.812
#&gt; SRR1655045     6  0.3370      0.959 0.000 0.124 0.000 0.064 0.000 0.812
#&gt; SRR1655046     6  0.3370      0.959 0.000 0.124 0.000 0.064 0.000 0.812
#&gt; SRR1655047     6  0.3370      0.959 0.000 0.124 0.000 0.064 0.000 0.812
#&gt; SRR1655048     6  0.3521      0.889 0.000 0.156 0.000 0.004 0.044 0.796
#&gt; SRR1655049     6  0.3521      0.889 0.000 0.156 0.000 0.004 0.044 0.796
#&gt; SRR1655050     5  0.6260      0.953 0.000 0.008 0.124 0.376 0.464 0.028
#&gt; SRR1655051     5  0.6260      0.953 0.000 0.008 0.124 0.376 0.464 0.028
#&gt; SRR1655052     5  0.5991      0.970 0.000 0.008 0.124 0.312 0.536 0.020
#&gt; SRR1655053     5  0.5991      0.970 0.000 0.008 0.124 0.312 0.536 0.020
#&gt; SRR1655054     5  0.6209      0.967 0.000 0.008 0.124 0.344 0.496 0.028
#&gt; SRR1655055     5  0.6209      0.967 0.000 0.008 0.124 0.344 0.496 0.028
#&gt; SRR1655056     5  0.5991      0.970 0.000 0.008 0.124 0.312 0.536 0.020
#&gt; SRR1655057     5  0.5991      0.970 0.000 0.008 0.124 0.312 0.536 0.020
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.998       0.999         0.3004 0.701   0.701
#> 3 3 0.803           0.921       0.955         0.7594 0.803   0.719
#> 4 4 0.875           0.939       0.967         0.3709 0.771   0.546
#> 5 5 0.917           0.890       0.907         0.1000 0.844   0.508
#> 6 6 1.000           0.990       0.977         0.0515 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655003     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655004     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655005     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655006     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655007     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655008     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655009     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655010     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655011     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655012     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655013     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655014     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655015     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655016     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655017     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655018     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655019     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655020     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655021     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655022     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655023     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655024     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655025     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655026     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655027     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655028     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655029     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655030     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655031     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655032     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655033     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655034     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655035     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655036     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655037     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655038     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655039     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655040     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655041     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655042     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655043     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655044     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655045     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655046     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655047     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655048     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655049     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655050     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655051     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655052     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655053     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655054     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655055     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655056     2  0.0000      0.999 0.000 1.000
#&gt; SRR1655057     2  0.0000      0.999 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655012     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655013     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655014     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655015     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655016     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655017     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655018     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655019     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655020     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655021     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655022     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655023     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655024     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655025     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655026     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655027     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655028     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655029     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655030     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655031     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655032     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655033     2   0.175      0.915  0 0.952 0.048
#&gt; SRR1655034     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655042     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655043     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655044     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655045     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655046     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655047     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655048     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655049     2   0.000      0.928  0 1.000 0.000
#&gt; SRR1655050     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655051     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655052     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655053     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655054     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655055     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655056     2   0.529      0.729  0 0.732 0.268
#&gt; SRR1655057     2   0.529      0.729  0 0.732 0.268
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.904  0 1.000 0.000 0.000
#&gt; SRR1655026     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655027     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655028     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655029     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655030     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655031     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655032     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655033     4  0.0188      0.994  0 0.004 0.000 0.996
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655042     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655043     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655044     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655045     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655046     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655047     2  0.4277      0.708  0 0.720 0.000 0.280
#&gt; SRR1655048     2  0.0817      0.894  0 0.976 0.000 0.024
#&gt; SRR1655049     2  0.0817      0.894  0 0.976 0.000 0.024
#&gt; SRR1655050     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655051     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655052     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655053     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655054     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655055     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655056     4  0.0336      0.994  0 0.000 0.008 0.992
#&gt; SRR1655057     4  0.0336      0.994  0 0.000 0.008 0.992
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655012     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655013     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655014     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655015     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655016     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655017     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655018     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655019     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655020     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655021     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655022     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655023     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655024     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655025     2   0.000      1.000  0 1.000  0 0.000 0.000
#&gt; SRR1655026     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655027     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655028     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655029     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655030     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655031     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655032     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655033     4   0.443      0.534  0 0.004  0 0.540 0.456
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655042     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655043     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655044     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655045     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655046     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655047     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655048     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655049     4   0.051      0.694  0 0.016  0 0.984 0.000
#&gt; SRR1655050     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655051     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655052     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655053     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655054     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655055     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655056     5   0.000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655057     5   0.000      1.000  0 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655012     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655013     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655014     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655015     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655016     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655017     2  0.1926      0.955  0 0.912  0 0.000 0.068 0.020
#&gt; SRR1655018     2  0.0146      0.965  0 0.996  0 0.000 0.000 0.004
#&gt; SRR1655019     2  0.0146      0.965  0 0.996  0 0.000 0.000 0.004
#&gt; SRR1655020     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.966  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655026     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655027     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655028     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655029     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655030     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655031     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655032     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655033     4  0.0000      1.000  0 0.000  0 1.000 0.000 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655042     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655043     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655044     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655045     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655046     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655047     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655048     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655049     6  0.0632      1.000  0 0.000  0 0.024 0.000 0.976
#&gt; SRR1655050     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655051     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655052     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655053     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655054     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655055     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655056     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
#&gt; SRR1655057     5  0.1387      1.000  0 0.000  0 0.068 0.932 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 1.000           1.000       1.000         0.6587 0.803   0.719
#> 4 4 0.809           0.940       0.956         0.4463 0.766   0.536
#> 5 5 1.000           0.983       0.992         0.0997 0.938   0.769
#> 6 6 1.000           1.000       1.000         0.0624 0.932   0.690
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3
#&gt; SRR1655002     1       0          1  1  0  0
#&gt; SRR1655003     1       0          1  1  0  0
#&gt; SRR1655004     1       0          1  1  0  0
#&gt; SRR1655005     1       0          1  1  0  0
#&gt; SRR1655006     1       0          1  1  0  0
#&gt; SRR1655007     1       0          1  1  0  0
#&gt; SRR1655008     1       0          1  1  0  0
#&gt; SRR1655009     1       0          1  1  0  0
#&gt; SRR1655010     1       0          1  1  0  0
#&gt; SRR1655011     1       0          1  1  0  0
#&gt; SRR1655012     2       0          1  0  1  0
#&gt; SRR1655013     2       0          1  0  1  0
#&gt; SRR1655014     2       0          1  0  1  0
#&gt; SRR1655015     2       0          1  0  1  0
#&gt; SRR1655016     2       0          1  0  1  0
#&gt; SRR1655017     2       0          1  0  1  0
#&gt; SRR1655018     2       0          1  0  1  0
#&gt; SRR1655019     2       0          1  0  1  0
#&gt; SRR1655020     2       0          1  0  1  0
#&gt; SRR1655021     2       0          1  0  1  0
#&gt; SRR1655022     2       0          1  0  1  0
#&gt; SRR1655023     2       0          1  0  1  0
#&gt; SRR1655024     2       0          1  0  1  0
#&gt; SRR1655025     2       0          1  0  1  0
#&gt; SRR1655026     2       0          1  0  1  0
#&gt; SRR1655027     2       0          1  0  1  0
#&gt; SRR1655028     2       0          1  0  1  0
#&gt; SRR1655029     2       0          1  0  1  0
#&gt; SRR1655030     2       0          1  0  1  0
#&gt; SRR1655031     2       0          1  0  1  0
#&gt; SRR1655032     2       0          1  0  1  0
#&gt; SRR1655033     2       0          1  0  1  0
#&gt; SRR1655034     3       0          1  0  0  1
#&gt; SRR1655035     3       0          1  0  0  1
#&gt; SRR1655036     3       0          1  0  0  1
#&gt; SRR1655037     3       0          1  0  0  1
#&gt; SRR1655038     3       0          1  0  0  1
#&gt; SRR1655039     3       0          1  0  0  1
#&gt; SRR1655040     3       0          1  0  0  1
#&gt; SRR1655041     3       0          1  0  0  1
#&gt; SRR1655042     2       0          1  0  1  0
#&gt; SRR1655043     2       0          1  0  1  0
#&gt; SRR1655044     2       0          1  0  1  0
#&gt; SRR1655045     2       0          1  0  1  0
#&gt; SRR1655046     2       0          1  0  1  0
#&gt; SRR1655047     2       0          1  0  1  0
#&gt; SRR1655048     2       0          1  0  1  0
#&gt; SRR1655049     2       0          1  0  1  0
#&gt; SRR1655050     2       0          1  0  1  0
#&gt; SRR1655051     2       0          1  0  1  0
#&gt; SRR1655052     2       0          1  0  1  0
#&gt; SRR1655053     2       0          1  0  1  0
#&gt; SRR1655054     2       0          1  0  1  0
#&gt; SRR1655055     2       0          1  0  1  0
#&gt; SRR1655056     2       0          1  0  1  0
#&gt; SRR1655057     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655013     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655014     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655015     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655016     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655017     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655018     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655019     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655020     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655021     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655022     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655023     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655024     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655025     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655026     4   0.349      0.852  0 0.188  0 0.812
#&gt; SRR1655027     4   0.302      0.864  0 0.148  0 0.852
#&gt; SRR1655028     4   0.201      0.864  0 0.080  0 0.920
#&gt; SRR1655029     4   0.228      0.866  0 0.096  0 0.904
#&gt; SRR1655030     4   0.391      0.825  0 0.232  0 0.768
#&gt; SRR1655031     4   0.398      0.818  0 0.240  0 0.760
#&gt; SRR1655032     4   0.287      0.865  0 0.136  0 0.864
#&gt; SRR1655033     4   0.344      0.854  0 0.184  0 0.816
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655043     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655044     4   0.436      0.770  0 0.292  0 0.708
#&gt; SRR1655045     4   0.436      0.770  0 0.292  0 0.708
#&gt; SRR1655046     4   0.436      0.770  0 0.292  0 0.708
#&gt; SRR1655047     4   0.436      0.770  0 0.292  0 0.708
#&gt; SRR1655048     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655049     2   0.000      1.000  0 1.000  0 0.000
#&gt; SRR1655050     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655051     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655052     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655053     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655054     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655055     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655056     4   0.000      0.847  0 0.000  0 1.000
#&gt; SRR1655057     4   0.000      0.847  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655013     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655014     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655015     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655016     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655017     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655018     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655019     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655020     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655021     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655022     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655023     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655024     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655025     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655026     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655027     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655028     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655029     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655030     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655031     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655032     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655033     4   0.000      0.949  0 0.000  0 1.000  0
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655043     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655044     4   0.112      0.928  0 0.044  0 0.956  0
#&gt; SRR1655045     4   0.112      0.928  0 0.044  0 0.956  0
#&gt; SRR1655046     4   0.293      0.799  0 0.180  0 0.820  0
#&gt; SRR1655047     4   0.300      0.790  0 0.188  0 0.812  0
#&gt; SRR1655048     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655049     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655050     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5   0.000      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     6       0          1  0  0  0  0  0  1
#&gt; SRR1655049     6       0          1  0  0  0  0  0  1
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.751           0.862       0.941         0.3592 0.701   0.701
#> 3 3 0.575           0.897       0.904         0.5793 0.735   0.622
#> 4 4 1.000           0.971       0.986         0.2742 0.777   0.522
#> 5 5 1.000           0.965       0.984         0.1017 0.927   0.736
#> 6 6 1.000           0.965       0.980         0.0454 0.969   0.846
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 4 5
```

There is also optional best $k$ = 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.000      1.000 1.000 0.000
#&gt; SRR1655003     1   0.000      1.000 1.000 0.000
#&gt; SRR1655004     1   0.000      1.000 1.000 0.000
#&gt; SRR1655005     1   0.000      1.000 1.000 0.000
#&gt; SRR1655006     1   0.000      1.000 1.000 0.000
#&gt; SRR1655007     1   0.000      1.000 1.000 0.000
#&gt; SRR1655008     1   0.000      1.000 1.000 0.000
#&gt; SRR1655009     1   0.000      1.000 1.000 0.000
#&gt; SRR1655010     1   0.000      1.000 1.000 0.000
#&gt; SRR1655011     1   0.000      1.000 1.000 0.000
#&gt; SRR1655012     2   0.000      0.921 0.000 1.000
#&gt; SRR1655013     2   0.000      0.921 0.000 1.000
#&gt; SRR1655014     2   0.000      0.921 0.000 1.000
#&gt; SRR1655015     2   0.000      0.921 0.000 1.000
#&gt; SRR1655016     2   0.000      0.921 0.000 1.000
#&gt; SRR1655017     2   0.000      0.921 0.000 1.000
#&gt; SRR1655018     2   0.000      0.921 0.000 1.000
#&gt; SRR1655019     2   0.000      0.921 0.000 1.000
#&gt; SRR1655020     2   0.000      0.921 0.000 1.000
#&gt; SRR1655021     2   0.000      0.921 0.000 1.000
#&gt; SRR1655022     2   0.000      0.921 0.000 1.000
#&gt; SRR1655023     2   0.000      0.921 0.000 1.000
#&gt; SRR1655024     2   0.000      0.921 0.000 1.000
#&gt; SRR1655025     2   0.000      0.921 0.000 1.000
#&gt; SRR1655026     2   0.000      0.921 0.000 1.000
#&gt; SRR1655027     2   0.000      0.921 0.000 1.000
#&gt; SRR1655028     2   0.000      0.921 0.000 1.000
#&gt; SRR1655029     2   0.000      0.921 0.000 1.000
#&gt; SRR1655030     2   0.000      0.921 0.000 1.000
#&gt; SRR1655031     2   0.000      0.921 0.000 1.000
#&gt; SRR1655032     2   0.000      0.921 0.000 1.000
#&gt; SRR1655033     2   0.000      0.921 0.000 1.000
#&gt; SRR1655034     2   0.978      0.408 0.412 0.588
#&gt; SRR1655035     2   0.978      0.408 0.412 0.588
#&gt; SRR1655036     2   0.978      0.408 0.412 0.588
#&gt; SRR1655037     2   0.978      0.408 0.412 0.588
#&gt; SRR1655038     2   0.978      0.408 0.412 0.588
#&gt; SRR1655039     2   0.978      0.408 0.412 0.588
#&gt; SRR1655040     2   0.978      0.408 0.412 0.588
#&gt; SRR1655041     2   0.978      0.408 0.412 0.588
#&gt; SRR1655042     2   0.000      0.921 0.000 1.000
#&gt; SRR1655043     2   0.000      0.921 0.000 1.000
#&gt; SRR1655044     2   0.000      0.921 0.000 1.000
#&gt; SRR1655045     2   0.000      0.921 0.000 1.000
#&gt; SRR1655046     2   0.000      0.921 0.000 1.000
#&gt; SRR1655047     2   0.000      0.921 0.000 1.000
#&gt; SRR1655048     2   0.000      0.921 0.000 1.000
#&gt; SRR1655049     2   0.000      0.921 0.000 1.000
#&gt; SRR1655050     2   0.000      0.921 0.000 1.000
#&gt; SRR1655051     2   0.000      0.921 0.000 1.000
#&gt; SRR1655052     2   0.000      0.921 0.000 1.000
#&gt; SRR1655053     2   0.000      0.921 0.000 1.000
#&gt; SRR1655054     2   0.000      0.921 0.000 1.000
#&gt; SRR1655055     2   0.000      0.921 0.000 1.000
#&gt; SRR1655056     2   0.000      0.921 0.000 1.000
#&gt; SRR1655057     2   0.000      0.921 0.000 1.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     3  0.3551      0.986  0 0.132 0.868
#&gt; SRR1655013     3  0.3551      0.986  0 0.132 0.868
#&gt; SRR1655014     2  0.4842      0.751  0 0.776 0.224
#&gt; SRR1655015     2  0.4842      0.751  0 0.776 0.224
#&gt; SRR1655016     3  0.3551      0.986  0 0.132 0.868
#&gt; SRR1655017     3  0.3551      0.986  0 0.132 0.868
#&gt; SRR1655018     3  0.3941      0.973  0 0.156 0.844
#&gt; SRR1655019     3  0.3941      0.973  0 0.156 0.844
#&gt; SRR1655020     3  0.3686      0.989  0 0.140 0.860
#&gt; SRR1655021     3  0.3686      0.989  0 0.140 0.860
#&gt; SRR1655022     3  0.3752      0.989  0 0.144 0.856
#&gt; SRR1655023     3  0.3752      0.989  0 0.144 0.856
#&gt; SRR1655024     3  0.3752      0.989  0 0.144 0.856
#&gt; SRR1655025     3  0.3752      0.989  0 0.144 0.856
#&gt; SRR1655026     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655027     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655028     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655029     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655030     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655031     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655032     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655033     2  0.1753      0.878  0 0.952 0.048
#&gt; SRR1655034     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655035     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655036     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655037     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655038     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655039     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655040     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655041     2  0.3551      0.809  0 0.868 0.132
#&gt; SRR1655042     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655043     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655044     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655045     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655046     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655047     2  0.4178      0.804  0 0.828 0.172
#&gt; SRR1655048     2  0.4346      0.791  0 0.816 0.184
#&gt; SRR1655049     2  0.4346      0.791  0 0.816 0.184
#&gt; SRR1655050     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655051     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655052     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655053     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655054     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655055     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655056     2  0.0237      0.878  0 0.996 0.004
#&gt; SRR1655057     2  0.0237      0.878  0 0.996 0.004
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655013     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655014     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655015     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655016     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655017     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655018     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655019     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655020     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655021     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655022     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655023     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655024     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655025     2   0.000      0.967  0 1.000  0 0.000
#&gt; SRR1655026     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655027     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655028     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655029     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655030     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655031     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655032     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655033     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655043     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655044     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655045     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655046     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655047     4   0.172      0.943  0 0.064  0 0.936
#&gt; SRR1655048     2   0.357      0.748  0 0.804  0 0.196
#&gt; SRR1655049     2   0.357      0.748  0 0.804  0 0.196
#&gt; SRR1655050     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655051     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655052     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655053     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655054     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655055     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655056     4   0.000      0.979  0 0.000  0 1.000
#&gt; SRR1655057     4   0.000      0.979  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2  0.0609      0.937  0 0.980  0 0.020  0
#&gt; SRR1655013     2  0.0609      0.937  0 0.980  0 0.020  0
#&gt; SRR1655014     2  0.0794      0.933  0 0.972  0 0.028  0
#&gt; SRR1655015     2  0.0794      0.933  0 0.972  0 0.028  0
#&gt; SRR1655016     2  0.0609      0.937  0 0.980  0 0.020  0
#&gt; SRR1655017     2  0.0609      0.937  0 0.980  0 0.020  0
#&gt; SRR1655018     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655019     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655020     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655021     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655022     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655023     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655024     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655025     2  0.0000      0.938  0 1.000  0 0.000  0
#&gt; SRR1655026     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655027     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655028     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655029     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655030     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655031     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655032     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655033     4  0.0000      0.991  0 0.000  0 1.000  0
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655043     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655044     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655045     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655046     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655047     4  0.0510      0.989  0 0.016  0 0.984  0
#&gt; SRR1655048     2  0.3999      0.524  0 0.656  0 0.344  0
#&gt; SRR1655049     2  0.3999      0.524  0 0.656  0 0.344  0
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655012     2  0.0547      0.928  0 0.980  0 0.000  0 0.020
#&gt; SRR1655013     2  0.0547      0.928  0 0.980  0 0.000  0 0.020
#&gt; SRR1655014     2  0.0790      0.925  0 0.968  0 0.000  0 0.032
#&gt; SRR1655015     2  0.0790      0.925  0 0.968  0 0.000  0 0.032
#&gt; SRR1655016     2  0.0547      0.928  0 0.980  0 0.000  0 0.020
#&gt; SRR1655017     2  0.0547      0.928  0 0.980  0 0.000  0 0.020
#&gt; SRR1655018     2  0.0865      0.928  0 0.964  0 0.000  0 0.036
#&gt; SRR1655019     2  0.0865      0.928  0 0.964  0 0.000  0 0.036
#&gt; SRR1655020     2  0.0146      0.926  0 0.996  0 0.000  0 0.004
#&gt; SRR1655021     2  0.0146      0.926  0 0.996  0 0.000  0 0.004
#&gt; SRR1655022     2  0.0937      0.927  0 0.960  0 0.000  0 0.040
#&gt; SRR1655023     2  0.0937      0.927  0 0.960  0 0.000  0 0.040
#&gt; SRR1655024     2  0.0937      0.927  0 0.960  0 0.000  0 0.040
#&gt; SRR1655025     2  0.0937      0.927  0 0.960  0 0.000  0 0.040
#&gt; SRR1655026     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655027     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655028     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655029     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655030     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655031     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655032     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655033     4  0.0000      1.000  0 0.000  0 1.000  0 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655042     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655043     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655044     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655045     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655046     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655047     6  0.0146      1.000  0 0.000  0 0.004  0 0.996
#&gt; SRR1655048     2  0.3672      0.519  0 0.632  0 0.000  0 0.368
#&gt; SRR1655049     2  0.3672      0.519  0 0.632  0 0.000  0 0.368
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.862           0.892       0.950         0.7953 0.803   0.719
#> 4 4 1.000           0.985       0.988         0.3387 0.771   0.546
#> 5 5 0.942           0.935       0.938         0.0999 0.844   0.508
#> 6 6 0.965           0.997       0.964         0.0445 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4 5
```

There is also optional best $k$ = 2 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655012     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655013     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655014     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655015     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655016     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655017     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655018     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655019     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655020     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655021     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655022     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655023     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655024     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655025     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655026     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655027     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655028     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655029     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655030     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655031     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655032     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655033     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655034     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655042     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655043     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655044     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655045     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655046     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655047     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655048     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655049     2   0.000      0.919  0 1.000 0.000
#&gt; SRR1655050     2   0.610      0.479  0 0.608 0.392
#&gt; SRR1655051     2   0.613      0.462  0 0.600 0.400
#&gt; SRR1655052     2   0.571      0.603  0 0.680 0.320
#&gt; SRR1655053     2   0.571      0.603  0 0.680 0.320
#&gt; SRR1655054     2   0.573      0.598  0 0.676 0.324
#&gt; SRR1655055     2   0.568      0.609  0 0.684 0.316
#&gt; SRR1655056     2   0.606      0.495  0 0.616 0.384
#&gt; SRR1655057     2   0.590      0.553  0 0.648 0.352
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655026     4  0.1867      0.949  0 0.072 0.000 0.928
#&gt; SRR1655027     4  0.1792      0.950  0 0.068 0.000 0.932
#&gt; SRR1655028     4  0.1792      0.950  0 0.068 0.000 0.932
#&gt; SRR1655029     4  0.1792      0.950  0 0.068 0.000 0.932
#&gt; SRR1655030     4  0.1940      0.946  0 0.076 0.000 0.924
#&gt; SRR1655031     4  0.1867      0.949  0 0.072 0.000 0.928
#&gt; SRR1655032     4  0.1940      0.946  0 0.076 0.000 0.924
#&gt; SRR1655033     4  0.1940      0.946  0 0.076 0.000 0.924
#&gt; SRR1655034     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655035     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655036     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655037     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655038     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655039     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655040     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655041     3  0.0336      1.000  0 0.000 0.992 0.008
#&gt; SRR1655042     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655043     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655044     2  0.0188      0.995  0 0.996 0.000 0.004
#&gt; SRR1655045     2  0.0336      0.991  0 0.992 0.000 0.008
#&gt; SRR1655046     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655047     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655048     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655049     2  0.0000      0.999  0 1.000 0.000 0.000
#&gt; SRR1655050     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655051     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655052     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655053     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655054     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655055     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655056     4  0.0000      0.951  0 0.000 0.000 1.000
#&gt; SRR1655057     4  0.0000      0.951  0 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655012     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655013     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655014     2  0.0000      0.987  0 1.000  0 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.987  0 1.000  0 0.000 0.000
#&gt; SRR1655016     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655017     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655018     2  0.0162      0.990  0 0.996  0 0.004 0.000
#&gt; SRR1655019     2  0.0162      0.990  0 0.996  0 0.004 0.000
#&gt; SRR1655020     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655021     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655022     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655023     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655024     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655025     2  0.0510      0.996  0 0.984  0 0.016 0.000
#&gt; SRR1655026     4  0.4541      0.748  0 0.032  0 0.680 0.288
#&gt; SRR1655027     4  0.4562      0.744  0 0.032  0 0.676 0.292
#&gt; SRR1655028     4  0.4498      0.753  0 0.032  0 0.688 0.280
#&gt; SRR1655029     4  0.4541      0.748  0 0.032  0 0.680 0.288
#&gt; SRR1655030     4  0.4661      0.722  0 0.032  0 0.656 0.312
#&gt; SRR1655031     4  0.4661      0.722  0 0.032  0 0.656 0.312
#&gt; SRR1655032     4  0.4302      0.768  0 0.032  0 0.720 0.248
#&gt; SRR1655033     4  0.4248      0.770  0 0.032  0 0.728 0.240
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655042     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655043     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655044     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655045     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655046     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655047     4  0.1608      0.809  0 0.072  0 0.928 0.000
#&gt; SRR1655048     4  0.1671      0.807  0 0.076  0 0.924 0.000
#&gt; SRR1655049     4  0.1671      0.807  0 0.076  0 0.924 0.000
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1655002     1  0.0146      0.998 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1655003     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0146      0.998 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1655007     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0146      0.998 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1655011     1  0.0146      0.998 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1655012     2  0.0291      0.995 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655013     2  0.0291      0.995 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655014     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0508      0.988 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; SRR1655017     2  0.0405      0.992 0.000 0.988 0.000 0.008 0.000 0.004
#&gt; SRR1655018     2  0.0146      0.992 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1655019     2  0.0146      0.992 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1655020     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655021     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655022     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655023     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655024     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655025     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1655026     4  0.2474      0.998 0.000 0.080 0.000 0.880 0.040 0.000
#&gt; SRR1655027     4  0.2474      0.998 0.000 0.080 0.000 0.880 0.040 0.000
#&gt; SRR1655028     4  0.2474      0.998 0.000 0.080 0.000 0.880 0.040 0.000
#&gt; SRR1655029     4  0.2474      0.998 0.000 0.080 0.000 0.880 0.040 0.000
#&gt; SRR1655030     4  0.2474      0.998 0.000 0.080 0.000 0.880 0.040 0.000
#&gt; SRR1655031     4  0.2488      0.994 0.000 0.076 0.000 0.880 0.044 0.000
#&gt; SRR1655032     4  0.2617      0.996 0.000 0.080 0.000 0.876 0.040 0.004
#&gt; SRR1655033     4  0.2617      0.996 0.000 0.080 0.000 0.876 0.040 0.004
#&gt; SRR1655034     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655036     3  0.0291      0.995 0.000 0.000 0.992 0.004 0.000 0.004
#&gt; SRR1655037     3  0.0291      0.995 0.000 0.000 0.992 0.004 0.000 0.004
#&gt; SRR1655038     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      0.998 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1655042     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655043     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655044     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655045     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655046     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655047     6  0.2218      0.997 0.000 0.012 0.000 0.104 0.000 0.884
#&gt; SRR1655048     6  0.2214      0.992 0.000 0.016 0.000 0.096 0.000 0.888
#&gt; SRR1655049     6  0.2214      0.992 0.000 0.016 0.000 0.096 0.000 0.888
#&gt; SRR1655050     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655051     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655052     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655053     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655054     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655055     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655056     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655057     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.803           0.957       0.977         0.7209 0.803   0.719
#> 4 4 0.855           0.977       0.973         0.2654 0.844   0.691
#> 5 5 0.886           0.969       0.973         0.2084 0.855   0.582
#> 6 6 0.933           0.970       0.969         0.0623 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655012     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655013     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655014     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655015     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655016     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655017     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655018     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655019     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655020     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655021     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655022     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655023     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655024     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655025     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655026     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655027     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655028     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655029     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655030     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655031     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655032     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655033     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655034     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000 1.000
#&gt; SRR1655042     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655043     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655044     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655045     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655046     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655047     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655048     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655049     2   0.000      0.963  0 1.000 0.000
#&gt; SRR1655050     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655051     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655052     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655053     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655054     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655055     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655056     2   0.406      0.841  0 0.836 0.164
#&gt; SRR1655057     2   0.406      0.841  0 0.836 0.164
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655013     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655014     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655015     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655016     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655017     2   0.187      0.919  0 0.928  0 0.072
#&gt; SRR1655018     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655019     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655020     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655021     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655022     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655023     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655024     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655025     2   0.000      0.964  0 1.000  0 0.000
#&gt; SRR1655026     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655027     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655028     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655029     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655030     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655031     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655032     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655033     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655043     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655044     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655045     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655046     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655047     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655048     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655049     2   0.102      0.968  0 0.968  0 0.032
#&gt; SRR1655050     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655051     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655052     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655053     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655054     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655055     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655056     4   0.187      1.000  0 0.072  0 0.928
#&gt; SRR1655057     4   0.187      1.000  0 0.072  0 0.928
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1     0.0      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655013     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655014     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655015     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655016     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655017     2     0.0      0.856  0 1.000  0 0.000  0
#&gt; SRR1655018     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655019     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655020     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655021     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655022     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655023     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655024     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655025     2     0.3      0.893  0 0.812  0 0.188  0
#&gt; SRR1655026     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655027     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655028     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655029     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655030     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655031     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655032     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655033     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655034     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3     0.0      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655043     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655044     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655045     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655046     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655047     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655048     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655049     4     0.0      1.000  0 0.000  0 1.000  0
#&gt; SRR1655050     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5     0.0      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5     0.0      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655012     2  0.0000      0.868  0 1.000  0 0.000  0 0.000
#&gt; SRR1655013     2  0.0000      0.868  0 1.000  0 0.000  0 0.000
#&gt; SRR1655014     2  0.0363      0.863  0 0.988  0 0.012  0 0.000
#&gt; SRR1655015     2  0.0363      0.863  0 0.988  0 0.012  0 0.000
#&gt; SRR1655016     2  0.0000      0.868  0 1.000  0 0.000  0 0.000
#&gt; SRR1655017     2  0.0000      0.868  0 1.000  0 0.000  0 0.000
#&gt; SRR1655018     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655019     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655020     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655021     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655022     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655023     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655024     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655025     2  0.2805      0.894  0 0.812  0 0.004  0 0.184
#&gt; SRR1655026     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655027     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655028     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655029     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655030     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655031     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655032     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655033     4  0.0363      1.000  0 0.000  0 0.988  0 0.012
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655042     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655043     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655044     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655045     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655046     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655047     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655048     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655049     6  0.0000      1.000  0 0.000  0 0.000  0 1.000
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.983       0.979         0.2963 0.701   0.701
#> 3 3 0.460           0.739       0.813         0.8931 0.688   0.556
#> 4 4 0.490           0.653       0.724         0.2081 0.958   0.893
#> 5 5 0.646           0.786       0.766         0.1130 0.840   0.553
#> 6 6 0.708           0.901       0.748         0.0529 0.944   0.735
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.343      1.000 0.936 0.064
#&gt; SRR1655003     1   0.343      1.000 0.936 0.064
#&gt; SRR1655004     1   0.343      1.000 0.936 0.064
#&gt; SRR1655005     1   0.343      1.000 0.936 0.064
#&gt; SRR1655006     1   0.343      1.000 0.936 0.064
#&gt; SRR1655007     1   0.343      1.000 0.936 0.064
#&gt; SRR1655008     1   0.343      1.000 0.936 0.064
#&gt; SRR1655009     1   0.343      1.000 0.936 0.064
#&gt; SRR1655010     1   0.343      1.000 0.936 0.064
#&gt; SRR1655011     1   0.343      1.000 0.936 0.064
#&gt; SRR1655012     2   0.000      0.988 0.000 1.000
#&gt; SRR1655013     2   0.000      0.988 0.000 1.000
#&gt; SRR1655014     2   0.000      0.988 0.000 1.000
#&gt; SRR1655015     2   0.000      0.988 0.000 1.000
#&gt; SRR1655016     2   0.000      0.988 0.000 1.000
#&gt; SRR1655017     2   0.000      0.988 0.000 1.000
#&gt; SRR1655018     2   0.000      0.988 0.000 1.000
#&gt; SRR1655019     2   0.000      0.988 0.000 1.000
#&gt; SRR1655020     2   0.000      0.988 0.000 1.000
#&gt; SRR1655021     2   0.000      0.988 0.000 1.000
#&gt; SRR1655022     2   0.000      0.988 0.000 1.000
#&gt; SRR1655023     2   0.000      0.988 0.000 1.000
#&gt; SRR1655024     2   0.000      0.988 0.000 1.000
#&gt; SRR1655025     2   0.000      0.988 0.000 1.000
#&gt; SRR1655026     2   0.000      0.988 0.000 1.000
#&gt; SRR1655027     2   0.000      0.988 0.000 1.000
#&gt; SRR1655028     2   0.000      0.988 0.000 1.000
#&gt; SRR1655029     2   0.000      0.988 0.000 1.000
#&gt; SRR1655030     2   0.000      0.988 0.000 1.000
#&gt; SRR1655031     2   0.000      0.988 0.000 1.000
#&gt; SRR1655032     2   0.000      0.988 0.000 1.000
#&gt; SRR1655033     2   0.000      0.988 0.000 1.000
#&gt; SRR1655034     2   0.343      0.941 0.064 0.936
#&gt; SRR1655035     2   0.343      0.941 0.064 0.936
#&gt; SRR1655036     2   0.343      0.941 0.064 0.936
#&gt; SRR1655037     2   0.343      0.941 0.064 0.936
#&gt; SRR1655038     2   0.343      0.941 0.064 0.936
#&gt; SRR1655039     2   0.343      0.941 0.064 0.936
#&gt; SRR1655040     2   0.343      0.941 0.064 0.936
#&gt; SRR1655041     2   0.343      0.941 0.064 0.936
#&gt; SRR1655042     2   0.000      0.988 0.000 1.000
#&gt; SRR1655043     2   0.000      0.988 0.000 1.000
#&gt; SRR1655044     2   0.000      0.988 0.000 1.000
#&gt; SRR1655045     2   0.000      0.988 0.000 1.000
#&gt; SRR1655046     2   0.000      0.988 0.000 1.000
#&gt; SRR1655047     2   0.000      0.988 0.000 1.000
#&gt; SRR1655048     2   0.000      0.988 0.000 1.000
#&gt; SRR1655049     2   0.000      0.988 0.000 1.000
#&gt; SRR1655050     2   0.000      0.988 0.000 1.000
#&gt; SRR1655051     2   0.000      0.988 0.000 1.000
#&gt; SRR1655052     2   0.000      0.988 0.000 1.000
#&gt; SRR1655053     2   0.000      0.988 0.000 1.000
#&gt; SRR1655054     2   0.000      0.988 0.000 1.000
#&gt; SRR1655055     2   0.000      0.988 0.000 1.000
#&gt; SRR1655056     2   0.000      0.988 0.000 1.000
#&gt; SRR1655057     2   0.000      0.988 0.000 1.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1655003     1  0.1860      0.980 0.948 0.000 0.052
#&gt; SRR1655004     1  0.0892      0.984 0.980 0.000 0.020
#&gt; SRR1655005     1  0.1753      0.981 0.952 0.000 0.048
#&gt; SRR1655006     1  0.0000      0.984 1.000 0.000 0.000
#&gt; SRR1655007     1  0.1411      0.984 0.964 0.000 0.036
#&gt; SRR1655008     1  0.1753      0.985 0.952 0.000 0.048
#&gt; SRR1655009     1  0.2066      0.982 0.940 0.000 0.060
#&gt; SRR1655010     1  0.0747      0.984 0.984 0.000 0.016
#&gt; SRR1655011     1  0.0747      0.984 0.984 0.000 0.016
#&gt; SRR1655012     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655013     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655014     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655015     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655016     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655017     2  0.0424      0.772 0.000 0.992 0.008
#&gt; SRR1655018     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655019     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655020     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655021     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655022     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655023     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655024     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655025     2  0.0237      0.774 0.000 0.996 0.004
#&gt; SRR1655026     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655027     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655028     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655029     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655030     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655031     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655032     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655033     2  0.5968      0.481 0.000 0.636 0.364
#&gt; SRR1655034     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655035     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655036     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655037     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655038     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655039     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655040     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655041     3  0.3686      0.738 0.000 0.140 0.860
#&gt; SRR1655042     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655043     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655044     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655045     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655046     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655047     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655048     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655049     2  0.4235      0.748 0.000 0.824 0.176
#&gt; SRR1655050     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655051     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655052     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655053     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655054     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655055     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655056     3  0.5926      0.621 0.000 0.356 0.644
#&gt; SRR1655057     3  0.5926      0.621 0.000 0.356 0.644
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1655002     1  0.1792      0.950 0.932 0.000 0.000 0.068
#&gt; SRR1655003     1  0.3335      0.944 0.856 0.000 0.016 0.128
#&gt; SRR1655004     1  0.2611      0.951 0.896 0.000 0.008 0.096
#&gt; SRR1655005     1  0.3335      0.944 0.856 0.000 0.016 0.128
#&gt; SRR1655006     1  0.1792      0.950 0.932 0.000 0.000 0.068
#&gt; SRR1655007     1  0.0927      0.949 0.976 0.000 0.016 0.008
#&gt; SRR1655008     1  0.2197      0.947 0.928 0.000 0.024 0.048
#&gt; SRR1655009     1  0.2466      0.945 0.916 0.000 0.028 0.056
#&gt; SRR1655010     1  0.0469      0.949 0.988 0.000 0.012 0.000
#&gt; SRR1655011     1  0.0469      0.949 0.988 0.000 0.012 0.000
#&gt; SRR1655012     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655013     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655014     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655015     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655016     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655017     2  0.6653      0.600 0.000 0.548 0.096 0.356
#&gt; SRR1655018     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655019     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655020     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655021     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655022     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655023     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655024     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655025     2  0.6336      0.616 0.000 0.608 0.088 0.304
#&gt; SRR1655026     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655027     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655028     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655029     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655030     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655031     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655032     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655033     2  0.6637     -0.176 0.000 0.572 0.104 0.324
#&gt; SRR1655034     3  0.2399      0.982 0.000 0.048 0.920 0.032
#&gt; SRR1655035     3  0.2399      0.982 0.000 0.048 0.920 0.032
#&gt; SRR1655036     3  0.1722      0.978 0.000 0.048 0.944 0.008
#&gt; SRR1655037     3  0.1722      0.978 0.000 0.048 0.944 0.008
#&gt; SRR1655038     3  0.2399      0.982 0.000 0.048 0.920 0.032
#&gt; SRR1655039     3  0.2399      0.982 0.000 0.048 0.920 0.032
#&gt; SRR1655040     3  0.1389      0.981 0.000 0.048 0.952 0.000
#&gt; SRR1655041     3  0.1389      0.981 0.000 0.048 0.952 0.000
#&gt; SRR1655042     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655043     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655044     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655045     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655046     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655047     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655048     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655049     2  0.0000      0.518 0.000 1.000 0.000 0.000
#&gt; SRR1655050     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655051     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655052     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655053     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655054     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655055     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655056     4  0.7836      1.000 0.000 0.288 0.304 0.408
#&gt; SRR1655057     4  0.7836      1.000 0.000 0.288 0.304 0.408
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.2037      0.943 0.920 0.000 0.012 0.004 0.064
#&gt; SRR1655003     1  0.3407      0.930 0.836 0.000 0.020 0.012 0.132
#&gt; SRR1655004     1  0.2519      0.942 0.884 0.000 0.016 0.000 0.100
#&gt; SRR1655005     1  0.3061      0.934 0.844 0.000 0.020 0.000 0.136
#&gt; SRR1655006     1  0.1942      0.943 0.920 0.000 0.012 0.000 0.068
#&gt; SRR1655007     1  0.1087      0.940 0.968 0.000 0.016 0.008 0.008
#&gt; SRR1655008     1  0.1914      0.939 0.924 0.000 0.016 0.000 0.060
#&gt; SRR1655009     1  0.2079      0.937 0.916 0.000 0.020 0.000 0.064
#&gt; SRR1655010     1  0.0451      0.942 0.988 0.000 0.004 0.008 0.000
#&gt; SRR1655011     1  0.0451      0.942 0.988 0.000 0.004 0.008 0.000
#&gt; SRR1655012     2  0.3242      0.844 0.000 0.816 0.000 0.012 0.172
#&gt; SRR1655013     2  0.3242      0.844 0.000 0.816 0.000 0.012 0.172
#&gt; SRR1655014     2  0.3280      0.844 0.000 0.812 0.000 0.012 0.176
#&gt; SRR1655015     2  0.3280      0.844 0.000 0.812 0.000 0.012 0.176
#&gt; SRR1655016     2  0.3242      0.844 0.000 0.816 0.000 0.012 0.172
#&gt; SRR1655017     2  0.3242      0.844 0.000 0.816 0.000 0.012 0.172
#&gt; SRR1655018     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655019     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655020     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655021     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655022     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655023     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655024     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655025     2  0.1043      0.880 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1655026     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655027     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655028     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655029     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655030     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655031     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655032     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655033     4  0.3115      0.543 0.000 0.112 0.036 0.852 0.000
#&gt; SRR1655034     3  0.3011      0.964 0.000 0.032 0.884 0.048 0.036
#&gt; SRR1655035     3  0.3011      0.964 0.000 0.032 0.884 0.048 0.036
#&gt; SRR1655036     3  0.2450      0.951 0.000 0.032 0.912 0.028 0.028
#&gt; SRR1655037     3  0.2450      0.951 0.000 0.032 0.912 0.028 0.028
#&gt; SRR1655038     3  0.2937      0.964 0.000 0.032 0.888 0.044 0.036
#&gt; SRR1655039     3  0.2937      0.964 0.000 0.032 0.888 0.044 0.036
#&gt; SRR1655040     3  0.1579      0.962 0.000 0.032 0.944 0.024 0.000
#&gt; SRR1655041     3  0.1579      0.962 0.000 0.032 0.944 0.024 0.000
#&gt; SRR1655042     4  0.6935      0.569 0.000 0.340 0.012 0.428 0.220
#&gt; SRR1655043     4  0.6935      0.569 0.000 0.340 0.012 0.428 0.220
#&gt; SRR1655044     4  0.6951      0.568 0.000 0.340 0.012 0.424 0.224
#&gt; SRR1655045     4  0.6951      0.568 0.000 0.340 0.012 0.424 0.224
#&gt; SRR1655046     4  0.6935      0.569 0.000 0.340 0.012 0.428 0.220
#&gt; SRR1655047     4  0.6935      0.569 0.000 0.340 0.012 0.428 0.220
#&gt; SRR1655048     4  0.7009      0.563 0.000 0.344 0.016 0.424 0.216
#&gt; SRR1655049     4  0.7009      0.563 0.000 0.344 0.016 0.424 0.216
#&gt; SRR1655050     4  0.7631     -0.966 0.000 0.052 0.232 0.360 0.356
#&gt; SRR1655051     5  0.7608      0.969 0.000 0.052 0.224 0.360 0.364
#&gt; SRR1655052     5  0.7589      0.993 0.000 0.052 0.224 0.328 0.396
#&gt; SRR1655053     5  0.7589      0.993 0.000 0.052 0.224 0.328 0.396
#&gt; SRR1655054     5  0.7584      0.993 0.000 0.052 0.224 0.324 0.400
#&gt; SRR1655055     5  0.7584      0.993 0.000 0.052 0.224 0.324 0.400
#&gt; SRR1655056     5  0.7584      0.993 0.000 0.052 0.224 0.324 0.400
#&gt; SRR1655057     5  0.7589      0.993 0.000 0.052 0.224 0.328 0.396
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1655002     1   0.162      0.899 0.936 0.000 0.000 0.040 0.020 0.004
#&gt; SRR1655003     1   0.245      0.885 0.900 0.000 0.004 0.020 0.052 0.024
#&gt; SRR1655004     1   0.000      0.903 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1   0.199      0.889 0.924 0.000 0.004 0.024 0.036 0.012
#&gt; SRR1655006     1   0.115      0.902 0.956 0.000 0.000 0.032 0.012 0.000
#&gt; SRR1655007     1   0.418      0.900 0.768 0.000 0.008 0.152 0.060 0.012
#&gt; SRR1655008     1   0.386      0.899 0.812 0.000 0.020 0.100 0.056 0.012
#&gt; SRR1655009     1   0.431      0.892 0.780 0.000 0.020 0.112 0.072 0.016
#&gt; SRR1655010     1   0.376      0.900 0.800 0.000 0.008 0.140 0.040 0.012
#&gt; SRR1655011     1   0.376      0.900 0.800 0.000 0.008 0.140 0.040 0.012
#&gt; SRR1655012     2   0.378      0.740 0.000 0.760 0.000 0.188 0.052 0.000
#&gt; SRR1655013     2   0.378      0.740 0.000 0.760 0.000 0.188 0.052 0.000
#&gt; SRR1655014     2   0.405      0.736 0.000 0.756 0.004 0.164 0.076 0.000
#&gt; SRR1655015     2   0.405      0.736 0.000 0.756 0.004 0.164 0.076 0.000
#&gt; SRR1655016     2   0.378      0.740 0.000 0.760 0.000 0.188 0.052 0.000
#&gt; SRR1655017     2   0.378      0.740 0.000 0.760 0.000 0.188 0.052 0.000
#&gt; SRR1655018     2   0.235      0.793 0.000 0.868 0.000 0.000 0.008 0.124
#&gt; SRR1655019     2   0.235      0.793 0.000 0.868 0.000 0.000 0.008 0.124
#&gt; SRR1655020     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655021     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655022     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655023     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655024     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655025     2   0.223      0.794 0.000 0.872 0.000 0.000 0.004 0.124
#&gt; SRR1655026     4   0.752      0.994 0.000 0.072 0.032 0.416 0.216 0.264
#&gt; SRR1655027     4   0.752      0.994 0.000 0.072 0.032 0.416 0.216 0.264
#&gt; SRR1655028     4   0.753      0.994 0.000 0.072 0.032 0.412 0.220 0.264
#&gt; SRR1655029     4   0.753      0.994 0.000 0.072 0.032 0.412 0.220 0.264
#&gt; SRR1655030     4   0.754      0.991 0.000 0.072 0.032 0.408 0.220 0.268
#&gt; SRR1655031     4   0.754      0.991 0.000 0.072 0.032 0.408 0.220 0.268
#&gt; SRR1655032     4   0.754      0.993 0.000 0.072 0.032 0.408 0.220 0.268
#&gt; SRR1655033     4   0.754      0.993 0.000 0.072 0.032 0.408 0.220 0.268
#&gt; SRR1655034     3   0.137      0.911 0.000 0.016 0.952 0.016 0.000 0.016
#&gt; SRR1655035     3   0.137      0.911 0.000 0.016 0.952 0.016 0.000 0.016
#&gt; SRR1655036     3   0.423      0.890 0.000 0.016 0.784 0.120 0.020 0.060
#&gt; SRR1655037     3   0.423      0.890 0.000 0.016 0.784 0.120 0.020 0.060
#&gt; SRR1655038     3   0.117      0.913 0.000 0.016 0.960 0.000 0.016 0.008
#&gt; SRR1655039     3   0.117      0.913 0.000 0.016 0.960 0.000 0.016 0.008
#&gt; SRR1655040     3   0.356      0.909 0.000 0.016 0.840 0.076 0.028 0.040
#&gt; SRR1655041     3   0.356      0.909 0.000 0.016 0.840 0.076 0.028 0.040
#&gt; SRR1655042     6   0.234      0.983 0.000 0.148 0.000 0.000 0.000 0.852
#&gt; SRR1655043     6   0.234      0.983 0.000 0.148 0.000 0.000 0.000 0.852
#&gt; SRR1655044     6   0.259      0.981 0.000 0.148 0.000 0.000 0.008 0.844
#&gt; SRR1655045     6   0.259      0.981 0.000 0.148 0.000 0.000 0.008 0.844
#&gt; SRR1655046     6   0.234      0.983 0.000 0.148 0.000 0.000 0.000 0.852
#&gt; SRR1655047     6   0.234      0.983 0.000 0.148 0.000 0.000 0.000 0.852
#&gt; SRR1655048     6   0.359      0.955 0.000 0.152 0.000 0.028 0.020 0.800
#&gt; SRR1655049     6   0.359      0.955 0.000 0.152 0.000 0.028 0.020 0.800
#&gt; SRR1655050     5   0.610      0.917 0.000 0.024 0.168 0.064 0.636 0.108
#&gt; SRR1655051     5   0.584      0.936 0.000 0.024 0.168 0.048 0.656 0.104
#&gt; SRR1655052     5   0.469      0.974 0.000 0.024 0.168 0.000 0.720 0.088
#&gt; SRR1655053     5   0.469      0.974 0.000 0.024 0.168 0.000 0.720 0.088
#&gt; SRR1655054     5   0.459      0.975 0.000 0.024 0.168 0.000 0.728 0.080
#&gt; SRR1655055     5   0.459      0.975 0.000 0.024 0.168 0.000 0.728 0.080
#&gt; SRR1655056     5   0.459      0.975 0.000 0.024 0.168 0.000 0.728 0.080
#&gt; SRR1655057     5   0.464      0.975 0.000 0.024 0.168 0.000 0.724 0.084
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.751           0.880       0.947         0.3534 0.701   0.701
#> 3 3 0.647           0.941       0.963         0.7074 0.688   0.556
#> 4 4 0.803           0.913       0.946         0.1908 0.803   0.542
#> 5 5 0.875           0.869       0.916         0.1059 0.844   0.508
#> 6 6 1.000           0.989       0.979         0.0549 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.000      1.000 1.000 0.000
#&gt; SRR1655003     1   0.000      1.000 1.000 0.000
#&gt; SRR1655004     1   0.000      1.000 1.000 0.000
#&gt; SRR1655005     1   0.000      1.000 1.000 0.000
#&gt; SRR1655006     1   0.000      1.000 1.000 0.000
#&gt; SRR1655007     1   0.000      1.000 1.000 0.000
#&gt; SRR1655008     1   0.000      1.000 1.000 0.000
#&gt; SRR1655009     1   0.000      1.000 1.000 0.000
#&gt; SRR1655010     1   0.000      1.000 1.000 0.000
#&gt; SRR1655011     1   0.000      1.000 1.000 0.000
#&gt; SRR1655012     2   0.000      0.929 0.000 1.000
#&gt; SRR1655013     2   0.000      0.929 0.000 1.000
#&gt; SRR1655014     2   0.000      0.929 0.000 1.000
#&gt; SRR1655015     2   0.000      0.929 0.000 1.000
#&gt; SRR1655016     2   0.000      0.929 0.000 1.000
#&gt; SRR1655017     2   0.000      0.929 0.000 1.000
#&gt; SRR1655018     2   0.000      0.929 0.000 1.000
#&gt; SRR1655019     2   0.000      0.929 0.000 1.000
#&gt; SRR1655020     2   0.000      0.929 0.000 1.000
#&gt; SRR1655021     2   0.000      0.929 0.000 1.000
#&gt; SRR1655022     2   0.000      0.929 0.000 1.000
#&gt; SRR1655023     2   0.000      0.929 0.000 1.000
#&gt; SRR1655024     2   0.000      0.929 0.000 1.000
#&gt; SRR1655025     2   0.000      0.929 0.000 1.000
#&gt; SRR1655026     2   0.000      0.929 0.000 1.000
#&gt; SRR1655027     2   0.000      0.929 0.000 1.000
#&gt; SRR1655028     2   0.000      0.929 0.000 1.000
#&gt; SRR1655029     2   0.000      0.929 0.000 1.000
#&gt; SRR1655030     2   0.000      0.929 0.000 1.000
#&gt; SRR1655031     2   0.000      0.929 0.000 1.000
#&gt; SRR1655032     2   0.000      0.929 0.000 1.000
#&gt; SRR1655033     2   0.000      0.929 0.000 1.000
#&gt; SRR1655034     2   0.952      0.500 0.372 0.628
#&gt; SRR1655035     2   0.952      0.500 0.372 0.628
#&gt; SRR1655036     2   0.952      0.500 0.372 0.628
#&gt; SRR1655037     2   0.952      0.500 0.372 0.628
#&gt; SRR1655038     2   0.952      0.500 0.372 0.628
#&gt; SRR1655039     2   0.952      0.500 0.372 0.628
#&gt; SRR1655040     2   0.952      0.500 0.372 0.628
#&gt; SRR1655041     2   0.952      0.500 0.372 0.628
#&gt; SRR1655042     2   0.000      0.929 0.000 1.000
#&gt; SRR1655043     2   0.000      0.929 0.000 1.000
#&gt; SRR1655044     2   0.000      0.929 0.000 1.000
#&gt; SRR1655045     2   0.000      0.929 0.000 1.000
#&gt; SRR1655046     2   0.000      0.929 0.000 1.000
#&gt; SRR1655047     2   0.000      0.929 0.000 1.000
#&gt; SRR1655048     2   0.000      0.929 0.000 1.000
#&gt; SRR1655049     2   0.000      0.929 0.000 1.000
#&gt; SRR1655050     2   0.000      0.929 0.000 1.000
#&gt; SRR1655051     2   0.000      0.929 0.000 1.000
#&gt; SRR1655052     2   0.000      0.929 0.000 1.000
#&gt; SRR1655053     2   0.000      0.929 0.000 1.000
#&gt; SRR1655054     2   0.000      0.929 0.000 1.000
#&gt; SRR1655055     2   0.000      0.929 0.000 1.000
#&gt; SRR1655056     2   0.000      0.929 0.000 1.000
#&gt; SRR1655057     2   0.000      0.929 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655013     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655014     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655015     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655016     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655017     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655018     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655019     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655020     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655021     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655022     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655023     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655024     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655025     2  0.0424      0.954  0 0.992 0.008
#&gt; SRR1655026     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655027     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655028     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655029     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655030     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655031     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655032     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655033     2  0.3619      0.867  0 0.864 0.136
#&gt; SRR1655034     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655035     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655036     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655037     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655038     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655039     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655040     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655041     3  0.0000      0.921  0 0.000 1.000
#&gt; SRR1655042     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655043     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655044     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655045     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655046     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655047     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655048     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655049     2  0.0000      0.953  0 1.000 0.000
#&gt; SRR1655050     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655051     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655052     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655053     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655054     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655055     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655056     3  0.3192      0.922  0 0.112 0.888
#&gt; SRR1655057     3  0.3192      0.922  0 0.112 0.888
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655013     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655014     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655015     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655016     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655017     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655018     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655019     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655020     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655021     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655022     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655023     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655024     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655025     2   0.000      0.876  0 1.000 0.000 0.000
#&gt; SRR1655026     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655027     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655028     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655029     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655030     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655031     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655032     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655033     4   0.179      0.937  0 0.068 0.000 0.932
#&gt; SRR1655034     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655042     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655043     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655044     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655045     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655046     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655047     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655048     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655049     2   0.425      0.736  0 0.724 0.000 0.276
#&gt; SRR1655050     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655051     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655052     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655053     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655054     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655055     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655056     4   0.102      0.936  0 0.000 0.032 0.968
#&gt; SRR1655057     4   0.102      0.936  0 0.000 0.032 0.968
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.995  0 1.000 0.000 0.000 0.000
#&gt; SRR1655018     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655019     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655020     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655021     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655022     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655023     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655024     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655025     2  0.0290      0.996  0 0.992 0.000 0.008 0.000
#&gt; SRR1655026     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655027     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655028     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655029     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655030     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655031     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655032     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655033     4  0.4291      0.440  0 0.000 0.000 0.536 0.464
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655042     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655043     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655044     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655045     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655046     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655047     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655048     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655049     4  0.1965      0.653  0 0.096 0.000 0.904 0.000
#&gt; SRR1655050     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655051     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655052     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655053     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655054     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655055     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655056     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655057     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655012     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655013     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655014     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655015     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655016     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655017     2  0.1572      0.949  0 0.936  0 0.028 0.000 0.036
#&gt; SRR1655018     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655019     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655020     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655021     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655022     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655023     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655024     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655025     2  0.0790      0.962  0 0.968  0 0.000 0.000 0.032
#&gt; SRR1655026     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655027     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655028     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655029     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655030     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655031     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655032     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655033     4  0.0713      1.000  0 0.000  0 0.972 0.028 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655042     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655043     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655044     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655045     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655046     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655047     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655048     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655049     6  0.0865      1.000  0 0.000  0 0.036 0.000 0.964
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 1.000           1.000       1.000         0.6587 0.803   0.719
#> 4 4 0.830           0.900       0.925         0.4197 0.766   0.536
#> 5 5 0.981           0.934       0.970         0.1099 0.938   0.769
#> 6 6 1.000           1.000       1.000         0.0724 0.932   0.690
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3
#&gt; SRR1655002     1       0          1  1  0  0
#&gt; SRR1655003     1       0          1  1  0  0
#&gt; SRR1655004     1       0          1  1  0  0
#&gt; SRR1655005     1       0          1  1  0  0
#&gt; SRR1655006     1       0          1  1  0  0
#&gt; SRR1655007     1       0          1  1  0  0
#&gt; SRR1655008     1       0          1  1  0  0
#&gt; SRR1655009     1       0          1  1  0  0
#&gt; SRR1655010     1       0          1  1  0  0
#&gt; SRR1655011     1       0          1  1  0  0
#&gt; SRR1655012     2       0          1  0  1  0
#&gt; SRR1655013     2       0          1  0  1  0
#&gt; SRR1655014     2       0          1  0  1  0
#&gt; SRR1655015     2       0          1  0  1  0
#&gt; SRR1655016     2       0          1  0  1  0
#&gt; SRR1655017     2       0          1  0  1  0
#&gt; SRR1655018     2       0          1  0  1  0
#&gt; SRR1655019     2       0          1  0  1  0
#&gt; SRR1655020     2       0          1  0  1  0
#&gt; SRR1655021     2       0          1  0  1  0
#&gt; SRR1655022     2       0          1  0  1  0
#&gt; SRR1655023     2       0          1  0  1  0
#&gt; SRR1655024     2       0          1  0  1  0
#&gt; SRR1655025     2       0          1  0  1  0
#&gt; SRR1655026     2       0          1  0  1  0
#&gt; SRR1655027     2       0          1  0  1  0
#&gt; SRR1655028     2       0          1  0  1  0
#&gt; SRR1655029     2       0          1  0  1  0
#&gt; SRR1655030     2       0          1  0  1  0
#&gt; SRR1655031     2       0          1  0  1  0
#&gt; SRR1655032     2       0          1  0  1  0
#&gt; SRR1655033     2       0          1  0  1  0
#&gt; SRR1655034     3       0          1  0  0  1
#&gt; SRR1655035     3       0          1  0  0  1
#&gt; SRR1655036     3       0          1  0  0  1
#&gt; SRR1655037     3       0          1  0  0  1
#&gt; SRR1655038     3       0          1  0  0  1
#&gt; SRR1655039     3       0          1  0  0  1
#&gt; SRR1655040     3       0          1  0  0  1
#&gt; SRR1655041     3       0          1  0  0  1
#&gt; SRR1655042     2       0          1  0  1  0
#&gt; SRR1655043     2       0          1  0  1  0
#&gt; SRR1655044     2       0          1  0  1  0
#&gt; SRR1655045     2       0          1  0  1  0
#&gt; SRR1655046     2       0          1  0  1  0
#&gt; SRR1655047     2       0          1  0  1  0
#&gt; SRR1655048     2       0          1  0  1  0
#&gt; SRR1655049     2       0          1  0  1  0
#&gt; SRR1655050     2       0          1  0  1  0
#&gt; SRR1655051     2       0          1  0  1  0
#&gt; SRR1655052     2       0          1  0  1  0
#&gt; SRR1655053     2       0          1  0  1  0
#&gt; SRR1655054     2       0          1  0  1  0
#&gt; SRR1655055     2       0          1  0  1  0
#&gt; SRR1655056     2       0          1  0  1  0
#&gt; SRR1655057     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655013     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655014     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655015     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655016     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655017     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655018     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655019     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655020     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655021     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655022     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655023     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655024     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655025     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655026     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655027     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655028     4  0.4477      0.773  0 0.312  0 0.688
#&gt; SRR1655029     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655030     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655031     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655032     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655033     4  0.4500      0.773  0 0.316  0 0.684
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2  0.0469      0.979  0 0.988  0 0.012
#&gt; SRR1655043     2  0.2011      0.879  0 0.920  0 0.080
#&gt; SRR1655044     4  0.4843      0.681  0 0.396  0 0.604
#&gt; SRR1655045     4  0.4817      0.692  0 0.388  0 0.612
#&gt; SRR1655046     4  0.4855      0.675  0 0.400  0 0.600
#&gt; SRR1655047     4  0.4866      0.668  0 0.404  0 0.596
#&gt; SRR1655048     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655049     2  0.0000      0.993  0 1.000  0 0.000
#&gt; SRR1655050     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655051     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655052     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655053     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655054     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655055     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655056     4  0.0000      0.719  0 0.000  0 1.000
#&gt; SRR1655057     4  0.0000      0.719  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655013     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655014     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655015     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655016     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655017     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655018     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655019     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655020     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655021     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655022     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655023     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655024     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655025     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655026     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655027     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655028     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655029     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655030     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655031     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655032     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655033     4  0.0000      0.812  0 0.000  0 1.000  0
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     2  0.0404      0.981  0 0.988  0 0.012  0
#&gt; SRR1655043     2  0.1671      0.900  0 0.924  0 0.076  0
#&gt; SRR1655044     4  0.4161      0.517  0 0.392  0 0.608  0
#&gt; SRR1655045     4  0.4126      0.535  0 0.380  0 0.620  0
#&gt; SRR1655046     4  0.4182      0.502  0 0.400  0 0.600  0
#&gt; SRR1655047     4  0.4210      0.476  0 0.412  0 0.588  0
#&gt; SRR1655048     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655049     2  0.0000      0.994  0 1.000  0 0.000  0
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     6       0          1  0  0  0  0  0  1
#&gt; SRR1655049     6       0          1  0  0  0  0  0  1
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.751           0.953       0.975         0.3243 0.701   0.701
#> 3 3 0.621           0.836       0.897         0.6944 0.803   0.719
#> 4 4 0.751           0.887       0.921         0.2854 0.771   0.546
#> 5 5 0.908           0.945       0.962         0.1187 0.891   0.628
#> 6 6 0.933           0.975       0.975         0.0586 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 5
```

There is also optional best $k$ = 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.000      1.000 1.000 0.000
#&gt; SRR1655003     1   0.000      1.000 1.000 0.000
#&gt; SRR1655004     1   0.000      1.000 1.000 0.000
#&gt; SRR1655005     1   0.000      1.000 1.000 0.000
#&gt; SRR1655006     1   0.000      1.000 1.000 0.000
#&gt; SRR1655007     1   0.000      1.000 1.000 0.000
#&gt; SRR1655008     1   0.000      1.000 1.000 0.000
#&gt; SRR1655009     1   0.000      1.000 1.000 0.000
#&gt; SRR1655010     1   0.000      1.000 1.000 0.000
#&gt; SRR1655011     1   0.000      1.000 1.000 0.000
#&gt; SRR1655012     2   0.000      0.968 0.000 1.000
#&gt; SRR1655013     2   0.000      0.968 0.000 1.000
#&gt; SRR1655014     2   0.000      0.968 0.000 1.000
#&gt; SRR1655015     2   0.000      0.968 0.000 1.000
#&gt; SRR1655016     2   0.000      0.968 0.000 1.000
#&gt; SRR1655017     2   0.000      0.968 0.000 1.000
#&gt; SRR1655018     2   0.000      0.968 0.000 1.000
#&gt; SRR1655019     2   0.000      0.968 0.000 1.000
#&gt; SRR1655020     2   0.000      0.968 0.000 1.000
#&gt; SRR1655021     2   0.000      0.968 0.000 1.000
#&gt; SRR1655022     2   0.000      0.968 0.000 1.000
#&gt; SRR1655023     2   0.000      0.968 0.000 1.000
#&gt; SRR1655024     2   0.000      0.968 0.000 1.000
#&gt; SRR1655025     2   0.000      0.968 0.000 1.000
#&gt; SRR1655026     2   0.000      0.968 0.000 1.000
#&gt; SRR1655027     2   0.000      0.968 0.000 1.000
#&gt; SRR1655028     2   0.000      0.968 0.000 1.000
#&gt; SRR1655029     2   0.000      0.968 0.000 1.000
#&gt; SRR1655030     2   0.000      0.968 0.000 1.000
#&gt; SRR1655031     2   0.000      0.968 0.000 1.000
#&gt; SRR1655032     2   0.000      0.968 0.000 1.000
#&gt; SRR1655033     2   0.000      0.968 0.000 1.000
#&gt; SRR1655034     2   0.662      0.825 0.172 0.828
#&gt; SRR1655035     2   0.662      0.825 0.172 0.828
#&gt; SRR1655036     2   0.662      0.825 0.172 0.828
#&gt; SRR1655037     2   0.662      0.825 0.172 0.828
#&gt; SRR1655038     2   0.662      0.825 0.172 0.828
#&gt; SRR1655039     2   0.662      0.825 0.172 0.828
#&gt; SRR1655040     2   0.662      0.825 0.172 0.828
#&gt; SRR1655041     2   0.662      0.825 0.172 0.828
#&gt; SRR1655042     2   0.000      0.968 0.000 1.000
#&gt; SRR1655043     2   0.000      0.968 0.000 1.000
#&gt; SRR1655044     2   0.000      0.968 0.000 1.000
#&gt; SRR1655045     2   0.000      0.968 0.000 1.000
#&gt; SRR1655046     2   0.000      0.968 0.000 1.000
#&gt; SRR1655047     2   0.000      0.968 0.000 1.000
#&gt; SRR1655048     2   0.000      0.968 0.000 1.000
#&gt; SRR1655049     2   0.000      0.968 0.000 1.000
#&gt; SRR1655050     2   0.000      0.968 0.000 1.000
#&gt; SRR1655051     2   0.000      0.968 0.000 1.000
#&gt; SRR1655052     2   0.000      0.968 0.000 1.000
#&gt; SRR1655053     2   0.000      0.968 0.000 1.000
#&gt; SRR1655054     2   0.000      0.968 0.000 1.000
#&gt; SRR1655055     2   0.000      0.968 0.000 1.000
#&gt; SRR1655056     2   0.000      0.968 0.000 1.000
#&gt; SRR1655057     2   0.000      0.968 0.000 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655013     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655014     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655015     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655016     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655017     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655018     2  0.4002      0.817  0 0.840 0.160
#&gt; SRR1655019     2  0.4002      0.817  0 0.840 0.160
#&gt; SRR1655020     2  0.0237      0.822  0 0.996 0.004
#&gt; SRR1655021     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655022     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655023     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655024     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655025     2  0.0000      0.821  0 1.000 0.000
#&gt; SRR1655026     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655027     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655028     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655029     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655030     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655031     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655032     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655033     2  0.4654      0.800  0 0.792 0.208
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655042     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655043     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655044     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655045     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655046     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655047     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655048     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655049     2  0.2448      0.833  0 0.924 0.076
#&gt; SRR1655050     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655051     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655052     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655053     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655054     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655055     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655056     2  0.6252      0.530  0 0.556 0.444
#&gt; SRR1655057     2  0.6252      0.530  0 0.556 0.444
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655013     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655014     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655015     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655016     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655017     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655018     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655019     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655020     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655021     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655022     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655023     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655024     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655025     2   0.000      0.963  0 1.000  0 0.000
#&gt; SRR1655026     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655027     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655028     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655029     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655030     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655031     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655032     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655033     4   0.387      0.818  0 0.228  0 0.772
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655043     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655044     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655045     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655046     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655047     4   0.475      0.699  0 0.368  0 0.632
#&gt; SRR1655048     2   0.361      0.662  0 0.800  0 0.200
#&gt; SRR1655049     2   0.361      0.662  0 0.800  0 0.200
#&gt; SRR1655050     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655051     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655052     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655053     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655054     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655055     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655056     4   0.000      0.769  0 0.000  0 1.000
#&gt; SRR1655057     4   0.000      0.769  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655013     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655014     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655015     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655016     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655017     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655018     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655019     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655020     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655021     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655022     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655023     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655024     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655025     2   0.000      1.000  0 1.000  0 0.000  0
#&gt; SRR1655026     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655027     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655028     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655029     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655030     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655031     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655032     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655033     4   0.000      0.834  0 0.000  0 1.000  0
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655043     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655044     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655045     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655046     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655047     4   0.342      0.812  0 0.240  0 0.760  0
#&gt; SRR1655048     4   0.397      0.695  0 0.336  0 0.664  0
#&gt; SRR1655049     4   0.397      0.695  0 0.336  0 0.664  0
#&gt; SRR1655050     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5   0.000      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3 p4 p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0  0  0 0.000
#&gt; SRR1655012     2  0.0547      0.898  0 0.980  0  0  0 0.020
#&gt; SRR1655013     2  0.0547      0.898  0 0.980  0  0  0 0.020
#&gt; SRR1655014     2  0.0000      0.885  0 1.000  0  0  0 0.000
#&gt; SRR1655015     2  0.0000      0.885  0 1.000  0  0  0 0.000
#&gt; SRR1655016     2  0.0547      0.898  0 0.980  0  0  0 0.020
#&gt; SRR1655017     2  0.0547      0.898  0 0.980  0  0  0 0.020
#&gt; SRR1655018     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655019     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655020     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655021     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655022     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655023     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655024     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655025     2  0.2454      0.920  0 0.840  0  0  0 0.160
#&gt; SRR1655026     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655027     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655028     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655029     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655030     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655031     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655032     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655033     4  0.0000      1.000  0 0.000  0  1  0 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1  0  0 0.000
#&gt; SRR1655042     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655043     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655044     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655045     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655046     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655047     6  0.0000      0.991  0 0.000  0  0  0 1.000
#&gt; SRR1655048     6  0.0713      0.971  0 0.028  0  0  0 0.972
#&gt; SRR1655049     6  0.0713      0.971  0 0.028  0  0  0 0.972
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0  0  1 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0  0  1 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.917           0.966       0.980         0.9893 0.688   0.556
#> 4 4 1.000           0.999       0.999         0.2166 0.803   0.542
#> 5 5 0.917           0.957       0.963         0.1000 0.844   0.508
#> 6 6 1.000           0.996       0.946         0.0453 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655012     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655013     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655014     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655015     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655016     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655017     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655018     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655019     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655020     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655021     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655022     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655023     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655024     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655025     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655026     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655027     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655028     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655029     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655030     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655031     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655032     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655033     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655034     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655035     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655036     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655037     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655038     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655039     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655040     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655041     3   0.000      0.899  0 0.000 1.000
#&gt; SRR1655042     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655043     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655044     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655045     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655046     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655047     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655048     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655049     2   0.000      1.000  0 1.000 0.000
#&gt; SRR1655050     3   0.263      0.897  0 0.084 0.916
#&gt; SRR1655051     3   0.312      0.890  0 0.108 0.892
#&gt; SRR1655052     3   0.445      0.827  0 0.192 0.808
#&gt; SRR1655053     3   0.455      0.818  0 0.200 0.800
#&gt; SRR1655054     3   0.388      0.866  0 0.152 0.848
#&gt; SRR1655055     3   0.369      0.874  0 0.140 0.860
#&gt; SRR1655056     3   0.288      0.895  0 0.096 0.904
#&gt; SRR1655057     3   0.406      0.856  0 0.164 0.836
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655026     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655027     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655028     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655029     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655030     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655031     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655032     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655033     4  0.0188      0.997  0 0.004 0.000 0.996
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655042     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655043     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655044     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655045     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655046     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655047     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655048     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655049     2  0.0000      1.000  0 1.000 0.000 0.000
#&gt; SRR1655050     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655051     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655052     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655053     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655054     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655055     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655056     4  0.0188      0.997  0 0.000 0.004 0.996
#&gt; SRR1655057     4  0.0188      0.997  0 0.000 0.004 0.996
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1655002     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655003     1  0.0162      0.998 0.996 0.000  0 0.004 0.000
#&gt; SRR1655004     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655005     1  0.0162      0.998 0.996 0.000  0 0.004 0.000
#&gt; SRR1655006     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655007     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655008     1  0.0162      0.998 0.996 0.000  0 0.004 0.000
#&gt; SRR1655009     1  0.0162      0.998 0.996 0.000  0 0.004 0.000
#&gt; SRR1655010     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655011     1  0.0000      0.999 1.000 0.000  0 0.000 0.000
#&gt; SRR1655012     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655013     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655014     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655015     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655016     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655017     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655018     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655019     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655020     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655021     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655022     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655023     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655024     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655025     2  0.0000      1.000 0.000 1.000  0 0.000 0.000
#&gt; SRR1655026     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655027     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655028     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655029     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655030     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655031     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655032     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655033     4  0.3305      0.839 0.000 0.000  0 0.776 0.224
#&gt; SRR1655034     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; SRR1655042     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655043     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655044     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655045     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655046     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655047     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655048     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655049     4  0.0510      0.858 0.000 0.016  0 0.984 0.000
#&gt; SRR1655050     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655051     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655052     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655053     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655054     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655055     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655056     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
#&gt; SRR1655057     5  0.0000      1.000 0.000 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; SRR1655002     1  0.1267      0.974 0.940 0.000  0 0.060 0.000 0.000
#&gt; SRR1655003     1  0.1007      0.974 0.956 0.000  0 0.044 0.000 0.000
#&gt; SRR1655004     1  0.0260      0.984 0.992 0.000  0 0.008 0.000 0.000
#&gt; SRR1655005     1  0.1007      0.974 0.956 0.000  0 0.044 0.000 0.000
#&gt; SRR1655006     1  0.0458      0.983 0.984 0.000  0 0.016 0.000 0.000
#&gt; SRR1655007     1  0.0547      0.981 0.980 0.000  0 0.020 0.000 0.000
#&gt; SRR1655008     1  0.0000      0.983 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      0.983 1.000 0.000  0 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0458      0.983 0.984 0.000  0 0.016 0.000 0.000
#&gt; SRR1655011     1  0.0458      0.983 0.984 0.000  0 0.016 0.000 0.000
#&gt; SRR1655012     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655013     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655014     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655015     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655016     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655017     2  0.0146      0.998 0.000 0.996  0 0.004 0.000 0.000
#&gt; SRR1655018     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1655026     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655027     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655028     4  0.3172      0.995 0.000 0.000  0 0.832 0.092 0.076
#&gt; SRR1655029     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655030     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655031     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655032     4  0.3172      0.995 0.000 0.000  0 0.832 0.092 0.076
#&gt; SRR1655033     4  0.3167      0.998 0.000 0.000  0 0.832 0.096 0.072
#&gt; SRR1655034     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1655042     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655043     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655044     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655045     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655046     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655047     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655048     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655049     6  0.0000      1.000 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1655050     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655051     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655052     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655053     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655054     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655055     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655056     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; SRR1655057     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2     1               1           1         0.2994 0.701   0.701
#> 3 3     1               1           1         0.6587 0.803   0.719
#> 4 4     1               1           1         0.4598 0.771   0.546
#> 5 5     1               1           1         0.1002 0.927   0.736
#> 6 6     1               1           1         0.0521 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3
#&gt; SRR1655002     1       0          1  1  0  0
#&gt; SRR1655003     1       0          1  1  0  0
#&gt; SRR1655004     1       0          1  1  0  0
#&gt; SRR1655005     1       0          1  1  0  0
#&gt; SRR1655006     1       0          1  1  0  0
#&gt; SRR1655007     1       0          1  1  0  0
#&gt; SRR1655008     1       0          1  1  0  0
#&gt; SRR1655009     1       0          1  1  0  0
#&gt; SRR1655010     1       0          1  1  0  0
#&gt; SRR1655011     1       0          1  1  0  0
#&gt; SRR1655012     2       0          1  0  1  0
#&gt; SRR1655013     2       0          1  0  1  0
#&gt; SRR1655014     2       0          1  0  1  0
#&gt; SRR1655015     2       0          1  0  1  0
#&gt; SRR1655016     2       0          1  0  1  0
#&gt; SRR1655017     2       0          1  0  1  0
#&gt; SRR1655018     2       0          1  0  1  0
#&gt; SRR1655019     2       0          1  0  1  0
#&gt; SRR1655020     2       0          1  0  1  0
#&gt; SRR1655021     2       0          1  0  1  0
#&gt; SRR1655022     2       0          1  0  1  0
#&gt; SRR1655023     2       0          1  0  1  0
#&gt; SRR1655024     2       0          1  0  1  0
#&gt; SRR1655025     2       0          1  0  1  0
#&gt; SRR1655026     2       0          1  0  1  0
#&gt; SRR1655027     2       0          1  0  1  0
#&gt; SRR1655028     2       0          1  0  1  0
#&gt; SRR1655029     2       0          1  0  1  0
#&gt; SRR1655030     2       0          1  0  1  0
#&gt; SRR1655031     2       0          1  0  1  0
#&gt; SRR1655032     2       0          1  0  1  0
#&gt; SRR1655033     2       0          1  0  1  0
#&gt; SRR1655034     3       0          1  0  0  1
#&gt; SRR1655035     3       0          1  0  0  1
#&gt; SRR1655036     3       0          1  0  0  1
#&gt; SRR1655037     3       0          1  0  0  1
#&gt; SRR1655038     3       0          1  0  0  1
#&gt; SRR1655039     3       0          1  0  0  1
#&gt; SRR1655040     3       0          1  0  0  1
#&gt; SRR1655041     3       0          1  0  0  1
#&gt; SRR1655042     2       0          1  0  1  0
#&gt; SRR1655043     2       0          1  0  1  0
#&gt; SRR1655044     2       0          1  0  1  0
#&gt; SRR1655045     2       0          1  0  1  0
#&gt; SRR1655046     2       0          1  0  1  0
#&gt; SRR1655047     2       0          1  0  1  0
#&gt; SRR1655048     2       0          1  0  1  0
#&gt; SRR1655049     2       0          1  0  1  0
#&gt; SRR1655050     2       0          1  0  1  0
#&gt; SRR1655051     2       0          1  0  1  0
#&gt; SRR1655052     2       0          1  0  1  0
#&gt; SRR1655053     2       0          1  0  1  0
#&gt; SRR1655054     2       0          1  0  1  0
#&gt; SRR1655055     2       0          1  0  1  0
#&gt; SRR1655056     2       0          1  0  1  0
#&gt; SRR1655057     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4
#&gt; SRR1655002     1       0          1  1  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1
#&gt; SRR1655027     4       0          1  0  0  0  1
#&gt; SRR1655028     4       0          1  0  0  0  1
#&gt; SRR1655029     4       0          1  0  0  0  1
#&gt; SRR1655030     4       0          1  0  0  0  1
#&gt; SRR1655031     4       0          1  0  0  0  1
#&gt; SRR1655032     4       0          1  0  0  0  1
#&gt; SRR1655033     4       0          1  0  0  0  1
#&gt; SRR1655034     3       0          1  0  0  1  0
#&gt; SRR1655035     3       0          1  0  0  1  0
#&gt; SRR1655036     3       0          1  0  0  1  0
#&gt; SRR1655037     3       0          1  0  0  1  0
#&gt; SRR1655038     3       0          1  0  0  1  0
#&gt; SRR1655039     3       0          1  0  0  1  0
#&gt; SRR1655040     3       0          1  0  0  1  0
#&gt; SRR1655041     3       0          1  0  0  1  0
#&gt; SRR1655042     2       0          1  0  1  0  0
#&gt; SRR1655043     2       0          1  0  1  0  0
#&gt; SRR1655044     2       0          1  0  1  0  0
#&gt; SRR1655045     2       0          1  0  1  0  0
#&gt; SRR1655046     2       0          1  0  1  0  0
#&gt; SRR1655047     2       0          1  0  1  0  0
#&gt; SRR1655048     2       0          1  0  1  0  0
#&gt; SRR1655049     2       0          1  0  1  0  0
#&gt; SRR1655050     4       0          1  0  0  0  1
#&gt; SRR1655051     4       0          1  0  0  0  1
#&gt; SRR1655052     4       0          1  0  0  0  1
#&gt; SRR1655053     4       0          1  0  0  0  1
#&gt; SRR1655054     4       0          1  0  0  0  1
#&gt; SRR1655055     4       0          1  0  0  0  1
#&gt; SRR1655056     4       0          1  0  0  0  1
#&gt; SRR1655057     4       0          1  0  0  0  1
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5
#&gt; SRR1655002     1       0          1  1  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0
#&gt; SRR1655026     5       0          1  0  0  0  0  1
#&gt; SRR1655027     5       0          1  0  0  0  0  1
#&gt; SRR1655028     5       0          1  0  0  0  0  1
#&gt; SRR1655029     5       0          1  0  0  0  0  1
#&gt; SRR1655030     5       0          1  0  0  0  0  1
#&gt; SRR1655031     5       0          1  0  0  0  0  1
#&gt; SRR1655032     5       0          1  0  0  0  0  1
#&gt; SRR1655033     5       0          1  0  0  0  0  1
#&gt; SRR1655034     3       0          1  0  0  1  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0
#&gt; SRR1655042     4       0          1  0  0  0  1  0
#&gt; SRR1655043     4       0          1  0  0  0  1  0
#&gt; SRR1655044     4       0          1  0  0  0  1  0
#&gt; SRR1655045     4       0          1  0  0  0  1  0
#&gt; SRR1655046     4       0          1  0  0  0  1  0
#&gt; SRR1655047     4       0          1  0  0  0  1  0
#&gt; SRR1655048     4       0          1  0  0  0  1  0
#&gt; SRR1655049     4       0          1  0  0  0  1  0
#&gt; SRR1655050     5       0          1  0  0  0  0  1
#&gt; SRR1655051     5       0          1  0  0  0  0  1
#&gt; SRR1655052     5       0          1  0  0  0  0  1
#&gt; SRR1655053     5       0          1  0  0  0  0  1
#&gt; SRR1655054     5       0          1  0  0  0  0  1
#&gt; SRR1655055     5       0          1  0  0  0  0  1
#&gt; SRR1655056     5       0          1  0  0  0  0  1
#&gt; SRR1655057     5       0          1  0  0  0  0  1
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     6       0          1  0  0  0  0  0  1
#&gt; SRR1655049     6       0          1  0  0  0  0  0  1
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.844           0.957       0.963         0.3211 0.701   0.701
#> 3 3 0.418           0.678       0.805         0.8175 0.688   0.556
#> 4 4 0.550           0.757       0.773         0.1765 0.803   0.542
#> 5 5 0.717           0.813       0.788         0.1133 0.927   0.736
#> 6 6 0.728           0.887       0.693         0.0495 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.204      1.000 0.968 0.032
#&gt; SRR1655003     1   0.204      1.000 0.968 0.032
#&gt; SRR1655004     1   0.204      1.000 0.968 0.032
#&gt; SRR1655005     1   0.204      1.000 0.968 0.032
#&gt; SRR1655006     1   0.204      1.000 0.968 0.032
#&gt; SRR1655007     1   0.204      1.000 0.968 0.032
#&gt; SRR1655008     1   0.204      1.000 0.968 0.032
#&gt; SRR1655009     1   0.204      1.000 0.968 0.032
#&gt; SRR1655010     1   0.204      1.000 0.968 0.032
#&gt; SRR1655011     1   0.204      1.000 0.968 0.032
#&gt; SRR1655012     2   0.163      0.963 0.024 0.976
#&gt; SRR1655013     2   0.163      0.963 0.024 0.976
#&gt; SRR1655014     2   0.163      0.963 0.024 0.976
#&gt; SRR1655015     2   0.163      0.963 0.024 0.976
#&gt; SRR1655016     2   0.163      0.963 0.024 0.976
#&gt; SRR1655017     2   0.163      0.963 0.024 0.976
#&gt; SRR1655018     2   0.163      0.963 0.024 0.976
#&gt; SRR1655019     2   0.163      0.963 0.024 0.976
#&gt; SRR1655020     2   0.163      0.963 0.024 0.976
#&gt; SRR1655021     2   0.163      0.963 0.024 0.976
#&gt; SRR1655022     2   0.163      0.963 0.024 0.976
#&gt; SRR1655023     2   0.163      0.963 0.024 0.976
#&gt; SRR1655024     2   0.163      0.963 0.024 0.976
#&gt; SRR1655025     2   0.163      0.963 0.024 0.976
#&gt; SRR1655026     2   0.000      0.960 0.000 1.000
#&gt; SRR1655027     2   0.000      0.960 0.000 1.000
#&gt; SRR1655028     2   0.000      0.960 0.000 1.000
#&gt; SRR1655029     2   0.000      0.960 0.000 1.000
#&gt; SRR1655030     2   0.000      0.960 0.000 1.000
#&gt; SRR1655031     2   0.000      0.960 0.000 1.000
#&gt; SRR1655032     2   0.000      0.960 0.000 1.000
#&gt; SRR1655033     2   0.000      0.960 0.000 1.000
#&gt; SRR1655034     2   0.541      0.892 0.124 0.876
#&gt; SRR1655035     2   0.541      0.892 0.124 0.876
#&gt; SRR1655036     2   0.541      0.892 0.124 0.876
#&gt; SRR1655037     2   0.541      0.892 0.124 0.876
#&gt; SRR1655038     2   0.541      0.892 0.124 0.876
#&gt; SRR1655039     2   0.541      0.892 0.124 0.876
#&gt; SRR1655040     2   0.541      0.892 0.124 0.876
#&gt; SRR1655041     2   0.541      0.892 0.124 0.876
#&gt; SRR1655042     2   0.163      0.963 0.024 0.976
#&gt; SRR1655043     2   0.163      0.963 0.024 0.976
#&gt; SRR1655044     2   0.163      0.963 0.024 0.976
#&gt; SRR1655045     2   0.163      0.963 0.024 0.976
#&gt; SRR1655046     2   0.163      0.963 0.024 0.976
#&gt; SRR1655047     2   0.163      0.963 0.024 0.976
#&gt; SRR1655048     2   0.163      0.963 0.024 0.976
#&gt; SRR1655049     2   0.163      0.963 0.024 0.976
#&gt; SRR1655050     2   0.204      0.949 0.032 0.968
#&gt; SRR1655051     2   0.204      0.949 0.032 0.968
#&gt; SRR1655052     2   0.204      0.949 0.032 0.968
#&gt; SRR1655053     2   0.204      0.949 0.032 0.968
#&gt; SRR1655054     2   0.204      0.949 0.032 0.968
#&gt; SRR1655055     2   0.204      0.949 0.032 0.968
#&gt; SRR1655056     2   0.204      0.949 0.032 0.968
#&gt; SRR1655057     2   0.204      0.949 0.032 0.968
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1  0.1964      0.978 0.944 0.000 0.056
#&gt; SRR1655003     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1655006     1  0.1964      0.978 0.944 0.000 0.056
#&gt; SRR1655007     1  0.2066      0.978 0.940 0.000 0.060
#&gt; SRR1655008     1  0.0237      0.978 0.996 0.000 0.004
#&gt; SRR1655009     1  0.0237      0.978 0.996 0.000 0.004
#&gt; SRR1655010     1  0.2066      0.978 0.940 0.000 0.060
#&gt; SRR1655011     1  0.2066      0.978 0.940 0.000 0.060
#&gt; SRR1655012     2  0.1031      0.745 0.000 0.976 0.024
#&gt; SRR1655013     2  0.1031      0.745 0.000 0.976 0.024
#&gt; SRR1655014     2  0.1163      0.743 0.000 0.972 0.028
#&gt; SRR1655015     2  0.1163      0.743 0.000 0.972 0.028
#&gt; SRR1655016     2  0.1031      0.745 0.000 0.976 0.024
#&gt; SRR1655017     2  0.1031      0.745 0.000 0.976 0.024
#&gt; SRR1655018     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655019     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655020     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655021     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655022     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655023     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655024     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655025     2  0.1031      0.744 0.000 0.976 0.024
#&gt; SRR1655026     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655027     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655028     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655029     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655030     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655031     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655032     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655033     2  0.6291      0.273 0.000 0.532 0.468
#&gt; SRR1655034     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655035     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655036     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655037     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655038     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655039     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655040     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655041     3  0.7256      0.685 0.124 0.164 0.712
#&gt; SRR1655042     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655043     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655044     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655045     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655046     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655047     2  0.4346      0.690 0.000 0.816 0.184
#&gt; SRR1655048     2  0.1964      0.735 0.000 0.944 0.056
#&gt; SRR1655049     2  0.1964      0.735 0.000 0.944 0.056
#&gt; SRR1655050     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655051     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655052     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655053     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655054     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655055     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655056     3  0.5465      0.562 0.000 0.288 0.712
#&gt; SRR1655057     3  0.5465      0.562 0.000 0.288 0.712
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1655002     1  0.0817      0.944 0.976 0.000 0.024 0.000
#&gt; SRR1655003     1  0.3247      0.945 0.880 0.000 0.060 0.060
#&gt; SRR1655004     1  0.3247      0.945 0.880 0.000 0.060 0.060
#&gt; SRR1655005     1  0.3247      0.945 0.880 0.000 0.060 0.060
#&gt; SRR1655006     1  0.0817      0.944 0.976 0.000 0.024 0.000
#&gt; SRR1655007     1  0.0469      0.944 0.988 0.000 0.000 0.012
#&gt; SRR1655008     1  0.2996      0.945 0.892 0.000 0.044 0.064
#&gt; SRR1655009     1  0.2996      0.945 0.892 0.000 0.044 0.064
#&gt; SRR1655010     1  0.0469      0.944 0.988 0.000 0.000 0.012
#&gt; SRR1655011     1  0.0469      0.944 0.988 0.000 0.000 0.012
#&gt; SRR1655012     2  0.2021      0.756 0.000 0.932 0.056 0.012
#&gt; SRR1655013     2  0.2021      0.756 0.000 0.932 0.056 0.012
#&gt; SRR1655014     2  0.1557      0.753 0.000 0.944 0.056 0.000
#&gt; SRR1655015     2  0.1557      0.753 0.000 0.944 0.056 0.000
#&gt; SRR1655016     2  0.2021      0.756 0.000 0.932 0.056 0.012
#&gt; SRR1655017     2  0.2021      0.756 0.000 0.932 0.056 0.012
#&gt; SRR1655018     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655019     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655020     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655021     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655022     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655023     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655024     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655025     2  0.0188      0.770 0.000 0.996 0.000 0.004
#&gt; SRR1655026     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655027     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655028     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655029     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655030     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655031     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655032     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655033     4  0.3907      0.715 0.000 0.232 0.000 0.768
#&gt; SRR1655034     3  0.6247      0.971 0.052 0.088 0.728 0.132
#&gt; SRR1655035     3  0.6247      0.971 0.052 0.088 0.728 0.132
#&gt; SRR1655036     3  0.6568      0.977 0.052 0.088 0.700 0.160
#&gt; SRR1655037     3  0.6568      0.977 0.052 0.088 0.700 0.160
#&gt; SRR1655038     3  0.6041      0.976 0.052 0.088 0.744 0.116
#&gt; SRR1655039     3  0.6041      0.976 0.052 0.088 0.744 0.116
#&gt; SRR1655040     3  0.6568      0.977 0.052 0.088 0.700 0.160
#&gt; SRR1655041     3  0.6568      0.977 0.052 0.088 0.700 0.160
#&gt; SRR1655042     2  0.6747      0.343 0.000 0.528 0.100 0.372
#&gt; SRR1655043     2  0.6747      0.343 0.000 0.528 0.100 0.372
#&gt; SRR1655044     2  0.6766      0.326 0.000 0.520 0.100 0.380
#&gt; SRR1655045     2  0.6766      0.326 0.000 0.520 0.100 0.380
#&gt; SRR1655046     2  0.6766      0.326 0.000 0.520 0.100 0.380
#&gt; SRR1655047     2  0.6766      0.326 0.000 0.520 0.100 0.380
#&gt; SRR1655048     2  0.5900      0.571 0.000 0.684 0.096 0.220
#&gt; SRR1655049     2  0.5900      0.571 0.000 0.684 0.096 0.220
#&gt; SRR1655050     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655051     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655052     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655053     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655054     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655055     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655056     4  0.6621      0.703 0.000 0.140 0.244 0.616
#&gt; SRR1655057     4  0.6621      0.703 0.000 0.140 0.244 0.616
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.1153      0.918 0.964 0.000 0.008 0.024 0.004
#&gt; SRR1655003     1  0.4213      0.918 0.808 0.000 0.036 0.108 0.048
#&gt; SRR1655004     1  0.4162      0.918 0.812 0.000 0.036 0.104 0.048
#&gt; SRR1655005     1  0.4162      0.918 0.812 0.000 0.036 0.104 0.048
#&gt; SRR1655006     1  0.1153      0.918 0.964 0.000 0.008 0.024 0.004
#&gt; SRR1655007     1  0.0324      0.918 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655008     1  0.3826      0.918 0.836 0.000 0.032 0.080 0.052
#&gt; SRR1655009     1  0.3826      0.918 0.836 0.000 0.032 0.080 0.052
#&gt; SRR1655010     1  0.0324      0.918 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655011     1  0.0324      0.918 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655012     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655013     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655014     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655015     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655016     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655017     2  0.1399      0.833 0.000 0.952 0.020 0.000 0.028
#&gt; SRR1655018     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655019     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655020     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655021     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655022     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655023     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655024     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655025     2  0.2648      0.866 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1655026     5  0.5364      0.588 0.000 0.076 0.004 0.284 0.636
#&gt; SRR1655027     5  0.5364      0.588 0.000 0.076 0.004 0.284 0.636
#&gt; SRR1655028     5  0.5252      0.588 0.000 0.076 0.000 0.292 0.632
#&gt; SRR1655029     5  0.5252      0.588 0.000 0.076 0.000 0.292 0.632
#&gt; SRR1655030     5  0.5364      0.588 0.000 0.076 0.004 0.284 0.636
#&gt; SRR1655031     5  0.5364      0.588 0.000 0.076 0.004 0.284 0.636
#&gt; SRR1655032     5  0.5271      0.587 0.000 0.076 0.000 0.296 0.628
#&gt; SRR1655033     5  0.5271      0.587 0.000 0.076 0.000 0.296 0.628
#&gt; SRR1655034     3  0.3629      0.960 0.032 0.044 0.860 0.012 0.052
#&gt; SRR1655035     3  0.3629      0.960 0.032 0.044 0.860 0.012 0.052
#&gt; SRR1655036     3  0.4097      0.971 0.032 0.044 0.840 0.048 0.036
#&gt; SRR1655037     3  0.4097      0.971 0.032 0.044 0.840 0.048 0.036
#&gt; SRR1655038     3  0.2772      0.970 0.032 0.044 0.896 0.000 0.028
#&gt; SRR1655039     3  0.2772      0.970 0.032 0.044 0.896 0.000 0.028
#&gt; SRR1655040     3  0.3951      0.972 0.032 0.044 0.848 0.040 0.036
#&gt; SRR1655041     3  0.3951      0.972 0.032 0.044 0.848 0.040 0.036
#&gt; SRR1655042     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655043     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655044     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655045     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655046     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655047     4  0.4850      0.928 0.000 0.224 0.000 0.700 0.076
#&gt; SRR1655048     4  0.4201      0.736 0.000 0.328 0.008 0.664 0.000
#&gt; SRR1655049     4  0.4201      0.736 0.000 0.328 0.008 0.664 0.000
#&gt; SRR1655050     5  0.5398      0.618 0.000 0.048 0.248 0.032 0.672
#&gt; SRR1655051     5  0.5398      0.618 0.000 0.048 0.248 0.032 0.672
#&gt; SRR1655052     5  0.5064      0.619 0.000 0.048 0.248 0.016 0.688
#&gt; SRR1655053     5  0.5064      0.619 0.000 0.048 0.248 0.016 0.688
#&gt; SRR1655054     5  0.5321      0.618 0.000 0.048 0.248 0.028 0.676
#&gt; SRR1655055     5  0.5321      0.618 0.000 0.048 0.248 0.028 0.676
#&gt; SRR1655056     5  0.5064      0.619 0.000 0.048 0.248 0.016 0.688
#&gt; SRR1655057     5  0.5064      0.619 0.000 0.048 0.248 0.016 0.688
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1655002     1  0.4923      0.832 0.668 0.000 0.004 0.000 0.184 0.144
#&gt; SRR1655003     1  0.0146      0.833 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1655004     1  0.0000      0.833 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      0.833 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.4923      0.832 0.668 0.000 0.004 0.000 0.184 0.144
#&gt; SRR1655007     1  0.4934      0.832 0.628 0.000 0.000 0.000 0.264 0.108
#&gt; SRR1655008     1  0.2215      0.831 0.916 0.000 0.020 0.008 0.024 0.032
#&gt; SRR1655009     1  0.2215      0.831 0.916 0.000 0.020 0.008 0.024 0.032
#&gt; SRR1655010     1  0.4934      0.832 0.628 0.000 0.000 0.000 0.264 0.108
#&gt; SRR1655011     1  0.4934      0.832 0.628 0.000 0.000 0.000 0.264 0.108
#&gt; SRR1655012     2  0.0291      0.796 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655013     2  0.0291      0.796 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655014     2  0.0632      0.796 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; SRR1655015     2  0.0632      0.796 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; SRR1655016     2  0.0291      0.796 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655017     2  0.0291      0.796 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1655018     2  0.4699      0.850 0.000 0.704 0.012 0.000 0.100 0.184
#&gt; SRR1655019     2  0.4699      0.850 0.000 0.704 0.012 0.000 0.100 0.184
#&gt; SRR1655020     2  0.4699      0.850 0.000 0.704 0.012 0.000 0.100 0.184
#&gt; SRR1655021     2  0.4699      0.850 0.000 0.704 0.012 0.000 0.100 0.184
#&gt; SRR1655022     2  0.4647      0.850 0.000 0.704 0.008 0.000 0.104 0.184
#&gt; SRR1655023     2  0.4647      0.850 0.000 0.704 0.008 0.000 0.104 0.184
#&gt; SRR1655024     2  0.4676      0.848 0.000 0.700 0.008 0.000 0.104 0.188
#&gt; SRR1655025     2  0.4676      0.848 0.000 0.700 0.008 0.000 0.104 0.188
#&gt; SRR1655026     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655027     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655028     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655029     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655030     4  0.0858      0.996 0.000 0.028 0.000 0.968 0.004 0.000
#&gt; SRR1655031     4  0.0858      0.996 0.000 0.028 0.000 0.968 0.004 0.000
#&gt; SRR1655032     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655033     4  0.0713      0.999 0.000 0.028 0.000 0.972 0.000 0.000
#&gt; SRR1655034     3  0.2921      0.921 0.012 0.016 0.888 0.024 0.024 0.036
#&gt; SRR1655035     3  0.2921      0.921 0.012 0.016 0.888 0.024 0.024 0.036
#&gt; SRR1655036     3  0.3202      0.947 0.012 0.016 0.868 0.016 0.024 0.064
#&gt; SRR1655037     3  0.3202      0.947 0.012 0.016 0.868 0.016 0.024 0.064
#&gt; SRR1655038     3  0.1533      0.941 0.012 0.016 0.948 0.016 0.008 0.000
#&gt; SRR1655039     3  0.1533      0.941 0.012 0.016 0.948 0.016 0.008 0.000
#&gt; SRR1655040     3  0.3202      0.947 0.012 0.016 0.868 0.016 0.024 0.064
#&gt; SRR1655041     3  0.3202      0.947 0.012 0.016 0.868 0.016 0.024 0.064
#&gt; SRR1655042     6  0.5608      0.873 0.000 0.128 0.004 0.388 0.000 0.480
#&gt; SRR1655043     6  0.5608      0.873 0.000 0.128 0.004 0.388 0.000 0.480
#&gt; SRR1655044     6  0.5479      0.874 0.000 0.128 0.000 0.388 0.000 0.484
#&gt; SRR1655045     6  0.5479      0.874 0.000 0.128 0.000 0.388 0.000 0.484
#&gt; SRR1655046     6  0.5479      0.874 0.000 0.128 0.000 0.388 0.000 0.484
#&gt; SRR1655047     6  0.5479      0.874 0.000 0.128 0.000 0.388 0.000 0.484
#&gt; SRR1655048     6  0.6429      0.684 0.000 0.192 0.004 0.200 0.056 0.548
#&gt; SRR1655049     6  0.6429      0.684 0.000 0.192 0.004 0.200 0.056 0.548
#&gt; SRR1655050     5  0.6219      0.950 0.000 0.004 0.108 0.380 0.468 0.040
#&gt; SRR1655051     5  0.6219      0.950 0.000 0.004 0.108 0.380 0.468 0.040
#&gt; SRR1655052     5  0.6514      0.963 0.000 0.004 0.108 0.384 0.440 0.064
#&gt; SRR1655053     5  0.6514      0.963 0.000 0.004 0.108 0.384 0.440 0.064
#&gt; SRR1655054     5  0.5902      0.960 0.000 0.004 0.108 0.376 0.492 0.020
#&gt; SRR1655055     5  0.5902      0.960 0.000 0.004 0.108 0.376 0.492 0.020
#&gt; SRR1655056     5  0.6514      0.963 0.000 0.004 0.108 0.384 0.440 0.064
#&gt; SRR1655057     5  0.6514      0.963 0.000 0.004 0.108 0.384 0.440 0.064
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4447 0.556   0.556
#> 3 3 0.657           0.765       0.864         0.4519 0.771   0.589
#> 4 4 0.764           0.866       0.925         0.1263 0.948   0.841
#> 5 5 0.917           0.901       0.937         0.0968 0.844   0.508
#> 6 6 1.000           0.997       0.993         0.0518 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     1       0          1  1  0
#&gt; SRR1655035     1       0          1  1  0
#&gt; SRR1655036     1       0          1  1  0
#&gt; SRR1655037     1       0          1  1  0
#&gt; SRR1655038     1       0          1  1  0
#&gt; SRR1655039     1       0          1  1  0
#&gt; SRR1655040     1       0          1  1  0
#&gt; SRR1655041     1       0          1  1  0
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655003     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655004     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655005     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655006     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655007     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655008     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655009     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655010     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655011     1   0.000      0.771 1.000 0.000 0.000
#&gt; SRR1655012     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655013     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655014     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655015     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655016     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655017     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655018     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655019     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655020     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655021     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655022     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655023     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655024     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655025     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655026     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655027     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655028     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655029     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655030     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655031     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655032     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655033     3   0.618      0.546 0.000 0.416 0.584
#&gt; SRR1655034     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655035     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655036     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655037     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655038     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655039     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655040     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655041     1   0.629      0.643 0.536 0.000 0.464
#&gt; SRR1655042     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655043     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655044     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655045     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655046     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655047     2   0.296      0.884 0.000 0.900 0.100
#&gt; SRR1655048     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655049     2   0.000      0.960 0.000 1.000 0.000
#&gt; SRR1655050     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655051     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655052     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655053     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655054     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655055     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655056     3   0.000      0.617 0.000 0.000 1.000
#&gt; SRR1655057     3   0.000      0.617 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.840 0.000 1.000 0.000 0.000
#&gt; SRR1655026     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655027     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655028     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655029     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655030     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655031     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655032     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655033     4  0.0707      0.916 0.000 0.020 0.000 0.980
#&gt; SRR1655034     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655035     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655036     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655037     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655038     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655039     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655040     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655041     3  0.0188      1.000 0.004 0.000 0.996 0.000
#&gt; SRR1655042     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655043     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655044     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655045     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655046     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655047     2  0.4972      0.424 0.000 0.544 0.000 0.456
#&gt; SRR1655048     2  0.2589      0.790 0.000 0.884 0.000 0.116
#&gt; SRR1655049     2  0.2589      0.790 0.000 0.884 0.000 0.116
#&gt; SRR1655050     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655051     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655052     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655053     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655054     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655055     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655056     4  0.2589      0.910 0.000 0.000 0.116 0.884
#&gt; SRR1655057     4  0.2589      0.910 0.000 0.000 0.116 0.884
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      1.000  0 1.000 0.000 0.000 0.000
#&gt; SRR1655026     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655027     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655028     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655029     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655030     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655031     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655032     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655033     4  0.4249      0.593  0 0.000 0.000 0.568 0.432
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655042     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655043     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655044     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655045     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655046     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655047     4  0.0162      0.716  0 0.004 0.000 0.996 0.000
#&gt; SRR1655048     4  0.0510      0.709  0 0.016 0.000 0.984 0.000
#&gt; SRR1655049     4  0.0510      0.709  0 0.016 0.000 0.984 0.000
#&gt; SRR1655050     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655051     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655052     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655053     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655054     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655055     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655056     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
#&gt; SRR1655057     5  0.0162      1.000  0 0.000 0.004 0.000 0.996
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4   p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.00 0.000
#&gt; SRR1655012     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655013     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655014     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655015     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655016     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655017     2  0.0806      0.986  0 0.972  0 0.000 0.02 0.008
#&gt; SRR1655018     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655019     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655020     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655021     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655022     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655023     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655024     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655025     2  0.0000      0.990  0 1.000  0 0.000 0.00 0.000
#&gt; SRR1655026     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655027     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655028     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655029     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655030     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655031     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655032     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655033     4  0.0000      1.000  0 0.000  0 1.000 0.00 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.00 0.000
#&gt; SRR1655042     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655043     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655044     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655045     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655046     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655047     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655048     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655049     6  0.0260      1.000  0 0.000  0 0.008 0.00 0.992
#&gt; SRR1655050     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655051     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655052     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655053     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655054     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655055     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655056     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
#&gt; SRR1655057     5  0.0547      1.000  0 0.000  0 0.020 0.98 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.651           0.748       0.847         0.8786 0.803   0.719
#> 4 4 1.000           0.963       0.986         0.2947 0.766   0.536
#> 5 5 1.000           0.960       0.985         0.0912 0.938   0.769
#> 6 6 1.000           1.000       1.000         0.0560 0.932   0.690
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 4 5
```

There is also optional best $k$ = 2 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.000
#&gt; SRR1655012     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655013     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655014     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655015     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655016     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655017     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655018     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655019     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655020     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655021     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655022     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655023     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655024     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655025     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655026     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655027     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655028     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655029     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655030     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655031     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655032     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655033     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655034     3   0.611      0.930  0 0.396 0.604
#&gt; SRR1655035     3   0.586      0.859  0 0.344 0.656
#&gt; SRR1655036     3   0.625      0.966  0 0.444 0.556
#&gt; SRR1655037     3   0.625      0.966  0 0.444 0.556
#&gt; SRR1655038     3   0.624      0.969  0 0.440 0.560
#&gt; SRR1655039     3   0.624      0.969  0 0.440 0.560
#&gt; SRR1655040     3   0.624      0.969  0 0.440 0.560
#&gt; SRR1655041     3   0.624      0.969  0 0.440 0.560
#&gt; SRR1655042     2   0.216      0.581  0 0.936 0.064
#&gt; SRR1655043     2   0.296      0.596  0 0.900 0.100
#&gt; SRR1655044     2   0.624      0.712  0 0.560 0.440
#&gt; SRR1655045     2   0.624      0.712  0 0.560 0.440
#&gt; SRR1655046     2   0.624      0.712  0 0.560 0.440
#&gt; SRR1655047     2   0.624      0.712  0 0.560 0.440
#&gt; SRR1655048     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655049     2   0.000      0.554  0 1.000 0.000
#&gt; SRR1655050     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655051     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655052     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655053     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655054     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655055     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655056     2   0.629      0.715  0 0.536 0.464
#&gt; SRR1655057     2   0.629      0.715  0 0.536 0.464
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655013     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655014     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655015     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655016     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655017     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655018     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655019     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655020     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655021     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655022     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655023     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655024     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655025     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655026     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655027     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655028     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655029     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655030     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655031     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655032     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655033     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2   0.471      0.464  0 0.640  0 0.360
#&gt; SRR1655043     2   0.489      0.339  0 0.588  0 0.412
#&gt; SRR1655044     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655045     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655046     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655047     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655048     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655049     2   0.000      0.944  0 1.000  0 0.000
#&gt; SRR1655050     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655051     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655052     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655053     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655054     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655055     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655056     4   0.000      1.000  0 0.000  0 1.000
#&gt; SRR1655057     4   0.000      1.000  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5
#&gt; SRR1655002     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655003     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655004     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655005     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655006     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655007     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655008     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655009     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655010     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655011     1   0.000      1.000  1 0.000  0 0.000  0
#&gt; SRR1655012     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655013     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655014     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655015     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655016     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655017     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655018     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655019     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655020     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655021     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655022     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655023     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655024     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655025     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655026     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655027     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655028     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655029     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655030     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655031     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655032     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655033     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655034     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655035     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655036     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655037     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655038     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655039     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655040     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655041     3   0.000      1.000  0 0.000  1 0.000  0
#&gt; SRR1655042     2   0.423      0.307  0 0.576  0 0.424  0
#&gt; SRR1655043     2   0.423      0.318  0 0.580  0 0.420  0
#&gt; SRR1655044     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655045     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655046     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655047     4   0.000      1.000  0 0.000  0 1.000  0
#&gt; SRR1655048     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655049     2   0.000      0.945  0 1.000  0 0.000  0
#&gt; SRR1655050     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655051     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655052     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655053     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655054     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655055     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655056     5   0.000      1.000  0 0.000  0 0.000  1
#&gt; SRR1655057     5   0.000      1.000  0 0.000  0 0.000  1
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     6       0          1  0  0  0  0  0  1
#&gt; SRR1655049     6       0          1  0  0  0  0  0  1
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.751           0.943       0.970          0.330 0.701   0.701
#> 3 3 0.831           0.871       0.941          0.880 0.688   0.556
#> 4 4 1.000           0.998       0.999          0.168 0.886   0.707
#> 5 5 1.000           1.000       1.000          0.100 0.927   0.736
#> 6 6 1.000           1.000       1.000          0.039 0.969   0.846
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 4 5
```

There is also optional best $k$ = 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1   0.000      1.000 1.000 0.000
#&gt; SRR1655003     1   0.000      1.000 1.000 0.000
#&gt; SRR1655004     1   0.000      1.000 1.000 0.000
#&gt; SRR1655005     1   0.000      1.000 1.000 0.000
#&gt; SRR1655006     1   0.000      1.000 1.000 0.000
#&gt; SRR1655007     1   0.000      1.000 1.000 0.000
#&gt; SRR1655008     1   0.000      1.000 1.000 0.000
#&gt; SRR1655009     1   0.000      1.000 1.000 0.000
#&gt; SRR1655010     1   0.000      1.000 1.000 0.000
#&gt; SRR1655011     1   0.000      1.000 1.000 0.000
#&gt; SRR1655012     2   0.000      0.960 0.000 1.000
#&gt; SRR1655013     2   0.000      0.960 0.000 1.000
#&gt; SRR1655014     2   0.000      0.960 0.000 1.000
#&gt; SRR1655015     2   0.000      0.960 0.000 1.000
#&gt; SRR1655016     2   0.000      0.960 0.000 1.000
#&gt; SRR1655017     2   0.000      0.960 0.000 1.000
#&gt; SRR1655018     2   0.000      0.960 0.000 1.000
#&gt; SRR1655019     2   0.000      0.960 0.000 1.000
#&gt; SRR1655020     2   0.000      0.960 0.000 1.000
#&gt; SRR1655021     2   0.000      0.960 0.000 1.000
#&gt; SRR1655022     2   0.000      0.960 0.000 1.000
#&gt; SRR1655023     2   0.000      0.960 0.000 1.000
#&gt; SRR1655024     2   0.000      0.960 0.000 1.000
#&gt; SRR1655025     2   0.000      0.960 0.000 1.000
#&gt; SRR1655026     2   0.141      0.951 0.020 0.980
#&gt; SRR1655027     2   0.141      0.951 0.020 0.980
#&gt; SRR1655028     2   0.141      0.951 0.020 0.980
#&gt; SRR1655029     2   0.141      0.951 0.020 0.980
#&gt; SRR1655030     2   0.141      0.951 0.020 0.980
#&gt; SRR1655031     2   0.141      0.951 0.020 0.980
#&gt; SRR1655032     2   0.141      0.951 0.020 0.980
#&gt; SRR1655033     2   0.141      0.951 0.020 0.980
#&gt; SRR1655034     2   0.706      0.800 0.192 0.808
#&gt; SRR1655035     2   0.706      0.800 0.192 0.808
#&gt; SRR1655036     2   0.706      0.800 0.192 0.808
#&gt; SRR1655037     2   0.706      0.800 0.192 0.808
#&gt; SRR1655038     2   0.706      0.800 0.192 0.808
#&gt; SRR1655039     2   0.706      0.800 0.192 0.808
#&gt; SRR1655040     2   0.706      0.800 0.192 0.808
#&gt; SRR1655041     2   0.706      0.800 0.192 0.808
#&gt; SRR1655042     2   0.000      0.960 0.000 1.000
#&gt; SRR1655043     2   0.000      0.960 0.000 1.000
#&gt; SRR1655044     2   0.000      0.960 0.000 1.000
#&gt; SRR1655045     2   0.000      0.960 0.000 1.000
#&gt; SRR1655046     2   0.000      0.960 0.000 1.000
#&gt; SRR1655047     2   0.000      0.960 0.000 1.000
#&gt; SRR1655048     2   0.000      0.960 0.000 1.000
#&gt; SRR1655049     2   0.000      0.960 0.000 1.000
#&gt; SRR1655050     2   0.000      0.960 0.000 1.000
#&gt; SRR1655051     2   0.000      0.960 0.000 1.000
#&gt; SRR1655052     2   0.000      0.960 0.000 1.000
#&gt; SRR1655053     2   0.000      0.960 0.000 1.000
#&gt; SRR1655054     2   0.000      0.960 0.000 1.000
#&gt; SRR1655055     2   0.000      0.960 0.000 1.000
#&gt; SRR1655056     2   0.000      0.960 0.000 1.000
#&gt; SRR1655057     2   0.000      0.960 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655013     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655014     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655015     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655016     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655017     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655018     2  0.1753      0.954  0 0.952 0.048
#&gt; SRR1655019     2  0.1753      0.954  0 0.952 0.048
#&gt; SRR1655020     2  0.0892      0.971  0 0.980 0.020
#&gt; SRR1655021     2  0.0892      0.971  0 0.980 0.020
#&gt; SRR1655022     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655023     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655024     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655025     2  0.0000      0.979  0 1.000 0.000
#&gt; SRR1655026     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655027     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655028     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655029     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655030     3  0.2165      0.863  0 0.064 0.936
#&gt; SRR1655031     3  0.2165      0.863  0 0.064 0.936
#&gt; SRR1655032     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655033     3  0.1529      0.878  0 0.040 0.960
#&gt; SRR1655034     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655035     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655036     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655037     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655038     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655039     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655040     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655041     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655042     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655043     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655044     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655045     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655046     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655047     3  0.6252      0.352  0 0.444 0.556
#&gt; SRR1655048     2  0.2066      0.939  0 0.940 0.060
#&gt; SRR1655049     2  0.2066      0.939  0 0.940 0.060
#&gt; SRR1655050     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655051     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655052     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655053     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655054     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655055     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655056     3  0.0000      0.884  0 0.000 1.000
#&gt; SRR1655057     3  0.0000      0.884  0 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655013     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655014     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655015     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655016     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655017     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655018     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655019     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655020     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655021     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655022     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655023     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655024     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655025     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655026     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655027     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655028     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655029     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655030     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655031     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655032     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655033     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     4  0.0921      0.969  0 0.028  0 0.972
#&gt; SRR1655043     4  0.0921      0.969  0 0.028  0 0.972
#&gt; SRR1655044     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655045     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655046     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655047     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655048     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655049     2  0.0000      1.000  0 1.000  0 0.000
#&gt; SRR1655050     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655051     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655052     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655053     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655054     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655055     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655056     4  0.0000      0.997  0 0.000  0 1.000
#&gt; SRR1655057     4  0.0000      0.997  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5
#&gt; SRR1655002     1       0          1  1  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0
#&gt; SRR1655042     4       0          1  0  0  0  1  0
#&gt; SRR1655043     4       0          1  0  0  0  1  0
#&gt; SRR1655044     4       0          1  0  0  0  1  0
#&gt; SRR1655045     4       0          1  0  0  0  1  0
#&gt; SRR1655046     4       0          1  0  0  0  1  0
#&gt; SRR1655047     4       0          1  0  0  0  1  0
#&gt; SRR1655048     2       0          1  0  1  0  0  0
#&gt; SRR1655049     2       0          1  0  1  0  0  0
#&gt; SRR1655050     5       0          1  0  0  0  0  1
#&gt; SRR1655051     5       0          1  0  0  0  0  1
#&gt; SRR1655052     5       0          1  0  0  0  0  1
#&gt; SRR1655053     5       0          1  0  0  0  0  1
#&gt; SRR1655054     5       0          1  0  0  0  0  1
#&gt; SRR1655055     5       0          1  0  0  0  0  1
#&gt; SRR1655056     5       0          1  0  0  0  0  1
#&gt; SRR1655057     5       0          1  0  0  0  0  1
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; SRR1655002     1       0          1  1  0  0  0  0  0
#&gt; SRR1655003     1       0          1  1  0  0  0  0  0
#&gt; SRR1655004     1       0          1  1  0  0  0  0  0
#&gt; SRR1655005     1       0          1  1  0  0  0  0  0
#&gt; SRR1655006     1       0          1  1  0  0  0  0  0
#&gt; SRR1655007     1       0          1  1  0  0  0  0  0
#&gt; SRR1655008     1       0          1  1  0  0  0  0  0
#&gt; SRR1655009     1       0          1  1  0  0  0  0  0
#&gt; SRR1655010     1       0          1  1  0  0  0  0  0
#&gt; SRR1655011     1       0          1  1  0  0  0  0  0
#&gt; SRR1655012     2       0          1  0  1  0  0  0  0
#&gt; SRR1655013     2       0          1  0  1  0  0  0  0
#&gt; SRR1655014     2       0          1  0  1  0  0  0  0
#&gt; SRR1655015     2       0          1  0  1  0  0  0  0
#&gt; SRR1655016     2       0          1  0  1  0  0  0  0
#&gt; SRR1655017     2       0          1  0  1  0  0  0  0
#&gt; SRR1655018     2       0          1  0  1  0  0  0  0
#&gt; SRR1655019     2       0          1  0  1  0  0  0  0
#&gt; SRR1655020     2       0          1  0  1  0  0  0  0
#&gt; SRR1655021     2       0          1  0  1  0  0  0  0
#&gt; SRR1655022     2       0          1  0  1  0  0  0  0
#&gt; SRR1655023     2       0          1  0  1  0  0  0  0
#&gt; SRR1655024     2       0          1  0  1  0  0  0  0
#&gt; SRR1655025     2       0          1  0  1  0  0  0  0
#&gt; SRR1655026     4       0          1  0  0  0  1  0  0
#&gt; SRR1655027     4       0          1  0  0  0  1  0  0
#&gt; SRR1655028     4       0          1  0  0  0  1  0  0
#&gt; SRR1655029     4       0          1  0  0  0  1  0  0
#&gt; SRR1655030     4       0          1  0  0  0  1  0  0
#&gt; SRR1655031     4       0          1  0  0  0  1  0  0
#&gt; SRR1655032     4       0          1  0  0  0  1  0  0
#&gt; SRR1655033     4       0          1  0  0  0  1  0  0
#&gt; SRR1655034     3       0          1  0  0  1  0  0  0
#&gt; SRR1655035     3       0          1  0  0  1  0  0  0
#&gt; SRR1655036     3       0          1  0  0  1  0  0  0
#&gt; SRR1655037     3       0          1  0  0  1  0  0  0
#&gt; SRR1655038     3       0          1  0  0  1  0  0  0
#&gt; SRR1655039     3       0          1  0  0  1  0  0  0
#&gt; SRR1655040     3       0          1  0  0  1  0  0  0
#&gt; SRR1655041     3       0          1  0  0  1  0  0  0
#&gt; SRR1655042     6       0          1  0  0  0  0  0  1
#&gt; SRR1655043     6       0          1  0  0  0  0  0  1
#&gt; SRR1655044     6       0          1  0  0  0  0  0  1
#&gt; SRR1655045     6       0          1  0  0  0  0  0  1
#&gt; SRR1655046     6       0          1  0  0  0  0  0  1
#&gt; SRR1655047     6       0          1  0  0  0  0  0  1
#&gt; SRR1655048     2       0          1  0  1  0  0  0  0
#&gt; SRR1655049     2       0          1  0  1  0  0  0  0
#&gt; SRR1655050     5       0          1  0  0  0  0  1  0
#&gt; SRR1655051     5       0          1  0  0  0  0  1  0
#&gt; SRR1655052     5       0          1  0  0  0  0  1  0
#&gt; SRR1655053     5       0          1  0  0  0  0  1  0
#&gt; SRR1655054     5       0          1  0  0  0  0  1  0
#&gt; SRR1655055     5       0          1  0  0  0  0  1  0
#&gt; SRR1655056     5       0          1  0  0  0  0  1  0
#&gt; SRR1655057     5       0          1  0  0  0  0  1  0
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2995 0.701   0.701
#> 3 3 0.906           0.965       0.979         1.0450 0.688   0.556
#> 4 4 1.000           0.987       0.989         0.1762 0.803   0.542
#> 5 5 0.985           0.980       0.978         0.1031 0.844   0.508
#> 6 6 1.000           0.999       0.991         0.0521 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655003     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655004     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655005     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655006     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655007     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655008     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655009     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655010     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655011     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655012     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655013     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655014     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655015     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655016     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655017     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655018     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655019     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655020     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655021     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655022     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655023     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655024     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655025     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655026     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655027     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655028     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655029     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655030     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655031     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655032     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655033     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655034     2  0.0376      0.996 0.004 0.996
#&gt; SRR1655035     2  0.0376      0.996 0.004 0.996
#&gt; SRR1655036     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655037     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655038     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655039     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655040     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655041     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655042     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655043     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655044     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655045     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655046     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655047     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655048     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655049     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655050     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655051     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655052     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655053     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655054     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655055     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655056     2  0.0000      1.000 0.000 1.000
#&gt; SRR1655057     2  0.0000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655013     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655014     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655015     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655016     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655017     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655018     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655019     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655020     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655021     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655022     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655023     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655024     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655025     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655026     2  0.3116      0.900  0 0.892 0.108
#&gt; SRR1655027     2  0.3116      0.900  0 0.892 0.108
#&gt; SRR1655028     2  0.3879      0.858  0 0.848 0.152
#&gt; SRR1655029     2  0.4002      0.848  0 0.840 0.160
#&gt; SRR1655030     2  0.3619      0.875  0 0.864 0.136
#&gt; SRR1655031     2  0.3412      0.886  0 0.876 0.124
#&gt; SRR1655032     2  0.2796      0.912  0 0.908 0.092
#&gt; SRR1655033     2  0.2711      0.915  0 0.912 0.088
#&gt; SRR1655034     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655035     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655036     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655037     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655038     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655039     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655040     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655041     3  0.0000      0.983  0 0.000 1.000
#&gt; SRR1655042     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655043     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655044     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655045     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655046     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655047     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655048     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655049     2  0.0000      0.965  0 1.000 0.000
#&gt; SRR1655050     3  0.0892      0.983  0 0.020 0.980
#&gt; SRR1655051     3  0.0892      0.983  0 0.020 0.980
#&gt; SRR1655052     3  0.1289      0.973  0 0.032 0.968
#&gt; SRR1655053     3  0.1031      0.980  0 0.024 0.976
#&gt; SRR1655054     3  0.1289      0.973  0 0.032 0.968
#&gt; SRR1655055     3  0.0892      0.983  0 0.020 0.980
#&gt; SRR1655056     3  0.0892      0.983  0 0.020 0.980
#&gt; SRR1655057     3  0.1031      0.980  0 0.024 0.976
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655020     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.997  0 1.000 0.000 0.000
#&gt; SRR1655026     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655027     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655028     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655029     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655030     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655031     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655032     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655033     4  0.1557      0.961  0 0.056 0.000 0.944
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1655042     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655043     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655044     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655045     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655046     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655047     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655048     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655049     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655050     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655051     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655052     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655053     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655054     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655055     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655056     4  0.0657      0.960  0 0.004 0.012 0.984
#&gt; SRR1655057     4  0.0657      0.960  0 0.004 0.012 0.984
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655012     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655013     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655014     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655015     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655016     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655017     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655018     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655019     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655020     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655021     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655022     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655023     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655024     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655025     2  0.0404      1.000  0 0.988  0 0.012 0.000
#&gt; SRR1655026     4  0.2329      0.913  0 0.000  0 0.876 0.124
#&gt; SRR1655027     4  0.2377      0.911  0 0.000  0 0.872 0.128
#&gt; SRR1655028     4  0.1965      0.928  0 0.000  0 0.904 0.096
#&gt; SRR1655029     4  0.1908      0.929  0 0.000  0 0.908 0.092
#&gt; SRR1655030     4  0.2377      0.911  0 0.000  0 0.872 0.128
#&gt; SRR1655031     4  0.2377      0.911  0 0.000  0 0.872 0.128
#&gt; SRR1655032     4  0.1571      0.938  0 0.004  0 0.936 0.060
#&gt; SRR1655033     4  0.1571      0.938  0 0.004  0 0.936 0.060
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655042     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655043     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655044     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655045     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655046     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655047     4  0.0703      0.941  0 0.024  0 0.976 0.000
#&gt; SRR1655048     4  0.1197      0.927  0 0.048  0 0.952 0.000
#&gt; SRR1655049     4  0.1197      0.927  0 0.048  0 0.952 0.000
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655018     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655019     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655020     2  0.0146      0.997  0 0.996  0 0.000 0.000 0.004
#&gt; SRR1655021     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655022     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      1.000  0 1.000  0 0.000 0.000 0.000
#&gt; SRR1655026     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655027     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655028     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655029     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655030     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655031     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655032     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655033     4  0.0291      1.000  0 0.004  0 0.992 0.004 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000 0.000
#&gt; SRR1655042     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655043     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655044     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655045     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655046     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655047     6  0.1434      0.998  0 0.012  0 0.048 0.000 0.940
#&gt; SRR1655048     6  0.1297      0.993  0 0.012  0 0.040 0.000 0.948
#&gt; SRR1655049     6  0.1297      0.993  0 0.012  0 0.040 0.000 0.948
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 1.000           0.992       0.997         0.8758 0.735   0.622
#> 4 4 0.972           0.957       0.943         0.0299 0.984   0.964
#> 5 5 1.000           0.975       0.985         0.0396 0.979   0.951
#> 6 6 1.000           0.984       0.993         0.2801 0.829   0.571
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1   p2   p3
#&gt; SRR1655002     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655003     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655004     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655005     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655006     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655007     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655008     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655009     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655010     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655011     1   0.000      1.000  1 0.00 0.00
#&gt; SRR1655012     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655013     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655014     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655015     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655016     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655017     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655018     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655019     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655020     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655021     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655022     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655023     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655024     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655025     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655026     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655027     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655028     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655029     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655030     3   0.254      0.892  0 0.08 0.92
#&gt; SRR1655031     3   0.254      0.892  0 0.08 0.92
#&gt; SRR1655032     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655033     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655034     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655035     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655036     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655037     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655038     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655039     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655040     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655041     3   0.000      0.979  0 0.00 1.00
#&gt; SRR1655042     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655043     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655044     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655045     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655046     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655047     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655048     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655049     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655050     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655051     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655052     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655053     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655054     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655055     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655056     2   0.000      1.000  0 1.00 0.00
#&gt; SRR1655057     2   0.000      1.000  0 1.00 0.00
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; SRR1655002     4  0.0000      0.989 0.00 0.00 0.00 1.00
#&gt; SRR1655003     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655004     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655005     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655006     4  0.0707      0.966 0.02 0.00 0.00 0.98
#&gt; SRR1655007     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655008     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655009     1  0.4855      1.000 0.60 0.00 0.00 0.40
#&gt; SRR1655010     4  0.0000      0.989 0.00 0.00 0.00 1.00
#&gt; SRR1655011     4  0.0000      0.989 0.00 0.00 0.00 1.00
#&gt; SRR1655012     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655013     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655014     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655015     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655016     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655017     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655018     3  0.0000      0.685 0.00 0.00 1.00 0.00
#&gt; SRR1655019     3  0.0000      0.685 0.00 0.00 1.00 0.00
#&gt; SRR1655020     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655021     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655022     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655023     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655024     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655025     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655026     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655027     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655028     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655029     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655030     3  0.2011      0.672 0.00 0.08 0.92 0.00
#&gt; SRR1655031     3  0.2011      0.672 0.00 0.08 0.92 0.00
#&gt; SRR1655032     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655033     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655034     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655035     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655036     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655037     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655038     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655039     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655040     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655041     3  0.4855      0.867 0.40 0.00 0.60 0.00
#&gt; SRR1655042     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655043     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655044     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655045     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655046     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655047     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655048     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655049     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655050     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655051     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655052     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655053     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655054     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655055     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655056     2  0.0000      1.000 0.00 1.00 0.00 0.00
#&gt; SRR1655057     2  0.0000      1.000 0.00 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1655002     5  0.0000      0.992 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1655003     1  0.0000      0.959 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      0.959 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      0.959 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655006     5  0.0609      0.977 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1655007     1  0.3074      0.754 0.804 0.000 0.000 0.000 0.196
#&gt; SRR1655008     1  0.0000      0.959 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      0.959 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655010     5  0.0000      0.992 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1655011     5  0.0000      0.992 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1655012     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655013     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655014     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655015     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655016     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655017     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655018     2  0.0794      0.881 0.000 0.972 0.028 0.000 0.000
#&gt; SRR1655019     2  0.0794      0.881 0.000 0.972 0.028 0.000 0.000
#&gt; SRR1655020     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655021     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655022     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655023     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655024     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655025     4  0.0794      0.981 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1655026     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655027     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655028     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655029     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655030     2  0.2511      0.883 0.000 0.892 0.028 0.080 0.000
#&gt; SRR1655031     2  0.2511      0.883 0.000 0.892 0.028 0.080 0.000
#&gt; SRR1655032     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655033     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655034     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1655042     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655043     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655044     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655045     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655046     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655047     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655048     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655049     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655050     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655051     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655052     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655053     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655054     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655055     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655056     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1655057     4  0.0000      0.990 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2 p3   p4    p5   p6
#&gt; SRR1655002     5  0.0000      0.992 0.000  0  0 0.00 1.000 0.00
#&gt; SRR1655003     1  0.0000      0.959 1.000  0  0 0.00 0.000 0.00
#&gt; SRR1655004     1  0.0000      0.959 1.000  0  0 0.00 0.000 0.00
#&gt; SRR1655005     1  0.0000      0.959 1.000  0  0 0.00 0.000 0.00
#&gt; SRR1655006     5  0.0547      0.977 0.020  0  0 0.00 0.980 0.00
#&gt; SRR1655007     1  0.2762      0.754 0.804  0  0 0.00 0.196 0.00
#&gt; SRR1655008     1  0.0000      0.959 1.000  0  0 0.00 0.000 0.00
#&gt; SRR1655009     1  0.0000      0.959 1.000  0  0 0.00 0.000 0.00
#&gt; SRR1655010     5  0.0000      0.992 0.000  0  0 0.00 1.000 0.00
#&gt; SRR1655011     5  0.0000      0.992 0.000  0  0 0.00 1.000 0.00
#&gt; SRR1655012     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655013     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655014     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655015     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655016     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655017     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655018     4  0.0000      0.902 0.000  0  0 1.00 0.000 0.00
#&gt; SRR1655019     4  0.0000      0.902 0.000  0  0 1.00 0.000 0.00
#&gt; SRR1655020     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655021     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655022     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655023     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655024     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655025     2  0.0000      1.000 0.000  1  0 0.00 0.000 0.00
#&gt; SRR1655026     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655027     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655028     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655029     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655030     4  0.1556      0.903 0.000  0  0 0.92 0.000 0.08
#&gt; SRR1655031     4  0.1556      0.903 0.000  0  0 0.92 0.000 0.08
#&gt; SRR1655032     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655033     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655034     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655035     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655036     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655037     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655038     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655039     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655040     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655041     3  0.0000      1.000 0.000  0  1 0.00 0.000 0.00
#&gt; SRR1655042     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655043     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655044     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655045     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655046     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655047     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655048     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655049     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655050     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655051     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655052     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655053     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655054     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655055     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655056     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
#&gt; SRR1655057     6  0.0000      1.000 0.000  0  0 0.00 0.000 1.00
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.553           0.888       0.911         0.8041 0.766   0.667
#> 4 4 0.577           0.558       0.701         0.2693 0.831   0.639
#> 5 5 0.645           0.742       0.763         0.1077 0.808   0.435
#> 6 6 0.681           0.725       0.754         0.0674 0.930   0.693
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1  0.2625      0.961 0.916 0.000 0.084
#&gt; SRR1655003     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655006     1  0.2625      0.961 0.916 0.000 0.084
#&gt; SRR1655007     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      0.974 1.000 0.000 0.000
#&gt; SRR1655010     1  0.2625      0.961 0.916 0.000 0.084
#&gt; SRR1655011     1  0.2625      0.961 0.916 0.000 0.084
#&gt; SRR1655012     2  0.2796      0.888 0.000 0.908 0.092
#&gt; SRR1655013     2  0.2796      0.888 0.000 0.908 0.092
#&gt; SRR1655014     2  0.4452      0.794 0.000 0.808 0.192
#&gt; SRR1655015     2  0.4452      0.794 0.000 0.808 0.192
#&gt; SRR1655016     2  0.2796      0.888 0.000 0.908 0.092
#&gt; SRR1655017     2  0.2796      0.888 0.000 0.908 0.092
#&gt; SRR1655018     3  0.4235      0.919 0.000 0.176 0.824
#&gt; SRR1655019     3  0.4235      0.919 0.000 0.176 0.824
#&gt; SRR1655020     2  0.5968      0.478 0.000 0.636 0.364
#&gt; SRR1655021     2  0.5968      0.478 0.000 0.636 0.364
#&gt; SRR1655022     2  0.2711      0.889 0.000 0.912 0.088
#&gt; SRR1655023     2  0.2711      0.889 0.000 0.912 0.088
#&gt; SRR1655024     2  0.2711      0.889 0.000 0.912 0.088
#&gt; SRR1655025     2  0.2711      0.889 0.000 0.912 0.088
#&gt; SRR1655026     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655027     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655028     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655029     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655030     2  0.5760      0.531 0.000 0.672 0.328
#&gt; SRR1655031     2  0.5760      0.531 0.000 0.672 0.328
#&gt; SRR1655032     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655033     2  0.0592      0.896 0.000 0.988 0.012
#&gt; SRR1655034     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655035     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655036     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655037     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655038     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655039     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655040     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655041     3  0.3267      0.980 0.000 0.116 0.884
#&gt; SRR1655042     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655043     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655044     2  0.0892      0.893 0.000 0.980 0.020
#&gt; SRR1655045     2  0.0892      0.893 0.000 0.980 0.020
#&gt; SRR1655046     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655047     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655048     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655049     2  0.2066      0.897 0.000 0.940 0.060
#&gt; SRR1655050     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655051     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655052     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655053     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655054     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655055     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655056     2  0.1753      0.891 0.000 0.952 0.048
#&gt; SRR1655057     2  0.1753      0.891 0.000 0.952 0.048
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1655002     1  0.3219     0.9333 0.868 0.000 0.020 0.112
#&gt; SRR1655003     1  0.0000     0.9540 1.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000     0.9540 1.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000     0.9540 1.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.3166     0.9331 0.868 0.000 0.016 0.116
#&gt; SRR1655007     1  0.0672     0.9540 0.984 0.000 0.008 0.008
#&gt; SRR1655008     1  0.0672     0.9540 0.984 0.000 0.008 0.008
#&gt; SRR1655009     1  0.0672     0.9540 0.984 0.000 0.008 0.008
#&gt; SRR1655010     1  0.3217     0.9332 0.860 0.000 0.012 0.128
#&gt; SRR1655011     1  0.3217     0.9332 0.860 0.000 0.012 0.128
#&gt; SRR1655012     2  0.1661     0.4815 0.000 0.944 0.052 0.004
#&gt; SRR1655013     2  0.1661     0.4815 0.000 0.944 0.052 0.004
#&gt; SRR1655014     2  0.4424     0.4100 0.000 0.812 0.088 0.100
#&gt; SRR1655015     2  0.4424     0.4100 0.000 0.812 0.088 0.100
#&gt; SRR1655016     2  0.1661     0.4815 0.000 0.944 0.052 0.004
#&gt; SRR1655017     2  0.1661     0.4815 0.000 0.944 0.052 0.004
#&gt; SRR1655018     3  0.5678     0.7558 0.000 0.172 0.716 0.112
#&gt; SRR1655019     3  0.5678     0.7558 0.000 0.172 0.716 0.112
#&gt; SRR1655020     2  0.5811     0.3344 0.000 0.704 0.180 0.116
#&gt; SRR1655021     2  0.5811     0.3344 0.000 0.704 0.180 0.116
#&gt; SRR1655022     2  0.1743     0.4819 0.000 0.940 0.056 0.004
#&gt; SRR1655023     2  0.1743     0.4819 0.000 0.940 0.056 0.004
#&gt; SRR1655024     2  0.2363     0.4761 0.000 0.920 0.056 0.024
#&gt; SRR1655025     2  0.2363     0.4761 0.000 0.920 0.056 0.024
#&gt; SRR1655026     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655027     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655028     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655029     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655030     4  0.7700    -0.0563 0.000 0.384 0.220 0.396
#&gt; SRR1655031     4  0.7700    -0.0563 0.000 0.384 0.220 0.396
#&gt; SRR1655032     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655033     2  0.5000    -0.2130 0.000 0.504 0.000 0.496
#&gt; SRR1655034     3  0.1629     0.9369 0.000 0.024 0.952 0.024
#&gt; SRR1655035     3  0.1629     0.9369 0.000 0.024 0.952 0.024
#&gt; SRR1655036     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655037     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655038     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655039     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655040     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655041     3  0.1151     0.9410 0.000 0.024 0.968 0.008
#&gt; SRR1655042     2  0.4866     0.3271 0.000 0.596 0.000 0.404
#&gt; SRR1655043     2  0.4866     0.3271 0.000 0.596 0.000 0.404
#&gt; SRR1655044     2  0.4817     0.3043 0.000 0.612 0.000 0.388
#&gt; SRR1655045     2  0.4817     0.3043 0.000 0.612 0.000 0.388
#&gt; SRR1655046     2  0.4866     0.3271 0.000 0.596 0.000 0.404
#&gt; SRR1655047     2  0.4866     0.3271 0.000 0.596 0.000 0.404
#&gt; SRR1655048     2  0.4817     0.3216 0.000 0.612 0.000 0.388
#&gt; SRR1655049     2  0.4817     0.3216 0.000 0.612 0.000 0.388
#&gt; SRR1655050     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655051     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655052     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655053     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655054     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655055     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655056     4  0.5306     0.7795 0.000 0.348 0.020 0.632
#&gt; SRR1655057     4  0.5306     0.7795 0.000 0.348 0.020 0.632
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.4140      0.897 0.792 0.028 0.016 0.160 0.004
#&gt; SRR1655003     1  0.1041      0.926 0.964 0.032 0.000 0.004 0.000
#&gt; SRR1655004     1  0.1124      0.926 0.960 0.036 0.000 0.004 0.000
#&gt; SRR1655005     1  0.1124      0.926 0.960 0.036 0.000 0.004 0.000
#&gt; SRR1655006     1  0.4044      0.897 0.792 0.020 0.016 0.168 0.004
#&gt; SRR1655007     1  0.0162      0.926 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1655008     1  0.0000      0.926 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      0.926 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.3668      0.897 0.796 0.012 0.004 0.184 0.004
#&gt; SRR1655011     1  0.3668      0.897 0.796 0.012 0.004 0.184 0.004
#&gt; SRR1655012     2  0.3551      0.853 0.000 0.820 0.000 0.044 0.136
#&gt; SRR1655013     2  0.3551      0.853 0.000 0.820 0.000 0.044 0.136
#&gt; SRR1655014     2  0.2341      0.791 0.000 0.912 0.020 0.012 0.056
#&gt; SRR1655015     2  0.2341      0.791 0.000 0.912 0.020 0.012 0.056
#&gt; SRR1655016     2  0.3551      0.853 0.000 0.820 0.000 0.044 0.136
#&gt; SRR1655017     2  0.3551      0.853 0.000 0.820 0.000 0.044 0.136
#&gt; SRR1655018     3  0.6329      0.414 0.000 0.372 0.492 0.128 0.008
#&gt; SRR1655019     3  0.6329      0.414 0.000 0.372 0.492 0.128 0.008
#&gt; SRR1655020     2  0.4226      0.702 0.000 0.808 0.068 0.096 0.028
#&gt; SRR1655021     2  0.4226      0.702 0.000 0.808 0.068 0.096 0.028
#&gt; SRR1655022     2  0.4343      0.833 0.000 0.768 0.000 0.096 0.136
#&gt; SRR1655023     2  0.4343      0.833 0.000 0.768 0.000 0.096 0.136
#&gt; SRR1655024     2  0.4437      0.825 0.000 0.760 0.000 0.100 0.140
#&gt; SRR1655025     2  0.4437      0.825 0.000 0.760 0.000 0.100 0.140
#&gt; SRR1655026     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655027     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655028     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655029     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655030     4  0.8409      0.185 0.000 0.232 0.168 0.348 0.252
#&gt; SRR1655031     4  0.8409      0.185 0.000 0.232 0.168 0.348 0.252
#&gt; SRR1655032     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655033     5  0.6339      0.418 0.000 0.128 0.020 0.284 0.568
#&gt; SRR1655034     3  0.2040      0.890 0.000 0.032 0.928 0.032 0.008
#&gt; SRR1655035     3  0.2040      0.890 0.000 0.032 0.928 0.032 0.008
#&gt; SRR1655036     3  0.1168      0.894 0.000 0.032 0.960 0.000 0.008
#&gt; SRR1655037     3  0.1168      0.894 0.000 0.032 0.960 0.000 0.008
#&gt; SRR1655038     3  0.1673      0.893 0.000 0.032 0.944 0.016 0.008
#&gt; SRR1655039     3  0.1673      0.893 0.000 0.032 0.944 0.016 0.008
#&gt; SRR1655040     3  0.1168      0.894 0.000 0.032 0.960 0.000 0.008
#&gt; SRR1655041     3  0.1168      0.894 0.000 0.032 0.960 0.000 0.008
#&gt; SRR1655042     4  0.6128      0.798 0.000 0.188 0.000 0.560 0.252
#&gt; SRR1655043     4  0.6128      0.798 0.000 0.188 0.000 0.560 0.252
#&gt; SRR1655044     4  0.6296      0.776 0.000 0.180 0.004 0.548 0.268
#&gt; SRR1655045     4  0.6296      0.776 0.000 0.180 0.004 0.548 0.268
#&gt; SRR1655046     4  0.6128      0.798 0.000 0.188 0.000 0.560 0.252
#&gt; SRR1655047     4  0.6128      0.798 0.000 0.188 0.000 0.560 0.252
#&gt; SRR1655048     4  0.6087      0.784 0.000 0.188 0.000 0.568 0.244
#&gt; SRR1655049     4  0.6087      0.784 0.000 0.188 0.000 0.568 0.244
#&gt; SRR1655050     5  0.1329      0.691 0.000 0.008 0.004 0.032 0.956
#&gt; SRR1655051     5  0.1329      0.691 0.000 0.008 0.004 0.032 0.956
#&gt; SRR1655052     5  0.0613      0.696 0.000 0.008 0.004 0.004 0.984
#&gt; SRR1655053     5  0.0613      0.696 0.000 0.008 0.004 0.004 0.984
#&gt; SRR1655054     5  0.0740      0.696 0.000 0.008 0.004 0.008 0.980
#&gt; SRR1655055     5  0.0740      0.696 0.000 0.008 0.004 0.008 0.980
#&gt; SRR1655056     5  0.0451      0.696 0.000 0.008 0.004 0.000 0.988
#&gt; SRR1655057     5  0.0451      0.696 0.000 0.008 0.004 0.000 0.988
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1655002     1  0.3330      0.851 0.716 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655003     1  0.1367      0.893 0.944 0.000 0.000 0.044 0.000 NA
#&gt; SRR1655004     1  0.1367      0.893 0.944 0.000 0.000 0.044 0.000 NA
#&gt; SRR1655005     1  0.1367      0.893 0.944 0.000 0.000 0.044 0.000 NA
#&gt; SRR1655006     1  0.3330      0.851 0.716 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655007     1  0.0146      0.893 0.996 0.000 0.000 0.004 0.000 NA
#&gt; SRR1655008     1  0.0000      0.893 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655009     1  0.0000      0.893 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655010     1  0.3656      0.851 0.728 0.004 0.000 0.012 0.000 NA
#&gt; SRR1655011     1  0.3656      0.851 0.728 0.004 0.000 0.012 0.000 NA
#&gt; SRR1655012     2  0.2177      0.870 0.000 0.908 0.000 0.032 0.052 NA
#&gt; SRR1655013     2  0.2177      0.870 0.000 0.908 0.000 0.032 0.052 NA
#&gt; SRR1655014     2  0.0924      0.847 0.000 0.972 0.008 0.008 0.004 NA
#&gt; SRR1655015     2  0.0924      0.847 0.000 0.972 0.008 0.008 0.004 NA
#&gt; SRR1655016     2  0.2177      0.870 0.000 0.908 0.000 0.032 0.052 NA
#&gt; SRR1655017     2  0.2177      0.870 0.000 0.908 0.000 0.032 0.052 NA
#&gt; SRR1655018     3  0.7412      0.277 0.000 0.276 0.364 0.104 0.004 NA
#&gt; SRR1655019     3  0.7412      0.277 0.000 0.276 0.364 0.104 0.004 NA
#&gt; SRR1655020     2  0.4375      0.752 0.000 0.752 0.024 0.056 0.004 NA
#&gt; SRR1655021     2  0.4375      0.752 0.000 0.752 0.024 0.056 0.004 NA
#&gt; SRR1655022     2  0.4359      0.851 0.000 0.772 0.000 0.092 0.052 NA
#&gt; SRR1655023     2  0.4359      0.851 0.000 0.772 0.000 0.092 0.052 NA
#&gt; SRR1655024     2  0.4416      0.842 0.000 0.768 0.000 0.096 0.056 NA
#&gt; SRR1655025     2  0.4416      0.842 0.000 0.768 0.000 0.096 0.056 NA
#&gt; SRR1655026     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655027     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655028     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655029     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655030     4  0.7990      0.269 0.000 0.128 0.104 0.460 0.148 NA
#&gt; SRR1655031     4  0.7990      0.269 0.000 0.128 0.104 0.460 0.148 NA
#&gt; SRR1655032     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655033     4  0.5576      0.255 0.000 0.040 0.000 0.468 0.440 NA
#&gt; SRR1655034     3  0.2706      0.829 0.000 0.008 0.876 0.028 0.004 NA
#&gt; SRR1655035     3  0.2706      0.829 0.000 0.008 0.876 0.028 0.004 NA
#&gt; SRR1655036     3  0.0146      0.853 0.000 0.004 0.996 0.000 0.000 NA
#&gt; SRR1655037     3  0.0146      0.853 0.000 0.004 0.996 0.000 0.000 NA
#&gt; SRR1655038     3  0.1194      0.850 0.000 0.004 0.956 0.008 0.000 NA
#&gt; SRR1655039     3  0.1194      0.850 0.000 0.004 0.956 0.008 0.000 NA
#&gt; SRR1655040     3  0.0146      0.853 0.000 0.004 0.996 0.000 0.000 NA
#&gt; SRR1655041     3  0.0146      0.853 0.000 0.004 0.996 0.000 0.000 NA
#&gt; SRR1655042     4  0.6357      0.579 0.000 0.104 0.000 0.572 0.132 NA
#&gt; SRR1655043     4  0.6357      0.579 0.000 0.104 0.000 0.572 0.132 NA
#&gt; SRR1655044     4  0.6284      0.576 0.000 0.092 0.000 0.580 0.144 NA
#&gt; SRR1655045     4  0.6284      0.576 0.000 0.092 0.000 0.580 0.144 NA
#&gt; SRR1655046     4  0.6333      0.579 0.000 0.104 0.000 0.576 0.132 NA
#&gt; SRR1655047     4  0.6333      0.579 0.000 0.104 0.000 0.576 0.132 NA
#&gt; SRR1655048     4  0.6518      0.559 0.000 0.108 0.000 0.544 0.128 NA
#&gt; SRR1655049     4  0.6518      0.559 0.000 0.108 0.000 0.544 0.128 NA
#&gt; SRR1655050     5  0.0858      0.967 0.000 0.004 0.000 0.000 0.968 NA
#&gt; SRR1655051     5  0.0777      0.968 0.000 0.004 0.000 0.000 0.972 NA
#&gt; SRR1655052     5  0.0692      0.978 0.000 0.004 0.000 0.000 0.976 NA
#&gt; SRR1655053     5  0.0692      0.978 0.000 0.004 0.000 0.000 0.976 NA
#&gt; SRR1655054     5  0.0692      0.974 0.000 0.004 0.000 0.000 0.976 NA
#&gt; SRR1655055     5  0.0692      0.974 0.000 0.004 0.000 0.000 0.976 NA
#&gt; SRR1655056     5  0.0858      0.978 0.000 0.004 0.000 0.000 0.968 NA
#&gt; SRR1655057     5  0.0858      0.978 0.000 0.004 0.000 0.000 0.968 NA
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000          0.299 0.701   0.701
#> 3 3 1.000           0.984       0.993          0.683 0.803   0.719
#> 4 4 0.799           0.956       0.924          0.307 0.782   0.567
#> 5 5 0.821           0.885       0.885          0.148 0.984   0.945
#> 6 6 0.911           0.816       0.751          0.072 0.849   0.477
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655003     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655004     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655005     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655006     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655007     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655008     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655009     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655010     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655011     1    0.00      1.000  1 0.000 0.000
#&gt; SRR1655012     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655013     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655014     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655015     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655016     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655017     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655018     2    0.46      0.751  0 0.796 0.204
#&gt; SRR1655019     2    0.46      0.751  0 0.796 0.204
#&gt; SRR1655020     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655021     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655022     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655023     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655024     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655025     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655026     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655027     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655028     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655029     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655030     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655031     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655032     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655033     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655034     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655035     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655036     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655037     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655038     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655039     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655040     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655041     3    0.00      1.000  0 0.000 1.000
#&gt; SRR1655042     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655043     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655044     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655045     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655046     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655047     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655048     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655049     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655050     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655051     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655052     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655053     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655054     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655055     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655056     2    0.00      0.989  0 1.000 0.000
#&gt; SRR1655057     2    0.00      0.989  0 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655013     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655014     2  0.4624      0.908  0 0.660  0 0.340
#&gt; SRR1655015     2  0.4624      0.908  0 0.660  0 0.340
#&gt; SRR1655016     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655017     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655018     2  0.0188      0.520  0 0.996  0 0.004
#&gt; SRR1655019     2  0.0188      0.520  0 0.996  0 0.004
#&gt; SRR1655020     2  0.4477      0.890  0 0.688  0 0.312
#&gt; SRR1655021     2  0.4477      0.890  0 0.688  0 0.312
#&gt; SRR1655022     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655023     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655024     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655025     2  0.4661      0.910  0 0.652  0 0.348
#&gt; SRR1655026     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655027     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655028     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655029     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655030     4  0.1302      0.938  0 0.044  0 0.956
#&gt; SRR1655031     4  0.1302      0.938  0 0.044  0 0.956
#&gt; SRR1655032     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655033     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655043     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655044     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655045     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655046     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655047     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655048     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655049     4  0.0469      0.985  0 0.012  0 0.988
#&gt; SRR1655050     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655051     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655052     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655053     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655054     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655055     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655056     4  0.0000      0.989  0 0.000  0 1.000
#&gt; SRR1655057     4  0.0000      0.989  0 0.000  0 1.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655018     5  0.4350      1.000  0 0.408 0.004 0.000 0.588
#&gt; SRR1655019     5  0.4350      1.000  0 0.408 0.004 0.000 0.588
#&gt; SRR1655020     2  0.0162      0.990  0 0.996 0.000 0.004 0.000
#&gt; SRR1655021     2  0.0162      0.990  0 0.996 0.000 0.004 0.000
#&gt; SRR1655022     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655023     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655024     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655025     2  0.0000      0.998  0 1.000 0.000 0.000 0.000
#&gt; SRR1655026     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655027     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655028     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655029     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655030     4  0.4924      0.783  0 0.060 0.000 0.668 0.272
#&gt; SRR1655031     4  0.4924      0.783  0 0.060 0.000 0.668 0.272
#&gt; SRR1655032     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655033     4  0.5010      0.789  0 0.076 0.000 0.676 0.248
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1655042     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655043     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655044     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655045     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655046     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655047     4  0.2020      0.681  0 0.100 0.000 0.900 0.000
#&gt; SRR1655048     4  0.2074      0.680  0 0.104 0.000 0.896 0.000
#&gt; SRR1655049     4  0.2074      0.680  0 0.104 0.000 0.896 0.000
#&gt; SRR1655050     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655051     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655052     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655053     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655054     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655055     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655056     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
#&gt; SRR1655057     4  0.5036      0.735  0 0.036 0.000 0.560 0.404
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655013     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655014     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655016     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655017     2  0.0000      0.988  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1655018     3  0.5481      0.192  0 0.072 0.488 0.420 0.000 0.020
#&gt; SRR1655019     3  0.5481      0.192  0 0.072 0.488 0.420 0.000 0.020
#&gt; SRR1655020     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655021     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655022     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655023     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655024     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655025     2  0.0547      0.988  0 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1655026     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655027     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655028     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655029     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655030     4  0.4400      0.972  0 0.032 0.000 0.592 0.000 0.376
#&gt; SRR1655031     4  0.4400      0.972  0 0.032 0.000 0.592 0.000 0.376
#&gt; SRR1655032     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655033     4  0.4593      0.991  0 0.044 0.000 0.576 0.000 0.380
#&gt; SRR1655034     5  0.3995     -0.772  0 0.000 0.480 0.000 0.516 0.004
#&gt; SRR1655035     5  0.3995     -0.772  0 0.000 0.480 0.000 0.516 0.004
#&gt; SRR1655036     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655037     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655038     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655039     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655040     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655041     3  0.3867      0.734  0 0.000 0.512 0.000 0.488 0.000
#&gt; SRR1655042     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655043     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655044     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655045     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655046     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655047     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655048     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655049     6  0.0632      1.000  0 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1655050     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655051     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655052     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655053     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655054     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655055     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655056     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
#&gt; SRR1655057     5  0.5080      0.591  0 0.008 0.000 0.452 0.484 0.056
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 1.000           0.975       0.984         0.8099 0.766   0.667
#> 4 4 0.997           0.952       0.979         0.3470 0.800   0.572
#> 5 5 0.989           0.945       0.973         0.0993 0.927   0.728
#> 6 6 1.000           0.963       0.985         0.0496 0.945   0.731
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000
#&gt; SRR1655012     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655013     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655014     2  0.1411      0.965  0 0.964 0.036
#&gt; SRR1655015     2  0.2959      0.905  0 0.900 0.100
#&gt; SRR1655016     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655017     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655018     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655019     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655020     2  0.4796      0.754  0 0.780 0.220
#&gt; SRR1655021     2  0.4796      0.754  0 0.780 0.220
#&gt; SRR1655022     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655023     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655024     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655025     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655026     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655027     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655028     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655029     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655030     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655031     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655032     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655033     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000 1.000
#&gt; SRR1655042     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655043     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655044     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655045     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655046     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655047     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655048     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655049     2  0.0892      0.976  0 0.980 0.020
#&gt; SRR1655050     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655051     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655052     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655053     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655054     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655055     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655056     2  0.0000      0.975  0 1.000 0.000
#&gt; SRR1655057     2  0.0000      0.975  0 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655013     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655014     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655015     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655016     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655017     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655018     3  0.4898      0.364  0 0.416 0.584 0.000
#&gt; SRR1655019     3  0.4898      0.364  0 0.416 0.584 0.000
#&gt; SRR1655020     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655021     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655022     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655023     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655024     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655025     2  0.0336      0.995  0 0.992 0.000 0.008
#&gt; SRR1655026     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655027     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655028     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655029     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655030     4  0.0592      0.979  0 0.016 0.000 0.984
#&gt; SRR1655031     4  0.0592      0.979  0 0.016 0.000 0.984
#&gt; SRR1655032     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655033     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655034     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655035     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655036     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655037     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655038     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655039     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655040     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655041     3  0.0000      0.895  0 0.000 1.000 0.000
#&gt; SRR1655042     4  0.1792      0.938  0 0.068 0.000 0.932
#&gt; SRR1655043     4  0.1716      0.942  0 0.064 0.000 0.936
#&gt; SRR1655044     4  0.0336      0.982  0 0.008 0.000 0.992
#&gt; SRR1655045     4  0.0336      0.982  0 0.008 0.000 0.992
#&gt; SRR1655046     4  0.0817      0.976  0 0.024 0.000 0.976
#&gt; SRR1655047     4  0.0817      0.976  0 0.024 0.000 0.976
#&gt; SRR1655048     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655049     2  0.0000      0.993  0 1.000 0.000 0.000
#&gt; SRR1655050     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655051     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655052     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655053     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655054     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655055     4  0.0000      0.985  0 0.000 0.000 1.000
#&gt; SRR1655056     4  0.1211      0.958  0 0.040 0.000 0.960
#&gt; SRR1655057     4  0.1118      0.962  0 0.036 0.000 0.964
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2  p3    p4    p5
#&gt; SRR1655002     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655003     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655004     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655005     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655006     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655007     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655008     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655009     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655010     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655011     1   0.000      1.000  1 0.000 0.0 0.000 0.000
#&gt; SRR1655012     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655013     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655014     2   0.029      0.983  0 0.992 0.0 0.008 0.000
#&gt; SRR1655015     2   0.029      0.983  0 0.992 0.0 0.008 0.000
#&gt; SRR1655016     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655017     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655018     3   0.418      0.396  0 0.400 0.6 0.000 0.000
#&gt; SRR1655019     3   0.418      0.396  0 0.400 0.6 0.000 0.000
#&gt; SRR1655020     2   0.029      0.983  0 0.992 0.0 0.008 0.000
#&gt; SRR1655021     2   0.029      0.983  0 0.992 0.0 0.008 0.000
#&gt; SRR1655022     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655023     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655024     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655025     2   0.000      0.986  0 1.000 0.0 0.000 0.000
#&gt; SRR1655026     4   0.156      0.953  0 0.008 0.0 0.940 0.052
#&gt; SRR1655027     4   0.176      0.950  0 0.008 0.0 0.928 0.064
#&gt; SRR1655028     4   0.176      0.950  0 0.008 0.0 0.928 0.064
#&gt; SRR1655029     4   0.176      0.950  0 0.008 0.0 0.928 0.064
#&gt; SRR1655030     4   0.029      0.958  0 0.008 0.0 0.992 0.000
#&gt; SRR1655031     4   0.029      0.958  0 0.008 0.0 0.992 0.000
#&gt; SRR1655032     4   0.176      0.950  0 0.008 0.0 0.928 0.064
#&gt; SRR1655033     4   0.176      0.950  0 0.008 0.0 0.928 0.064
#&gt; SRR1655034     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655035     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655036     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655037     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655038     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655039     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655040     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655041     3   0.000      0.894  0 0.000 1.0 0.000 0.000
#&gt; SRR1655042     4   0.112      0.930  0 0.044 0.0 0.956 0.000
#&gt; SRR1655043     4   0.104      0.933  0 0.040 0.0 0.960 0.000
#&gt; SRR1655044     4   0.000      0.958  0 0.000 0.0 1.000 0.000
#&gt; SRR1655045     4   0.000      0.958  0 0.000 0.0 1.000 0.000
#&gt; SRR1655046     4   0.000      0.958  0 0.000 0.0 1.000 0.000
#&gt; SRR1655047     4   0.000      0.958  0 0.000 0.0 1.000 0.000
#&gt; SRR1655048     2   0.161      0.931  0 0.928 0.0 0.072 0.000
#&gt; SRR1655049     2   0.161      0.931  0 0.928 0.0 0.072 0.000
#&gt; SRR1655050     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655051     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655052     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655053     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655054     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655055     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655056     5   0.000      1.000  0 0.000 0.0 0.000 1.000
#&gt; SRR1655057     5   0.000      1.000  0 0.000 0.0 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1  p2  p3    p4 p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.0 0.0 0.000  0 0.000
#&gt; SRR1655012     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655013     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655014     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655015     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655016     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655017     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655018     3  0.3756      0.407  0 0.4 0.6 0.000  0 0.000
#&gt; SRR1655019     3  0.3756      0.407  0 0.4 0.6 0.000  0 0.000
#&gt; SRR1655020     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655021     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655022     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655023     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655024     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655025     2  0.0000      1.000  0 1.0 0.0 0.000  0 0.000
#&gt; SRR1655026     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655027     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655028     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655029     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655030     4  0.0363      0.989  0 0.0 0.0 0.988  0 0.012
#&gt; SRR1655031     4  0.0458      0.986  0 0.0 0.0 0.984  0 0.016
#&gt; SRR1655032     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655033     4  0.0000      0.996  0 0.0 0.0 1.000  0 0.000
#&gt; SRR1655034     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655035     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655036     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655037     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655038     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655039     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655040     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655041     3  0.0000      0.898  0 0.0 1.0 0.000  0 0.000
#&gt; SRR1655042     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655043     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655044     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655045     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655046     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655047     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655048     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655049     6  0.0000      1.000  0 0.0 0.0 0.000  0 1.000
#&gt; SRR1655050     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.0 0.0 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.996       0.997         0.3020 0.701   0.701
#> 3 3 1.000           0.984       0.984         0.6748 0.803   0.719
#> 4 4 0.677           0.580       0.788         0.3632 0.797   0.598
#> 5 5 0.748           0.875       0.872         0.1381 0.849   0.540
#> 6 6 0.872           0.869       0.898         0.0638 0.958   0.795
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1655002     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655003     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655004     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655005     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655006     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655007     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655008     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655009     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655010     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655011     1  0.0000      1.000 1.000 0.000
#&gt; SRR1655012     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655013     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655014     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655015     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655016     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655017     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655018     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655019     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655020     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655021     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655022     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655023     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655024     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655025     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655026     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655027     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655028     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655029     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655030     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655031     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655032     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655033     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655034     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655035     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655036     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655037     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655038     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655039     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655040     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655041     2  0.0672      0.993 0.008 0.992
#&gt; SRR1655042     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655043     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655044     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655045     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655046     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655047     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655048     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655049     2  0.0000      0.997 0.000 1.000
#&gt; SRR1655050     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655051     2  0.0672      0.992 0.008 0.992
#&gt; SRR1655052     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655053     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655054     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655055     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655056     2  0.0938      0.990 0.012 0.988
#&gt; SRR1655057     2  0.0938      0.990 0.012 0.988
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1655002     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1655012     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655013     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655014     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655015     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655016     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655017     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655018     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655019     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655020     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655021     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655022     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655023     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655024     2  0.0592      0.980 0.000 0.988 0.012
#&gt; SRR1655025     2  0.0424      0.982 0.000 0.992 0.008
#&gt; SRR1655026     2  0.0892      0.977 0.000 0.980 0.020
#&gt; SRR1655027     2  0.0747      0.979 0.000 0.984 0.016
#&gt; SRR1655028     2  0.1031      0.976 0.000 0.976 0.024
#&gt; SRR1655029     2  0.0892      0.977 0.000 0.980 0.020
#&gt; SRR1655030     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655031     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655032     2  0.1031      0.976 0.000 0.976 0.024
#&gt; SRR1655033     2  0.1031      0.976 0.000 0.976 0.024
#&gt; SRR1655034     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655035     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655036     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655037     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655038     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655039     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655040     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655041     3  0.1031      1.000 0.024 0.000 0.976
#&gt; SRR1655042     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655043     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655044     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655045     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655046     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655047     2  0.0000      0.982 0.000 1.000 0.000
#&gt; SRR1655048     2  0.0237      0.982 0.000 0.996 0.004
#&gt; SRR1655049     2  0.0237      0.982 0.000 0.996 0.004
#&gt; SRR1655050     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655051     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655052     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655053     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655054     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655055     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655056     2  0.2066      0.957 0.000 0.940 0.060
#&gt; SRR1655057     2  0.2066      0.957 0.000 0.940 0.060
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000
#&gt; SRR1655012     2  0.4817      0.483  0 0.612  0 0.388
#&gt; SRR1655013     2  0.4817      0.483  0 0.612  0 0.388
#&gt; SRR1655014     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655015     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655016     2  0.4817      0.483  0 0.612  0 0.388
#&gt; SRR1655017     2  0.4817      0.483  0 0.612  0 0.388
#&gt; SRR1655018     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655019     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655020     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655021     2  0.4999      0.398  0 0.508  0 0.492
#&gt; SRR1655022     4  0.4999     -0.455  0 0.492  0 0.508
#&gt; SRR1655023     4  0.5000     -0.458  0 0.496  0 0.504
#&gt; SRR1655024     4  0.4999     -0.455  0 0.492  0 0.508
#&gt; SRR1655025     4  0.4999     -0.455  0 0.492  0 0.508
#&gt; SRR1655026     2  0.2408      0.418  0 0.896  0 0.104
#&gt; SRR1655027     2  0.2408      0.418  0 0.896  0 0.104
#&gt; SRR1655028     2  0.2530      0.401  0 0.888  0 0.112
#&gt; SRR1655029     2  0.2408      0.409  0 0.896  0 0.104
#&gt; SRR1655030     2  0.0000      0.535  0 1.000  0 0.000
#&gt; SRR1655031     2  0.0000      0.535  0 1.000  0 0.000
#&gt; SRR1655032     2  0.2408      0.408  0 0.896  0 0.104
#&gt; SRR1655033     2  0.2408      0.408  0 0.896  0 0.104
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000
#&gt; SRR1655042     2  0.1211      0.543  0 0.960  0 0.040
#&gt; SRR1655043     2  0.1211      0.543  0 0.960  0 0.040
#&gt; SRR1655044     2  0.0188      0.532  0 0.996  0 0.004
#&gt; SRR1655045     2  0.0188      0.532  0 0.996  0 0.004
#&gt; SRR1655046     2  0.0000      0.535  0 1.000  0 0.000
#&gt; SRR1655047     2  0.0000      0.535  0 1.000  0 0.000
#&gt; SRR1655048     2  0.4277      0.518  0 0.720  0 0.280
#&gt; SRR1655049     2  0.4277      0.518  0 0.720  0 0.280
#&gt; SRR1655050     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655051     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655052     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655053     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655054     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655055     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655056     4  0.4999      0.525  0 0.492  0 0.508
#&gt; SRR1655057     4  0.4999      0.525  0 0.492  0 0.508
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4    p5
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000 0.000
#&gt; SRR1655012     2  0.3534      0.833  0 0.744  0 0.256 0.000
#&gt; SRR1655013     2  0.3561      0.833  0 0.740  0 0.260 0.000
#&gt; SRR1655014     2  0.2732      0.822  0 0.840  0 0.160 0.000
#&gt; SRR1655015     2  0.2732      0.822  0 0.840  0 0.160 0.000
#&gt; SRR1655016     2  0.3534      0.833  0 0.744  0 0.256 0.000
#&gt; SRR1655017     2  0.3561      0.833  0 0.740  0 0.260 0.000
#&gt; SRR1655018     2  0.0794      0.766  0 0.972  0 0.028 0.000
#&gt; SRR1655019     2  0.0794      0.766  0 0.972  0 0.028 0.000
#&gt; SRR1655020     2  0.0703      0.770  0 0.976  0 0.024 0.000
#&gt; SRR1655021     2  0.0794      0.771  0 0.972  0 0.028 0.000
#&gt; SRR1655022     2  0.3895      0.796  0 0.680  0 0.320 0.000
#&gt; SRR1655023     2  0.3913      0.796  0 0.676  0 0.324 0.000
#&gt; SRR1655024     2  0.3949      0.805  0 0.696  0 0.300 0.004
#&gt; SRR1655025     2  0.3969      0.803  0 0.692  0 0.304 0.004
#&gt; SRR1655026     4  0.2583      0.799  0 0.004  0 0.864 0.132
#&gt; SRR1655027     4  0.2536      0.800  0 0.004  0 0.868 0.128
#&gt; SRR1655028     4  0.2605      0.795  0 0.000  0 0.852 0.148
#&gt; SRR1655029     4  0.2605      0.795  0 0.000  0 0.852 0.148
#&gt; SRR1655030     4  0.3612      0.642  0 0.268  0 0.732 0.000
#&gt; SRR1655031     4  0.3612      0.642  0 0.268  0 0.732 0.000
#&gt; SRR1655032     4  0.2561      0.796  0 0.000  0 0.856 0.144
#&gt; SRR1655033     4  0.2561      0.796  0 0.000  0 0.856 0.144
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000 0.000
#&gt; SRR1655042     4  0.2280      0.743  0 0.120  0 0.880 0.000
#&gt; SRR1655043     4  0.2230      0.745  0 0.116  0 0.884 0.000
#&gt; SRR1655044     4  0.3932      0.795  0 0.064  0 0.796 0.140
#&gt; SRR1655045     4  0.3932      0.795  0 0.064  0 0.796 0.140
#&gt; SRR1655046     4  0.2127      0.755  0 0.108  0 0.892 0.000
#&gt; SRR1655047     4  0.2179      0.754  0 0.112  0 0.888 0.000
#&gt; SRR1655048     4  0.3689      0.537  0 0.256  0 0.740 0.004
#&gt; SRR1655049     4  0.3689      0.537  0 0.256  0 0.740 0.004
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000 1.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2 p3    p4 p5    p6
#&gt; SRR1655002     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655003     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655004     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655005     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655006     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655007     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655008     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655009     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655010     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655011     1  0.0000      1.000  1 0.000  0 0.000  0 0.000
#&gt; SRR1655012     2  0.3841      0.745  0 0.616  0 0.004  0 0.380
#&gt; SRR1655013     2  0.3841      0.745  0 0.616  0 0.004  0 0.380
#&gt; SRR1655014     2  0.2389      0.760  0 0.864  0 0.008  0 0.128
#&gt; SRR1655015     2  0.2389      0.760  0 0.864  0 0.008  0 0.128
#&gt; SRR1655016     2  0.3841      0.745  0 0.616  0 0.004  0 0.380
#&gt; SRR1655017     2  0.3841      0.745  0 0.616  0 0.004  0 0.380
#&gt; SRR1655018     2  0.0363      0.694  0 0.988  0 0.000  0 0.012
#&gt; SRR1655019     2  0.0363      0.694  0 0.988  0 0.000  0 0.012
#&gt; SRR1655020     2  0.1858      0.747  0 0.904  0 0.004  0 0.092
#&gt; SRR1655021     2  0.1753      0.743  0 0.912  0 0.004  0 0.084
#&gt; SRR1655022     2  0.4101      0.696  0 0.580  0 0.012  0 0.408
#&gt; SRR1655023     2  0.4184      0.696  0 0.576  0 0.016  0 0.408
#&gt; SRR1655024     2  0.3966      0.694  0 0.552  0 0.004  0 0.444
#&gt; SRR1655025     2  0.3966      0.694  0 0.552  0 0.004  0 0.444
#&gt; SRR1655026     4  0.0146      0.912  0 0.000  0 0.996  0 0.004
#&gt; SRR1655027     4  0.0146      0.912  0 0.000  0 0.996  0 0.004
#&gt; SRR1655028     4  0.0146      0.912  0 0.000  0 0.996  0 0.004
#&gt; SRR1655029     4  0.0146      0.912  0 0.000  0 0.996  0 0.004
#&gt; SRR1655030     6  0.4343      0.721  0 0.156  0 0.120  0 0.724
#&gt; SRR1655031     6  0.4343      0.721  0 0.156  0 0.120  0 0.724
#&gt; SRR1655032     4  0.0000      0.910  0 0.000  0 1.000  0 0.000
#&gt; SRR1655033     4  0.0000      0.910  0 0.000  0 1.000  0 0.000
#&gt; SRR1655034     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655035     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655036     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655037     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655038     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655039     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655040     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655041     3  0.0000      1.000  0 0.000  1 0.000  0 0.000
#&gt; SRR1655042     6  0.1950      0.786  0 0.024  0 0.064  0 0.912
#&gt; SRR1655043     6  0.2039      0.789  0 0.020  0 0.076  0 0.904
#&gt; SRR1655044     4  0.3709      0.664  0 0.040  0 0.756  0 0.204
#&gt; SRR1655045     4  0.3709      0.664  0 0.040  0 0.756  0 0.204
#&gt; SRR1655046     6  0.3898      0.553  0 0.012  0 0.336  0 0.652
#&gt; SRR1655047     6  0.3819      0.594  0 0.012  0 0.316  0 0.672
#&gt; SRR1655048     6  0.1049      0.764  0 0.008  0 0.032  0 0.960
#&gt; SRR1655049     6  0.1049      0.764  0 0.008  0 0.032  0 0.960
#&gt; SRR1655050     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655051     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655052     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655053     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655054     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655055     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655056     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
#&gt; SRR1655057     5  0.0000      1.000  0 0.000  0 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 15837 rows and 56 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2994 0.701   0.701
#> 3 3 0.734           0.884       0.930         0.6715 0.766   0.667
#> 4 4 0.597           0.457       0.765         0.3018 0.735   0.512
#> 5 5 0.565           0.690       0.763         0.0908 0.730   0.406
#> 6 6 0.697           0.826       0.819         0.0869 0.917   0.709
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1655002     1       0          1  1  0
#&gt; SRR1655003     1       0          1  1  0
#&gt; SRR1655004     1       0          1  1  0
#&gt; SRR1655005     1       0          1  1  0
#&gt; SRR1655006     1       0          1  1  0
#&gt; SRR1655007     1       0          1  1  0
#&gt; SRR1655008     1       0          1  1  0
#&gt; SRR1655009     1       0          1  1  0
#&gt; SRR1655010     1       0          1  1  0
#&gt; SRR1655011     1       0          1  1  0
#&gt; SRR1655012     2       0          1  0  1
#&gt; SRR1655013     2       0          1  0  1
#&gt; SRR1655014     2       0          1  0  1
#&gt; SRR1655015     2       0          1  0  1
#&gt; SRR1655016     2       0          1  0  1
#&gt; SRR1655017     2       0          1  0  1
#&gt; SRR1655018     2       0          1  0  1
#&gt; SRR1655019     2       0          1  0  1
#&gt; SRR1655020     2       0          1  0  1
#&gt; SRR1655021     2       0          1  0  1
#&gt; SRR1655022     2       0          1  0  1
#&gt; SRR1655023     2       0          1  0  1
#&gt; SRR1655024     2       0          1  0  1
#&gt; SRR1655025     2       0          1  0  1
#&gt; SRR1655026     2       0          1  0  1
#&gt; SRR1655027     2       0          1  0  1
#&gt; SRR1655028     2       0          1  0  1
#&gt; SRR1655029     2       0          1  0  1
#&gt; SRR1655030     2       0          1  0  1
#&gt; SRR1655031     2       0          1  0  1
#&gt; SRR1655032     2       0          1  0  1
#&gt; SRR1655033     2       0          1  0  1
#&gt; SRR1655034     2       0          1  0  1
#&gt; SRR1655035     2       0          1  0  1
#&gt; SRR1655036     2       0          1  0  1
#&gt; SRR1655037     2       0          1  0  1
#&gt; SRR1655038     2       0          1  0  1
#&gt; SRR1655039     2       0          1  0  1
#&gt; SRR1655040     2       0          1  0  1
#&gt; SRR1655041     2       0          1  0  1
#&gt; SRR1655042     2       0          1  0  1
#&gt; SRR1655043     2       0          1  0  1
#&gt; SRR1655044     2       0          1  0  1
#&gt; SRR1655045     2       0          1  0  1
#&gt; SRR1655046     2       0          1  0  1
#&gt; SRR1655047     2       0          1  0  1
#&gt; SRR1655048     2       0          1  0  1
#&gt; SRR1655049     2       0          1  0  1
#&gt; SRR1655050     2       0          1  0  1
#&gt; SRR1655051     2       0          1  0  1
#&gt; SRR1655052     2       0          1  0  1
#&gt; SRR1655053     2       0          1  0  1
#&gt; SRR1655054     2       0          1  0  1
#&gt; SRR1655055     2       0          1  0  1
#&gt; SRR1655056     2       0          1  0  1
#&gt; SRR1655057     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3
#&gt; SRR1655002     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655003     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655004     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655005     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655006     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655007     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655008     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655009     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655010     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655011     1  0.0000     1.0000  1 0.000 0.000
#&gt; SRR1655012     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655013     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655014     2  0.5016     0.5292  0 0.760 0.240
#&gt; SRR1655015     2  0.6008     0.0849  0 0.628 0.372
#&gt; SRR1655016     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655017     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655018     3  0.5706     0.8566  0 0.320 0.680
#&gt; SRR1655019     3  0.5678     0.8629  0 0.316 0.684
#&gt; SRR1655020     2  0.6252    -0.2387  0 0.556 0.444
#&gt; SRR1655021     2  0.6274    -0.2856  0 0.544 0.456
#&gt; SRR1655022     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655023     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655024     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655025     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655026     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655027     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655028     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655029     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655030     2  0.0424     0.9318  0 0.992 0.008
#&gt; SRR1655031     2  0.0237     0.9362  0 0.996 0.004
#&gt; SRR1655032     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655033     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655034     3  0.4504     0.9316  0 0.196 0.804
#&gt; SRR1655035     3  0.4504     0.9316  0 0.196 0.804
#&gt; SRR1655036     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655037     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655038     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655039     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655040     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655041     3  0.4887     0.9609  0 0.228 0.772
#&gt; SRR1655042     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655043     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655044     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655045     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655046     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655047     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655048     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655049     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655050     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655051     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655052     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655053     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655054     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655055     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655056     2  0.0000     0.9404  0 1.000 0.000
#&gt; SRR1655057     2  0.0000     0.9404  0 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1655002     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655003     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655004     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655005     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655006     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655007     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655008     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655009     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655010     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655011     1  0.0000     1.0000  1 0.000 0.000 0.000
#&gt; SRR1655012     2  0.0188     0.6621  0 0.996 0.004 0.000
#&gt; SRR1655013     2  0.0188     0.6621  0 0.996 0.004 0.000
#&gt; SRR1655014     2  0.0524     0.6593  0 0.988 0.008 0.004
#&gt; SRR1655015     2  0.0524     0.6593  0 0.988 0.008 0.004
#&gt; SRR1655016     2  0.0188     0.6609  0 0.996 0.000 0.004
#&gt; SRR1655017     2  0.0000     0.6621  0 1.000 0.000 0.000
#&gt; SRR1655018     2  0.3907     0.5201  0 0.828 0.140 0.032
#&gt; SRR1655019     2  0.4244     0.4967  0 0.804 0.160 0.036
#&gt; SRR1655020     2  0.0657     0.6575  0 0.984 0.012 0.004
#&gt; SRR1655021     2  0.0657     0.6575  0 0.984 0.012 0.004
#&gt; SRR1655022     2  0.0376     0.6611  0 0.992 0.004 0.004
#&gt; SRR1655023     2  0.0376     0.6611  0 0.992 0.004 0.004
#&gt; SRR1655024     2  0.0000     0.6621  0 1.000 0.000 0.000
#&gt; SRR1655025     2  0.0000     0.6621  0 1.000 0.000 0.000
#&gt; SRR1655026     2  0.7602    -0.6916  0 0.420 0.200 0.380
#&gt; SRR1655027     2  0.7558    -0.6647  0 0.428 0.192 0.380
#&gt; SRR1655028     2  0.7510    -0.6420  0 0.436 0.184 0.380
#&gt; SRR1655029     2  0.7534    -0.6525  0 0.432 0.188 0.380
#&gt; SRR1655030     4  0.7913     0.9733  0 0.320 0.320 0.360
#&gt; SRR1655031     4  0.7913     0.9732  0 0.320 0.320 0.360
#&gt; SRR1655032     2  0.7485    -0.6299  0 0.440 0.180 0.380
#&gt; SRR1655033     2  0.7431    -0.6065  0 0.448 0.172 0.380
#&gt; SRR1655034     3  0.2319     0.5539  0 0.040 0.924 0.036
#&gt; SRR1655035     3  0.2319     0.5539  0 0.040 0.924 0.036
#&gt; SRR1655036     3  0.1302     0.5723  0 0.044 0.956 0.000
#&gt; SRR1655037     3  0.1302     0.5723  0 0.044 0.956 0.000
#&gt; SRR1655038     3  0.1211     0.5722  0 0.040 0.960 0.000
#&gt; SRR1655039     3  0.1211     0.5722  0 0.040 0.960 0.000
#&gt; SRR1655040     3  0.1302     0.5723  0 0.044 0.956 0.000
#&gt; SRR1655041     3  0.1302     0.5723  0 0.044 0.956 0.000
#&gt; SRR1655042     2  0.5077     0.4717  0 0.760 0.080 0.160
#&gt; SRR1655043     2  0.4711     0.5080  0 0.784 0.064 0.152
#&gt; SRR1655044     2  0.5664     0.3336  0 0.696 0.076 0.228
#&gt; SRR1655045     2  0.5759     0.3105  0 0.688 0.080 0.232
#&gt; SRR1655046     2  0.5594     0.3902  0 0.716 0.092 0.192
#&gt; SRR1655047     2  0.5705     0.3636  0 0.704 0.092 0.204
#&gt; SRR1655048     2  0.3245     0.5956  0 0.872 0.028 0.100
#&gt; SRR1655049     2  0.3143     0.5987  0 0.876 0.024 0.100
#&gt; SRR1655050     3  0.6867     0.0715  0 0.104 0.484 0.412
#&gt; SRR1655051     3  0.6867     0.0715  0 0.104 0.484 0.412
#&gt; SRR1655052     3  0.6867     0.0715  0 0.104 0.484 0.412
#&gt; SRR1655053     3  0.6822     0.0804  0 0.100 0.488 0.412
#&gt; SRR1655054     3  0.6953     0.0253  0 0.112 0.476 0.412
#&gt; SRR1655055     3  0.6953     0.0253  0 0.112 0.476 0.412
#&gt; SRR1655056     3  0.6867     0.0715  0 0.104 0.484 0.412
#&gt; SRR1655057     3  0.6867     0.0715  0 0.104 0.484 0.412
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4 p5
#&gt; SRR1655002     1  0.0162      0.998 0.996 0.000 0.000 0.000 NA
#&gt; SRR1655003     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655004     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655005     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655006     1  0.0162      0.998 0.996 0.000 0.000 0.000 NA
#&gt; SRR1655007     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655008     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655009     1  0.0000      0.999 1.000 0.000 0.000 0.000 NA
#&gt; SRR1655010     1  0.0162      0.998 0.996 0.000 0.000 0.000 NA
#&gt; SRR1655011     1  0.0162      0.998 0.996 0.000 0.000 0.000 NA
#&gt; SRR1655012     2  0.0510      0.924 0.000 0.984 0.000 0.016 NA
#&gt; SRR1655013     2  0.0510      0.922 0.000 0.984 0.000 0.016 NA
#&gt; SRR1655014     2  0.0898      0.924 0.000 0.972 0.020 0.008 NA
#&gt; SRR1655015     2  0.0898      0.923 0.000 0.972 0.020 0.008 NA
#&gt; SRR1655016     2  0.0703      0.918 0.000 0.976 0.000 0.024 NA
#&gt; SRR1655017     2  0.0703      0.918 0.000 0.976 0.000 0.024 NA
#&gt; SRR1655018     2  0.3909      0.758 0.000 0.800 0.148 0.048 NA
#&gt; SRR1655019     2  0.4060      0.744 0.000 0.788 0.156 0.052 NA
#&gt; SRR1655020     2  0.1750      0.901 0.000 0.936 0.028 0.036 NA
#&gt; SRR1655021     2  0.1493      0.910 0.000 0.948 0.028 0.024 NA
#&gt; SRR1655022     2  0.0771      0.926 0.000 0.976 0.020 0.004 NA
#&gt; SRR1655023     2  0.0807      0.927 0.000 0.976 0.012 0.012 NA
#&gt; SRR1655024     2  0.0912      0.922 0.000 0.972 0.012 0.016 NA
#&gt; SRR1655025     2  0.1195      0.912 0.000 0.960 0.012 0.028 NA
#&gt; SRR1655026     4  0.5772      0.502 0.000 0.328 0.108 0.564 NA
#&gt; SRR1655027     4  0.5811      0.495 0.000 0.340 0.108 0.552 NA
#&gt; SRR1655028     4  0.5850      0.504 0.000 0.316 0.120 0.564 NA
#&gt; SRR1655029     4  0.5771      0.507 0.000 0.316 0.112 0.572 NA
#&gt; SRR1655030     4  0.6073      0.517 0.000 0.264 0.172 0.564 NA
#&gt; SRR1655031     4  0.6083      0.517 0.000 0.260 0.176 0.564 NA
#&gt; SRR1655032     4  0.5845      0.485 0.000 0.352 0.108 0.540 NA
#&gt; SRR1655033     4  0.5865      0.480 0.000 0.360 0.108 0.532 NA
#&gt; SRR1655034     3  0.2082      0.958 0.000 0.032 0.928 0.024 NA
#&gt; SRR1655035     3  0.1989      0.962 0.000 0.032 0.932 0.020 NA
#&gt; SRR1655036     3  0.1648      0.984 0.000 0.040 0.940 0.020 NA
#&gt; SRR1655037     3  0.1648      0.984 0.000 0.040 0.940 0.020 NA
#&gt; SRR1655038     3  0.1568      0.983 0.000 0.036 0.944 0.020 NA
#&gt; SRR1655039     3  0.1728      0.983 0.000 0.036 0.940 0.020 NA
#&gt; SRR1655040     3  0.1901      0.983 0.000 0.040 0.932 0.024 NA
#&gt; SRR1655041     3  0.1808      0.983 0.000 0.040 0.936 0.020 NA
#&gt; SRR1655042     4  0.6003      0.348 0.000 0.388 0.056 0.528 NA
#&gt; SRR1655043     4  0.6141      0.308 0.000 0.408 0.064 0.500 NA
#&gt; SRR1655044     4  0.5851      0.377 0.000 0.372 0.048 0.552 NA
#&gt; SRR1655045     4  0.5779      0.379 0.000 0.368 0.044 0.560 NA
#&gt; SRR1655046     4  0.5946      0.351 0.000 0.388 0.052 0.532 NA
#&gt; SRR1655047     4  0.5946      0.351 0.000 0.388 0.052 0.532 NA
#&gt; SRR1655048     4  0.5941      0.285 0.000 0.424 0.048 0.500 NA
#&gt; SRR1655049     4  0.5941      0.285 0.000 0.424 0.048 0.500 NA
#&gt; SRR1655050     4  0.6939      0.201 0.000 0.124 0.328 0.500 NA
#&gt; SRR1655051     4  0.6889      0.189 0.000 0.116 0.336 0.500 NA
#&gt; SRR1655052     4  0.6914      0.196 0.000 0.120 0.332 0.500 NA
#&gt; SRR1655053     4  0.6914      0.196 0.000 0.120 0.332 0.500 NA
#&gt; SRR1655054     4  0.6985      0.209 0.000 0.132 0.320 0.500 NA
#&gt; SRR1655055     4  0.6985      0.209 0.000 0.132 0.320 0.500 NA
#&gt; SRR1655056     4  0.6939      0.201 0.000 0.124 0.328 0.500 NA
#&gt; SRR1655057     4  0.6914      0.196 0.000 0.120 0.332 0.500 NA
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1655002     1   0.026      0.996 0.992 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655003     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655004     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655005     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655006     1   0.026      0.996 0.992 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655007     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655008     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655009     1   0.000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655010     1   0.026      0.996 0.992 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655011     1   0.026      0.996 0.992 0.000 0.000 0.000 0.000 NA
#&gt; SRR1655012     2   0.146      0.893 0.000 0.944 0.004 0.036 0.016 NA
#&gt; SRR1655013     2   0.139      0.895 0.000 0.948 0.004 0.032 0.016 NA
#&gt; SRR1655014     2   0.123      0.883 0.000 0.960 0.012 0.012 0.012 NA
#&gt; SRR1655015     2   0.113      0.884 0.000 0.964 0.008 0.012 0.012 NA
#&gt; SRR1655016     2   0.212      0.878 0.000 0.908 0.008 0.064 0.020 NA
#&gt; SRR1655017     2   0.185      0.885 0.000 0.924 0.008 0.052 0.016 NA
#&gt; SRR1655018     2   0.396      0.772 0.000 0.804 0.076 0.092 0.016 NA
#&gt; SRR1655019     2   0.447      0.744 0.000 0.776 0.072 0.104 0.024 NA
#&gt; SRR1655020     2   0.194      0.866 0.000 0.924 0.012 0.048 0.004 NA
#&gt; SRR1655021     2   0.194      0.866 0.000 0.924 0.012 0.048 0.004 NA
#&gt; SRR1655022     2   0.171      0.894 0.000 0.932 0.004 0.040 0.024 NA
#&gt; SRR1655023     2   0.178      0.893 0.000 0.928 0.004 0.044 0.024 NA
#&gt; SRR1655024     2   0.304      0.842 0.000 0.860 0.020 0.060 0.060 NA
#&gt; SRR1655025     2   0.343      0.806 0.000 0.832 0.020 0.064 0.084 NA
#&gt; SRR1655026     4   0.752      0.491 0.000 0.268 0.084 0.364 0.268 NA
#&gt; SRR1655027     4   0.755      0.494 0.000 0.276 0.088 0.360 0.260 NA
#&gt; SRR1655028     4   0.747      0.489 0.000 0.276 0.076 0.360 0.272 NA
#&gt; SRR1655029     4   0.744      0.480 0.000 0.264 0.072 0.364 0.284 NA
#&gt; SRR1655030     4   0.773      0.432 0.000 0.212 0.120 0.372 0.276 NA
#&gt; SRR1655031     4   0.769      0.450 0.000 0.212 0.120 0.392 0.256 NA
#&gt; SRR1655032     4   0.746      0.471 0.000 0.280 0.072 0.348 0.284 NA
#&gt; SRR1655033     4   0.745      0.482 0.000 0.284 0.072 0.356 0.272 NA
#&gt; SRR1655034     3   0.412      0.938 0.000 0.032 0.772 0.020 0.164 NA
#&gt; SRR1655035     3   0.412      0.938 0.000 0.032 0.772 0.020 0.164 NA
#&gt; SRR1655036     3   0.469      0.967 0.000 0.048 0.724 0.020 0.192 NA
#&gt; SRR1655037     3   0.488      0.965 0.000 0.048 0.712 0.020 0.196 NA
#&gt; SRR1655038     3   0.445      0.966 0.000 0.044 0.732 0.024 0.196 NA
#&gt; SRR1655039     3   0.450      0.963 0.000 0.044 0.724 0.024 0.204 NA
#&gt; SRR1655040     3   0.466      0.968 0.000 0.044 0.724 0.020 0.196 NA
#&gt; SRR1655041     3   0.482      0.967 0.000 0.044 0.716 0.020 0.196 NA
#&gt; SRR1655042     4   0.256      0.619 0.000 0.084 0.008 0.880 0.028 NA
#&gt; SRR1655043     4   0.245      0.620 0.000 0.084 0.004 0.884 0.028 NA
#&gt; SRR1655044     4   0.246      0.626 0.000 0.084 0.000 0.880 0.036 NA
#&gt; SRR1655045     4   0.246      0.626 0.000 0.084 0.000 0.880 0.036 NA
#&gt; SRR1655046     4   0.236      0.625 0.000 0.072 0.004 0.892 0.032 NA
#&gt; SRR1655047     4   0.242      0.624 0.000 0.076 0.004 0.888 0.032 NA
#&gt; SRR1655048     4   0.294      0.614 0.000 0.100 0.012 0.856 0.032 NA
#&gt; SRR1655049     4   0.289      0.613 0.000 0.104 0.008 0.856 0.032 NA
#&gt; SRR1655050     5   0.120      0.989 0.000 0.040 0.008 0.000 0.952 NA
#&gt; SRR1655051     5   0.132      0.982 0.000 0.036 0.016 0.000 0.948 NA
#&gt; SRR1655052     5   0.120      0.989 0.000 0.040 0.008 0.000 0.952 NA
#&gt; SRR1655053     5   0.132      0.982 0.000 0.036 0.016 0.000 0.948 NA
#&gt; SRR1655054     5   0.137      0.975 0.000 0.044 0.000 0.012 0.944 NA
#&gt; SRR1655055     5   0.137      0.975 0.000 0.044 0.000 0.012 0.944 NA
#&gt; SRR1655056     5   0.120      0.989 0.000 0.040 0.008 0.000 0.952 NA
#&gt; SRR1655057     5   0.120      0.989 0.000 0.040 0.008 0.000 0.952 NA
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0           ComplexHeatmap_2.1.1        markdown_1.1               
#>  [4] knitr_1.26                  cola_1.3.2                  SummarizedExperiment_1.14.1
#>  [7] DelayedArray_0.10.0         BiocParallel_1.18.1         matrixStats_0.55.0         
#> [10] Biobase_2.44.0              GenomicRanges_1.36.1        GenomeInfoDb_1.20.0        
#> [13] IRanges_2.18.3              S4Vectors_0.22.1            BiocGenerics_0.30.0        
#> [16] GetoptLong_0.1.7           
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6           bit64_0.9-7            doParallel_1.0.15      RColorBrewer_1.1-2    
#>  [5] httr_1.4.1             backports_1.1.5        tools_3.6.0            R6_2.4.1              
#>  [9] DBI_1.0.0              lazyeval_0.2.2         colorspace_1.4-1       withr_2.1.2           
#> [13] tidyselect_0.2.5       gridExtra_2.3          bit_1.1-14             compiler_3.6.0        
#> [17] xml2_1.2.2             microbenchmark_1.4-7   pkgmaker_0.28          slam_0.1-46           
#> [21] scales_1.1.0           NMF_0.23.6             stringr_1.4.0          digest_0.6.23         
#> [25] XVector_0.24.0         pkgconfig_2.0.3        bibtex_0.4.2           highr_0.8             
#> [29] rlang_0.4.2            GlobalOptions_0.1.1    RSQLite_2.1.2          impute_1.58.0         
#> [33] shape_1.4.4            mclust_5.4.5           dendextend_1.12.0      dplyr_0.8.3           
#> [37] RCurl_1.95-4.12        magrittr_1.5           GenomeInfoDbData_1.2.1 Matrix_1.2-17         
#> [41] Rcpp_1.0.3             munsell_0.5.0          viridis_0.5.1          lifecycle_0.1.0       
#> [45] stringi_1.4.3          zlibbioc_1.30.0        plyr_1.8.4             blob_1.2.0            
#> [49] crayon_1.3.4           lattice_0.20-38        splines_3.6.0          annotate_1.62.0       
#> [53] circlize_0.4.9         zeallot_0.1.0          pillar_1.4.2           rjson_0.2.20          
#> [57] rngtools_1.4           reshape2_1.4.3         codetools_0.2-16       XML_3.98-1.20         
#> [61] glue_1.3.1             evaluate_0.14          vctrs_0.2.0            png_0.1-7             
#> [65] foreach_1.4.7          polyclip_1.10-0        gtable_0.3.0           purrr_0.3.3           
#> [69] clue_0.3-57            assertthat_0.2.1       ggplot2_3.2.1          xfun_0.11             
#> [73] gridBase_0.4-7         eulerr_6.0.0           xtable_1.8-4           skmeans_0.2-11        
#> [77] survival_2.44-1.1      viridisLite_0.3.0      tibble_2.1.3           iterators_1.0.12      
#> [81] memoise_1.1.0          AnnotationDbi_1.46.1   registry_0.5-1         GTF_0.0.1             
#> [85] cluster_2.1.0          brew_1.0-6
```


