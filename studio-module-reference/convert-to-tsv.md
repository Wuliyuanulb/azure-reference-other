---
title: "Convert to TSV | Microsoft Docs"
ms.custom: ""
ms.date: 03/02/2017
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: 1cdbcda4-2ece-4908-8b87-e6b636258d3d
caps.latest.revision: 18
author: "jeannt"
ms.author: "jeannt"
manager: "jhubbard"
---
# Convert to TSV
*Converts data input to a tab-delimited format*  
  
 Category: [Data Format Conversions](data-format-conversions.md)  
  
## Module Overview  
 You can use the [Convert to TSV](convert-to-tsv.md)module to convert any dataset from the internal format that is used by all Azure Machine Learning Studio modules, to a flat file in tab-separated format. Tab-separated value (TSV) files are compatible with many external tools, including:  
  
-   R and Python  
  
-   Excel and PowerPivot  
  
-   All relational databases  
  
 For example, if your experiment has an intermediate dataset that you would like to save for re-use in another tool or would like to call from code, you convert it to the TSV format, and then right-click the converted dataset to get the Python code needed to access the dataset.  
  
## How to Use [Convert to TSV](convert-to-tsv.md)  
Use the [Convert to TSV](convert-to-tsv.md) module whenever you need to download a dataset in tab-delimited format.  
  
1.  Add the [Convert to TSV](convert-to-tsv.md) to your experiment. You can find this module in the [Data Format Conversions](data-format-conversions.md) group in the **experiment items** list in Azure Machine Learning Studio. 
  
2.  Connect the module to another datset, or to a module that outputs a tabular dataset.

3. Run the experiment, or right-click just the [Convert to TSV](convert-to-tsv.md) module, and select **Run selected**.  
  
### Results

When conversion is complete, you can open the dataset, call it from R or Python code, use it in a Jupyter notebook, or save it to a local file.

If you want to download the dataset, double-click the module output, and indicate whether you want to open or save the datset.  
  
-   If you select **Open**, the dataset is loaded using whatever tool your computer uses by default to open .TSV files. Typically this is Microsoft Excel.  
  
-   If you  select **Download dataset**, by default, the file is saved with the name of the module plus a GUID representing the workspace ID. However, you can select the **Save As** option during download and change the file name or location.  
  
## Examples  
 Although there are no examples that are specific to this format, you can see examples of how format conversion is used by exploring these sample experiments in the [Model Gallery](https://gallery.cortanaintelligence.com/):  
  
-   The [Cross Validation for Binary Classification sample](http://go.microsoft.com/fwlink/?LinkId=525734) exports the results of cross validation to the comma-separated value (CSV) format so that results for multiple models can be compared by using a tool such as Excel.  
  
-   The [Color-Based Image Compression Quantization](http://go.microsoft.com/fwlink/?LinkId=525272) sample exports the datasets that are used for each portion of the analysis to CSV files, so that you can easily run a similar model in any tool that supports the CSV format.  
  
## Technical Notes  
 Tab-separated values (TSV) is a text format that is used to store data in a tabular structure. It is very similar to the CSV format, but the delimiter is a tab rather than a comma.  
  
 The TSV format is a useful alternative to the CSV format if your data contains commas. Commas are very common in text data and they are used in European number formats.  
  
 One problem with the tab-delimited format is that tab stops are frequently considered as white space in unstructured text. However, the IANA standard for TSV fosters clean and accurate parsing of TSV files by disallowing tabs within fields.  
  
 Note the following requirements for TSV files in Azure Machine Learning Studio:  
  
-   The [Convert to TSV](convert-to-tsv.md) module supports the output of a single heading row, if the dataset contains column names.  
  
-   The TSV provider supports UTF-8 character encoding only.  
  
-   When reading from or writing to TSV files, performance can be slower than with other formats (such as CSV).  
  
##  <a name="ExpectedInputs"></a> Expected Input  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Dataset|[Data Table](data-table.md)|Input dataset|  
  
##  <a name="Outputs"></a> Output  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Results dataset|GenericTsv|Output dataset|  
  
## See Also  
 [Data Format Conversions](data-format-conversions.md)   
 [A-Z Module List](a-z-module-list.md)