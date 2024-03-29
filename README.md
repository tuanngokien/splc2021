

# [SPLC2021 Challenge Case]
# Variability Fault Localization: A Benchmark

Software fault localization is one of the most expensive, tedious, and time-consuming activities in program debugging. This activity becomes even much more challenging in Software Product Line (SPL) systems due to the variability of failures in SPL systems. These unexpected behaviors are caused by variability faults which can only be exposed under some combinations of system features. Although localizing bugs in non-configurable code has been investigated in-depth, variability fault localization in SPL systems still remains mostly unexplored. To approach this challenge, we propose a benchmark for variability fault localization with a large set of 1,570 buggy versions of six SPL systems and baseline variability fault localization performance results. Our hope is to engage the community and stimulate new and better approaches to the problem of variability fault localization in SPL systems. [[Preprint](https://arxiv.org/abs/2107.04741)]

## Dataset
We collected 6 Java SPL systems in [SPL2Go](http://spl2go.cs.ovgu.de/), which are widely used in the existing studies about configurable code, to construct our dataset. In addition, the products of each system are composed by [FeatureHouse](https://www.se.cs.uni-saarland.de/apel/fh/), a popular automated software composer. Full dataset of configurable systems with test suites and failure reports can be retrieved from the download links.

#### Version 1 - Updated at 2021-03-05

This dataset contains 1,773 buggy versions collected from 6 Java SPL systems. 
The V1 detailed description can be found [here](https://drive.google.com/file/d/1qknDzvqoZXUktkYogbJoJgbdjvfkfsfT/view?usp=sharing).
In addition, we later found that some cases that were not valid and should be removed. Hence, the dataset V2 is highly recommended to acquire in this challenge. 

[DOWNLOAD V1](https://mega.nz/folder/4xQljShQ#XLswm0SwfNInBxzQA4SUrQ)


#### Version 2 - Updated at 2021-05-04 [NEW]

This dataset contains 1,570 buggy versions collected from 6 Java SPL systems, statistics are summarized in the table below.

[DOWNLOAD V2](https://vnueduvn-my.sharepoint.com/:f:/g/personal/tuanngokien_vnu_edu_vn/EhSZsIs7k3BJjlFtOeub3KQBNcwmlT-A5pi7bZ-ezJfj1w?e=EvnKd4)
<br />
<br />


<table>
<thead>
  <tr>
    <th colspan="2" rowspan="2">System</th>
    <th colspan="2">Details</th>
    <th colspan="3">Test Info</th>
    <th colspan="2">Bug info</th>
  </tr>
  <tr>
    <td>#LOC</td>
    <td>#Features</td>
    <td>#SP</td>
    <td>#Tests</td>
    <td>Cov</td>
    <td>#Versions</td>
    <td>#IF</td>
  </tr>
</thead>
<tbody>
  <tr>
    <td colspan="2">ZipMe</td>
    <td>3460</td>
    <td>13</td>
    <td>25</td>
    <td>255.0</td>
    <td>42.9</td>
    <td>304</td>
    <td>2.7</td>
  </tr>
  <tr>
    <td colspan="2">GPL</td>
    <td>1944</td>
    <td>27</td>
    <td>99</td>
    <td>86.9</td>
    <td>99.4</td>
    <td>372</td>
    <td>13.0</td>
  </tr>
  <tr>
    <td colspan="2">Elevator-FH-JML</td>
    <td>854</td>
    <td>6</td>
    <td>18</td>
    <td>166.0</td>
    <td>92.9</td>
    <td>122</td>
    <td>3.6</td>
   </tr>
  <tr>
    <td colspan="2">ExamDB</td>
    <td>513</td>
    <td>8</td>
    <td>8</td>
    <td>133.3</td>
    <td>99.5</td>
    <td>263</td>
    <td>1.1</td>
  </tr>
  <tr>
    <td colspan="2">Email-FH-JML</td>
    <td>439</td>
    <td>9</td>
    <td>27</td>
    <td>86.0</td>
    <td>97.7</td>
    <td>126</td>
    <td>4.1</td>
  </tr>
  <tr>
    <td colspan="2">BankAccountTP</td>
    <td>143</td>
    <td>8</td>
    <td>34</td>
    <td>19.8</td>
    <td>99.9</td>
    <td>383</td>
    <td>4.8</td>
  </tr>
</tbody>
</table>

<sup>#SP, #IF stand for size of sampled products and number of involving features, respectively. </sup>

## Baseline results
To construct baseline results, we conducted experiments with the naive adaption of Spectrum-Based
Fault Localization (SBFL), which considers the whole SPL system as a non-configurable code. The suspiciousness score of a statement is measured based on the tests counted from all the products. We use this adaption with different SBFL metrics to evaluate the baseline performance on localizing variability bugs in both single-bug and multiple-bug settings.

Experimental results can be found in related dataset.

## Artifact Structure

  
    ├── model.m                   # Feature model file structured in GUISDL format
    ├── model.m.ca4.csv           # Configuration sampling result file is produced by SPLCAtool 
    ├── configs/                  # Configuration files have been sampled **only Original System**
    ├── features/                 # Java source files of all the features
    ├── lib/                      # Java dependencies required when run products
    ├── variants/                 # All products (variants) composed based on configuration files in [configs/] 
    │   ├── model_m_ca4_0001/
    │   │   ├── src/               # Product's Java source code composed by FeatureHouse
    │   │   ├── test/              # Unit Test source code generated by Evosuite
    │   │   ├── build/             # Java classes built from [src/] and [test/]
    │   │   ├── coverage/          # Test execution data recorded by OpenClover **only Buggy System**
    │   │   │   ├── passed/                           # Coverage data of each passed test
    │   │   │   ├── failed/                           # Coverage data of each failed test
    │   │   │   ├── spectrum_passed_coverage.xml      # Program spectra aggregated from [passed/]
    │   │   │   ├── spectrum_failed_coverage.xml      # Program spectra aggregated from [failed/]
    │   │   ├── roles.meta         # Composed method mapping provided by FeatureHouse
    │   │   ├── batch.test.X.flag  # Overall test result of current product (X = passed/failed)
    │   ├── model_m_ca4_0002/
    │   ├── model_m_ca4_0003/
    │   ├── ...
    ├── X.mutant.log             # location of the modification in feature source code
    ├── config.report.csv.done   # a text file for reporting overall test result of each product in the system

---
**NOTE**

- Each variant folder is labeled based on its corresponding configuration file.
- A variant name in different versions of the same system is composed by the same configuration files, which is located in [configs/] folder of Original System
- Program spectra files are aggregated directly from test execution data ([passed/] and [failed/]) in the same product. They are used as input to SBFL technique to locating buggy statements.
- To run the unit tests for a product, participant need to compile source code with Java 8 and include all custom dependencies in [lib/], Junit v4.12, and Evosuite v1.0.6 jar files. 

---


### *Feature Remapping
For remapping purposes, the link  between each code statement in a product and the corresponding statement in the feature is recorded.

`[FEATURE_SOURCE_FILE] /features/BankAccount/Account.java`
```javascript
28| 	boolean undoUpdate( int x )
29| 	{
30|     	int newBalance = balance - x;
31|     	if (newBalance < OVERDRAFT_LIMIT) {
32|         	     return false;
33|		}
34|     	balance = newBalance;
35|     	return true;
36| 	}
``` 
<br/>
<br/>

`[COMPOSED_PRODUCT_SOURCE_FILE] /src/Account.java`
```javascript

38| 	//__feature_mapping__ [BankAccount] [28:36]
39| 	boolean undoUpdate( int x )
40| 	{
41|     	int newBalance = balance - x;
42|     	if (newBalance < OVERDRAFT_LIMIT) {
43|         	     return false;
44|		}
45|     	balance = newBalance;
46|     	return true;
47| 	}
``` 
<br/>
<br/>

`[COMPOSED_PRODUCT_COVERAGE_FILE] /coverage/spectrum_passed_coverage.xml`
```xml

<file path="Account.java" name="Account">   
   <line num="28" count="3" type="stmt" featureClass="BankAccount.Account" featureLineNum="20" /> 
   <line num="29" count="3" type="stmt" featureClass="BankAccount.Account" featureLineNum="21" buggy="true" />  
   <line num="30" count="1" type="stmt" featureClass="BankAccount.Account" featureLineNum="22" /> 
   <line num="32" count="2" type="stmt" featureClass="BankAccount.Account" featureLineNum="24" /> 
   <line num="33" count="2" type="stmt" featureClass="BankAccount.Account" featureLineNum="25" /> 
   <line num="39" count="3" type="method" featureClass="BankAccount.Account" featureLineNum="28" /> 
   <line num="41" count="3" type="stmt" featureClass="BankAccount.Account" featureLineNum="30" /> 
   <line num="42" count="3" type="stmt" featureClass="BankAccount.Account" featureLineNum="31" />  
   <line num="43" count="1" type="stmt" featureClass="BankAccount.Account" featureLineNum="32" /> 
   <line num="45" count="2" type="stmt" featureClass="BankAccount.Account" featureLineNum="34" /> 
   <line num="46" count="2" type="stmt" featureClass="BankAccount.Account" featureLineNum="35" />
</file>
```

## Related tools

- SPLCAtool - Software Product Line Covering Array Tool implements various algorithms related to covering arrays of feature models for product lines. [[Link]](https://martinfjohansen.com/splcatool/) 
- Mujava -  A mutation system for Java programs [[Link]](https://cs.gmu.edu/~offutt/mujava/)
- FeatureHouse - Automated Software Composition [[Link]](https://www.se.cs.uni-saarland.de/apel/fh/)
- Apache Ant - A command-line tool to build Java applications [[Link]](https://ant.apache.org/)
- Junit 4 - A simple framework to write repeatable tests [[Link]](https://junit.org/junit4/)
- EvoSuite - Automatic Test Suite Generation for Java - automatically generates test cases with assertions for classes written in Java code [[Link]](https://www.evosuite.org/)
- OpenClover -  Code coverage measurement tool for Java [[Link]](http://openclover.org/)

## Cite us
```
@inproceedings{ngo2021variability,
  title={Variability fault localization: a benchmark},
  author={Ngo, Kien-Tuan and Nguyen, Thu-Trang and Nguyen, Son and Vo, Hieu Dinh},
  booktitle={Proceedings of the 25th ACM International Systems and Software Product Line Conference-Volume A},
  pages={120--125},
  year={2021}
}
```
