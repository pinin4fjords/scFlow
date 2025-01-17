# scFlow 0.7.0

# scFlow 0.6.2

# *News*

# scFlow 0.6.0 _(10/11/2020)_

* Marker identification for groups of cells.  The `find_marker_genes()` function identifies marker genes for arbitrary groups of cells (e.g. celltypes, clusters).  The function is now invoked by the `annotate_celltype_metrics()` function to generate marker genes for clusters and celltypes which are presented in the updated `report_celltype_metrics()` function.

* Adaptive thresholding on mitochondrial counts with now enabled with `annotate_sce()` and reflected in the associated report `report_qc_sce()`.


# scFlow 0.5.0 _(14/05/2020)_

## New Features

* Adaptive thresholding for per-sample QC.  The `annotate_sce()` function now accepts "adaptive" as a threshold value for upper limits on various cell metrics together with a Median Absolute Deviation (MAD) threshold (default 3.5).  This allows QC thresholds to be determined on a per-sample basis, improving integration of datasets from different sources.  The `report_qc_sce()` function now plots and describes these adaptive thresholds.

* Dirichlet modeling for statistically significant differences in cell-type abundance.  The `model_celltype_freqs()` function allows flexible cell-type differential abundance analyses, with an accompanying function to generate a report `report_celltype_model()`.

* Cell type metric annotations generated by `annotate_celltype_metrics()` with an accompanying report produced by `report_celltype_metrics()`.  Currently includes three sections: (1) reduced dimensionality plots, (2) cell-type proportions by groups, and (3) distributions of various metrics for each cell-type.

* Differential gene expression reports with `report_de()` using the result table generated by the `perform_de()` function after differential expression analysis. The report includes all the parameters used during the differential expression analysis, a summary of the up and downregulated genes, a volcano plot showing top 10 up and down regulated genes, and the result table.

* Dataset integration performance analyses (including kBet) and plots produced by `annotate_integrated_sce()` with an accompanying report produced by `report_integrated_sce()`:
  
  - Finding outlier datasets (individuals): LIGER takes the union of variable genes across datasets and use them for integration. Therefore, it is important to find outlier datasets. Now `integrate_sce()` and `liger_preprocess()` functions have been modified in order to generate a list of variable genes for each of the datasets. The new `annotate_integrated_sce()` function generates Venn and Upset plots using this data in order to visualise sizes of isolated dataset participation to the total variable genes used for integration. This helps to identify outlier dataset(s).

  - Batch effect correction by LIGER: now `annotate_integrated_sce()` provides proofs for batch effect correction by LIGER. This function quantifies the batch effect caused by each of the categorical covariates for data generated by PCA vs LIGER. The kBET method is used for quantification of batch effects, and results are generated as rejection rate box plots as well as kBET test P-values. For each of the comparisons `annotate_integrated_sce()` also visualises the batch effects using tSNE plots.  `annotate_integrated_sce()` now generates UMAP plots to visualise clusters identified using PCA vs LIGER data.
        
  - Interactive report for dataset integration, dimension reduction, and clustering: The new `report_integrated_sce()` function generates an interactive report which includes method summary, key parameters used, Venn and Upset plots for variable genes used for integration, PCA vs LIGER side by side comparison of batch effect quantified by kBET and visualised by tSNE, and UMAP plots showing clusters identified using PCA vs LIGER data.

* Improvements in the QC report generated by `report_qc_sce()`, including new CSS styling.  Key information is now summarized at the beginning of the report.

* Pseudobulking algorithm improved to utilize matrix multiplication instead of lapply: typical 1-2 orders of magnitude speed increase.

# scFlow 0.4.0 _(14/02/2020)_

## New features

* `annotate_merged_sce()` and `report_merged_sce()` to examine QC metrics across samples to facilitate identification of problematic samples.

* `find_cells()` annotates cells / empty drops using the EmptyDrops algorithm.  The single-sample QC report generated by `report_qc_sce()` now includes EmptyDrops results with metrics and plots of  algorithm performance.

* `integrate_sce()` runs LIGER on a SingleCellExperiment created by `merge_sce()`, storing the H.norm factors in a `reducedDim()` slot available as an input for dimensionality reduction.

* `reduce_dims_sce()` now includes a parameter for `input_reduced_dim` which accepts one or more of PCA and Liger as input, producing `tSNE_PCA`, `tSNE_Liger`, `UMAP_PCA`, `UMAP_Liger` etc. allowing dimensionality reductions with and without integration to be examined.

* Improved annotation of SingleCellExperiment celltypes by `map_celltypes_sce()` to enable three new functions to write (`write_celltype_mappings()`), read (`read_celltype_mappings()`), and update (`map_custom_celltypes()`) the celltype mappings for a SingleCellExperiment.  This enables cluster -> celltype mappings to be quickly and easily revised if needed.

* `find_impacted_pathways()` and `report_impacted_pathways()` allow a differential gene expression table to be submitted to WebGestalt and ROntoTools (and in future additional tools) and an interactive report to be generated.
