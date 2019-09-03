---
layout: post
title:  "snakemake：编写任务流程的工具"
date:   2019-09-01 
updated: 2019-09-01
categories: 生物信息
tags: 工具
excerpt: 随着生物信息分析的发展，越来越多的分析是一步一步由上往下的，先前我们更多的是用shell语言串联这些子分析，但它的缺点也很明显--可读性和重复性交叉。snakemake是基于python语言开发的编写分析流程的工具，它只需要配置号配置文件和输入文件，我们不用关心每个子分析的结果文件，只要在命令行输入输出结果名字即可。运行过程可以配置如软件运行参数、运行日志文件等等，这极大方便了生信分析人员。
---

### What's Snakemake?

**Snakemake**是一种可重复和标准化的编写任务流程的工具。该流程工具基于友好的python语言，并且可以跨平台运行。生信分析领域经常会构建分析流程，通常我们会用snakemake语言构建。除此之外，**[Broad institude](https://www.broadinstitute.org/)**（布罗德研究所：麻省理工学院和哈佛大学的Eli和Edythe L. Broad研究所，通常被称为Broad研究所，是一个位于美国马萨诸塞州剑桥的生物医学和基因组研究中心）提出的**[workflow description language](https://software.broadinstitute.org/wdl/)**，这也可用于生信标准流程开发。

> The Snakemake workflow management system is a tool to create **reproducible and scalable** data analyses. Workflows are described via a human readable, Python based language. They can be seamlessly scaled to server, cluster, grid and cloud environments, without the need to modify the workflow definition. Finally, Snakemake workflows can entail a description of required software, which will be automatically deployed to any execution environment.

```python
rule targets:
    input:
        "plots/dataset1.pdf",
        "plots/dataset2.pdf"

rule plot:
    input:
        "raw/{dataset}.csv"
    output:
        "plots/{dataset}.pdf"
    shell:
        "somecommand {input} {output}"
```

1. snakemake基于python语法定义处理输入文件的rule，每个rule对输入文件处理后得到输出文件；
2. 输入输出文件可以通过**通配符**匹配，从而进行多数据处理；
3. rule不仅仅包含shell命令，也可以是python或者其他处理输入输出文件的编程语言；
4. 安装snakemake软件最好通过**conda**（python3版本）安装：*conda install -c bioconda snakemake*。



### snakemake的规则

#### **rule**及其命令

1. **rule**是snakefile的基本组成，每个rule一般包含input、output和shell三部分。其中比较特殊的是rule all命令，*它一般被用来定义一个流程的最后输出结果*。

   ```python
   rule all:
       input:"path/to/inputfile", "path/to/other/inputfile"
   
   rule NAME:
     	#多个输入输出可用逗号链接
       input: "path/to/inputfile", "path/to/other/inputfile"
       output: "path/to/outputfile", somename = "path/to/another/outputfile"
   		shell: "somecommand {input} {output}"
   ```

   

2. **wildcards**：获得匹配输入数据名字部分的通配符。

   ```python
   rule complex_conversion:
       input:
           "{dataset}/inputfile"
       output:
           "{dataset}/file.{group}.txt"
       shell:
           "somecommand --group {wildcards.group} < {input} > {output}"
   # dataset,group是两通配符，只要文件名符合该格式就会被处理。
   ```

   

3. **threads**：指定某个rule的分配给程序的线程数目。

4. **resources**：指定程序所需要的内存大小。

5. **expand**：扩展函数，可以将输入或者输出结果存成list。

6. **message**：运行rule时，输出在终端的信息。

7. **log**：用来生成程序运行的日志文件。

8. **Priority**：指定程序运行的优先性。

9. **params**：程序运行的参数。

10. **shell**：指定运行命令。

11. **run**：指定运行python命令。

12. **scripts**：指定运行的脚本。

13. **temp**和**protected**：temp类型中间文件会在结果输出后删除；protected类文件会保留下来。

14. **directory**：指定输出结果是目录。

    ```python
    rule NAME:
        input:
            "path/to/inputfile"
        output:
            directory("path/to/outputdir")
        shell:
            "somecommand {input} {output}"
    ```



#### 配置文件

配置文件有两种类型：json和yaml。每次运行需要修改的文件或者参数等均可以写入配置文件，然后在snakefile内读入文件。

```yaml
samples: "Sample_path.tsv"

params:
  left_primer:  29
  
results:     
  fastqc:       "result/00.quality/fastqc"

logs: "result/logs"

# snakefile -->   configfile: "config.yaml" 
```



### 执行snakemake

#### 参数含义

```markdown
--dryrun, -n  不执行命令，用于debug
--printshellcmds, -p   在屏幕输出每个rule的shell命令
--reason, -r  输出每条rule执行的原因,默认FALSE
--cores, --jobs, -j  指定运行的核数，若不指定，则使用最大的核数
--force, -f 重新运行第一条rule或指定的rule
--forceall, -F 重新运行所有的rule，不管是否已经有输出结果
--forcerun, -R 重新执行Snakefile，当更新了rule时候使用此命令
--snakefile, -s 指定Snakefile，否则是当前目录下的Snakefile
```

#### 流程可视化命令

```shell
snakemake --dag result/06.taxonomy/taxonomy.silva.cluster.txt | dot -Tpdf > workflow.pdf
```

#### 集群投递

```markdown
snakemake --cluster "qsub -V -cwd -q 节点队列" -j 10
# --cluster /-c CMD: 集群运行指令
# qusb -V -cwd -q， 表示输出当前环境变量(-V),在当前目录下运行(-cwd), 投递到指定的队列(-q), 如果不指定则使用任何可用队列
# --local-cores N: 在每个集群中最多并行N核
# --cluster-config/-u FILE: 集群配置文件
```



### 转录组cufflinks分析案例

```python
# path to track and reference
TRACK   = 'hg19.gtf'
REF     = 'hg19.fa'


# sample names and classes
CLASS1  = '101 102'.split()
CLASS2  = '103 104'.split()
SAMPLES = CLASS1 + CLASS2


# path to bam files
CLASS1_BAM = expand('mapped/{sample}.bam', sample=CLASS1)
CLASS2_BAM = expand('mapped/{sample}.bam', sample=CLASS2)


rule all:
    input:
        'diffexp/isoform_exp.diff',
        'assembly/comparison'


rule assembly:
    input:
        'mapped/{sample}.bam'
    output:
        'assembly/{sample}/transcripts.gtf',
        dir='assembly/{sample}'
    threads: 4
    shell:
        'cufflinks --num-threads {threads} -o {output.dir} '
        '--frag-bias-correct {REF} {input}'


rule compose_merge:
    input:
        expand('assembly/{sample}/transcripts.gtf', sample=SAMPLES)
    output:
        txt='assembly/assemblies.txt'
    run:
        with open(output.txt, 'w') as out:
            print(*input, sep="\n", file=out)


rule merge_assemblies:
    input:
        'assembly/assemblies.txt'
    output:
        'assembly/merged/merged.gtf', dir='assembly/merged'
    shell:
        'cuffmerge -o {output.dir} -s {REF} {input}'


rule compare_assemblies:
    input:
        'assembly/merged/merged.gtf'
    output:
        'assembly/comparison/all.stats',
        dir='assembly/comparison'
    shell:
        'cuffcompare -o {output.dir}all -s {REF} -r {TRACK} {input}'


rule diffexp:
    input:
        class1=CLASS1_BAM,
        class2=CLASS2_BAM,
        gtf='assembly/merged/merged.gtf'
    output:
        'diffexp/gene_exp.diff', 'diffexp/isoform_exp.diff'
    params:
        class1=",".join(CLASS1_BAM),
        class2=",".join(CLASS2_BAM)
    threads: 8
    shell:
        'cuffdiff --num-threads {threads} {gtf} {params.class1} {params.class2}'
```

![](https://raw.githubusercontent.com/HuaZou/HuaZou.github.io/master/_posts/img/snakemake_cufflinks.png)



### 引用

1. [snakemake](https://snakemake.readthedocs.io/en/latest/index.html)
2. [snakemake使用笔记](https://www.jianshu.com/p/14b9eccc0c0e)


参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。
