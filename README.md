# RADseq_Containers
## User-guide for users of the RADseq Containerized Pipelines (HCMR)

> Restriction-site associated DNA sequencing is a fractional genome sequencing strategy, that samples at reduced complexity across target genomes. Through the use of a restriction nuclease and attaching a series of adapters to the resulting DNA fragments, large numbers of genetic variations such as SNPs can be readily identified from analysis of next generation DNA sequence data. One can use a single or double-digest restriction site-associated DNA sequencing using the according protocol. The following workflows refer to ddRAD analyses. 

<br>

![ddRAD-seq](/ddradseq.png)

<br>

## **What you will find and what will you need to use it**
> The pipelines are made using [Snakemake](https://snakemake.readthedocs.io/en/stable/index.html) and containerized through [Singularity](https://sylabs.io/guides/3.5/user-guide/) with the help of the [Conda](https://docs.conda.io/en/latest/) package manager. If you are not familiar with any of these tools, don't worry. All you have to know, is that these technologies, give you the opportunity to transfer the workflow on any machine, as long as it works with a linux-based kernel and has Singularity installed. If these apply, you can run the pipeline effortlessly, without worrying about releases and packages that may not be compatible with each other. Conda handles that, and creates environments for the Snakemake workflow to run, without interferring with the system you are hosting it into.

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;![SCS](/scs.png)


> The marriaging of these three tools, create the pipelines and deliver them to you in the form of container images.
> Each ready to use pipeline is represented by an image, and can be run in any cluster or environment that supports the resources and 3d parties tools needed. 
>> In order for you to have some impact on the analysis and to also include your preferences in the procedure, the including and careful filling of a configuration file with lots of parameters is mandatory.

<br>

> When it comes to the analysis itself, you can choose between a de-novo or a reference based approach, who's steps is clesrly defined in the [STACKS](https://catchenlab.life.illinois.edu/stacks/) tool, the main algorithm used in such analyses:

<br>

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 
![Pipelines-STACKs](/STACKs.jpg)

<br>


**A. DeNovo_ddRAD**

> This image is suitable for those who want to run a simple quality control for their short (Illumina) reads, before and after trimming them, when planning to use long and short reads combination for assembling a genome. It actually performs quality check on the raw data, trimming of the raw data and quality check all over again. The results of the image are the following, located in the same directory your raw data live in:

1. One directory that contains the now trimmed short data, named *trimmomatic_output*
2. One directory that contains the results of the quality control of the raw short reads, named *Multiq_raw_report*
3. One directory that contains the results of the quality control of the trimmed short reads, named *Multiq_trimmed_report*

> Here are the tools used for this analysis:

| Tools       | Description        | Version |
| ------------- |:-------------:| -----:|
| fastqc     | Quality check | 0.11.8 |
| Multiqc     | Quality check | 1.6 |
| Trimmomatic    | Trimming     | 0.39  |

<br>

***
**B. Ref_ddRAD**

> Ref_ddRAD is an image suitable for those who want to run a simple quality control for their long (MinION) reads, before and after trimming them. It actually performs quality check, trimming of the raw data and quality check all over again. The results of the image are again, the following three, located in the same directory your long, raw data live in:

1. One fastq file that contains the now trimmed data, named *porechop_output.fastq.gz*
2. One directory that contains the results of the quality control before trimming, named *Nano_Raw_Report*
3. One directory that contains the results of the quality control after the trimming, named *Nano_Trimmed_Report*

> Here are the tools used for this analysis:

| Tools       | Description        | Version |
| ------------- |:-------------:| -----:|
| NanoPlot     | Quality check | 1.29.0 |
| Porechop    | Trimming     |  0.2.3 |

<br>

***


> Thus, in order for you to run your analysis in your organism's or industry's server, all you need is:

1. A server that has Singularity installed
2. The image of the pipeline you wish to use for your analysis
3. The corresponding configuration file along with the image, that lets you define different parameters for the analysis
4. A script that submits the image as a job into the nodes of your cluster


## **Breaking down the steps that lead to the analysis**
> Having found the image that correspond to the analysis you need, download it and copy it along with the configuration file in the *home folder* of your cluster account. A simple cp or a scp will do the trick. Once you're done, open the configuration file with *nano* to edit it. The configuration file is probably the most important component for your analysis to work, so take your time and double check all the paths and the parameters needed here. The file is made to work for all the currently available analyses, so your intervention and filling will in fact let the whole system know which image you are going to use and thus the goal of your chosen pipeline, so make sure to fill the right variables in order for your image to be executed properly. Inside the configuration file you will also find some requirements that your raw data should fulfill. If your data or data directories do not follow the standards, please make the appropriate changes before editing the configurations. The cleaness of your directories is also very important. The workflow generates many intermediate files while running, and deletes them when not using them anymore, in order for your outputs to be carefully and tidy arranged, and your account free of unnecessary files. In order for the workflow to not mistakenly read or delete a file with the same name or regular expression of the file it is truly looking for, the directory where you copy the image and the configuration file into should be empty, and the directories of your raw data should not contain **anything** more or less than the data itself. Clean the necessary directories before filling the configuration file, to avoid any mistakes. If you wish to re-run the workflow for any reason (errors or verifications), make sure to delete or move **any** already created output before proceeding.<br>
> Once done with all that, it's time to let the automated workflow do the rest for you. Follow the standars for running a job on your server's cluster and submit the image as follows (replace <image.simg> with the name of the actual image you have chosen, and add any path needed):
```
singularity run <image.simg>
```
> When the workflow is done, check carefully if all the files that should have been spawned are present in your directories, as and their status in the *Summary.txt* file. Ignore any other file mentioned, that may be spawned intermediately and has already been deleted by the workflow a priori. 


***
#### Pipelines:
> If you wish to use our images, get in contact with us for the necessary files.

#### Developers:
> If you are a developer and you wish to explore our code and learn more about these technologies, ask for access to our GitHub repository for developers and maintainers.

#### Credits and Citations:
> * Köster, Johannes and Rahmann, Sven. “Snakemake - A scalable bioinformatics workflow engine”. Bioinformatics 2012. <br>
> * Kurtzer GM, Sochat V, Bauer MW (2017), Singularity: Scientific containers for mobility of compute. PLoS ONE 12(5): e0177459. <br> 
> * Crist-Harif et al, Conda, GitHub repository: https://github.com/conda. <br>
> * Andrews S. (2010). FastQC: a quality control tool for high throughput sequence data. Available online at: http://www.bioinformatics.babraham.ac.uk/projects/fastqc. <br>
> * Li H, Handsaker B, Wysoker A, et al. The Sequence Alignment/Map format and SAMtools. Bioinformatics. 2009;25(16):2078‐2079.


#### Please credit accordingly:
> Author: Nellie Angelova, Bioinformatician, Hellenic Centre for Marine Research (HCMR) <br>
> Coordinator/ Supervisor: Tereza Manousaki, Researcher, Hellenic Centre for Marine Research (HCMR) <br>
> Contact: n.angelova@hcmr.gr
