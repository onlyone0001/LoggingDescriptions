<p align="center"> <a href="https://github.com/logpai"> <img src="https://github.com/logpai/logpai.github.io/blob/master/img/logpai_logo.jpg" width="425"></a></p>

# LoggingDescriptions
This repository maintains a set of _<code, log>_ pairs extracted from popular open-source projects, which are amendable to logging description generation research. More details about the dataset can be found in our paper:

+ Pinjia He, Zhuangbin Chen, Shilin He, Michael R. Lyu. [Characterizing the Natural Language Descriptions in Software Logging Statements](https://pinjiahe.github.io/papers/ASE18.pdf), *in Proc. of the 33rd ACM/IEEE International Conference on Automated
Software Engineering (ASE)*, 2018.

The projects are listed as follows, including 10 Java projects and 7 C# projects:

| No | Java Projects        | C# Projects            |  
| :------:|:-------------: |:-------------:|
| 01 |ActiveMQ		|Azure SDK	|
| 02 |Ambari		|CoreRT		|
| 03 |Brooklyn		|CoreFX		|
| 04 |Camel      	|Mono		|
| 05 |CloudStack 	|MonoDevelop	|
| 06 |Hadoop    	|Orleans	|
| 07 |Hbase     	|SharpDevelop	|
| 08 |Hive		|		|
| 09 |Synapse		|		|
| 10 |Ignite  		| 		|

Each folder of a project includes the following two files:

* *(project)_code_log_pairs.txt*: This file contains all the code-log pairs extracted from the project. The pairs from different files of the project are separated.
* *file_trace.txt*: To facilitate our data processing, different files of a project are renamed in the form of "sameple_ID". This file is used to help readers trace back to the original file.

## Data Extraction
In the paper, each _<code, log>_  pair is extracted from a single function and composed of two parts: the code text and the logging description. The code text contains 10 lines (if it has) of code statements preceeding the studied logging statement. The logging description contains the descriptive text in the same logging statement. Non-description parts such as variables are removed.

Processing Details:
1. All empty lines are skipped.
2. All English characters are converted to their lower cases.
3. In code text part, code lines are separeted by \tab.
4. Log statements that do not contain any description text are not considered as logging description but ordinary code statement in this dataset.
5. The extracted preceeding 10 lines of code statements do not exceed current function scope (see the following example for details).

## A Simplified Example 
For easy demonstration, in the following Java example, we simply extract **6 lines** of code insteaed of 10 for the code text part.

```java
public	void catchException() {
try {
		operation 1;
		operation 2;

	} catch (Exception1 e1) {
		LOGGER.error("Exception 1 happens", e1);

	} catch (Exception2 e2) {
		LOGGER.error(e2);

	} catch (Exception3 e3) {
		LOGGER.error("Exception 3 happens", e3);
	}
}
```

In this function, two _<code, log>_ pairs can be extracted (\tab indicates new lines of code statement):

* **_<code, log>_ pair 1:**

Code Text:
<pre>
public void catchexception() {     try {     operation 1;     operation 2;     } catch (exception1 e1) {
</pre>

Logging Description:
<pre>
exception 1 happens
</pre>

* **_<code, log>_ pair 2:**

Code Text:
<pre>
operation 2;     } catch (exception1 e1) {     } catch (exception2 e2) {     logger.error(e2);     } catch (exception3 e3) {
</pre>

Logging Description:
<pre>
exception 3 happens
</pre>


#### Further Explanation:
1. Logging statement "LOGGER.error(e2);" can not produce a _<code, log>_ pair since it does not contain any descriptive text except a variable. This kind of statement is treated as an ordinary code line, see **_<code, log>_ pair 2**, while others with descriptive text will not appear in the code part of any pairs, see **_<code, log>_ pair 1**.
2. In **_<code, log>_ pair 1**, the code text contains only 5 (<6) code lines, but it will not include code outside the function.

## Cite
If you use this dataset, please cite our paper using the following reference:<br /><br />
> @inproceedings{he2018characterizing, <br />
  title={Characterizing the natural language descriptions in software logging statements}, <br />
  author={He, Pinjia and Chen, Zhuangbin and He, Shilin and Lyu, Michael R}, <br />
  booktitle={Proceedings of the 33rd ACM/IEEE International Conference on Automated Software Engineering}, <br />
  pages={178--189}, <br />
  year={2018}, <br />
  organization={ACM} <br />
} <br />

## License
All datasets in this repository will follow the MIT license for free reuse.

