# Snakemake
Hugh (Trevor) Redford, Noah Skinner, and Yifei Ding

# What is Snakemake
Before we begin, it is crucial to learn what Snakemake does. Snakemake is a workflow management system designed to produce reproducible data analyses that is highly adaptable with powerful tools for generalization. 

# Snakemake Background
Snakemake is utilized a lot in bioinformatics. It significantly enhances research efficiency by allowing researchers to focus on data analysis and interpretation rather than managing complex workflows or troubleshooting dependency issues. By automating repetitive tasks, it reduces the time and effort required to conduct analyses. It also ensures data integrity and accuracy through clear documentation of inputs, outputs, and steps, which enhances transparency and trust in the results. Additionally, Snakemake only re-runs affected tasks when changes occur, ensuring consistent and reliable outputs.

Collaboration is made easier with Snakemake's declarative workflows, which enable researchers from different institutions or disciplines to share and run identical workflows, fostering interdisciplinary research. This also supports open science by lowering barriers to entry for less experienced users, democratizing access to powerful data analysis tools. Moreover, Snakemake optimizes resource allocation and avoids unnecessary computations, reducing the environmental footprint of large-scale analyses.

In summary, Snakemake transforms data analysis by promoting reproducibility, minimizing manual errors, and streamlining processes across diverse computational environments. Its profound impact spans fields like bioinformatics and machine learning, improving the quality, transparency, and efficiency of research.

# How to Use Snakmake 

1. Install Snakemake via Conda
2. Create a workflow directory
3. Implement a workflow in the snakemake file (Snakefile)
4. Organize input data
5. Execute workflow


# Key Features of Snakemake 

|Key Feature| Summary|    
|-----------|:----------------:|
|Python-like Syntax                  | Snakemake workflows are written in a Python-based syntax, making them easy to learn, especially for those familiar with Python, while remaining beginner-friendly due to their readability. The syntax is both flexible and extendable, allowing users to integrate custom logic and Python functions directly into workflows, enabling dynamic adaptation to varying datasets. Its declarative structure ensures workflows are clear, easy to understand, and maintainable, with rules that explicitly define inputs, outputs, and commands.          |
|Workflow Nesting    | Snakemake supports modular workflow design through workflow nesting, allowing complex pipelines to be broken into smaller, manageable sub-workflows that can operate independently or as part of a larger system. This promotes reusability, as sub-workflows can be easily imported and reused across different projects, such as for common preprocessing steps. Workflow nesting also facilitates collaborative development by enabling teams or individuals to work on specific parts of the workflow while seamlessly integrating their contributions.          | 
|Integration with Tools and Services | Snakemake integrates seamlessly with tools and systems, supporting containerization with Docker and Singularity for portability and consistent dependencies. It efficiently submits jobs to HPC clusters or cloud platforms like AWS and adapts to schedulers like SLURM. With Git integration for version control, compatibility with tools like STAR and R, and notification features via Slack or email, Snakemake ensures versatile and reproducible workflow management.|
|Scalable     | Snakemake is highly scalable, supporting workflows from small, local setups to massive computations. It automatically parallelizes tasks by detecting independent dependencies and effectively utilizing available CPU cores. With cluster support, Snakemake optimizes job scheduling based on resource requirements, ensuring efficient execution without overloading systems. It also dynamically adjusts resource allocation during execution and supports cloud platforms, enabling researchers to leverage scalable computational resources as needed.             | 

# Writing a Snakemake Workflow

To write a Snakemake workflow, you first must create a Snakefile. This is the file where the workflow will be defined. Workflows are written in terms of rules, where each rule performs a specific task or operation.
A basic rule would look like this:

```python
rule example:
  input:
    "data.gz"
  output:
    directory("data/")
  shell:
    """
    mkdir -p {output}
    pigz -d {input} > {output}
    """
```

In Snakemake, rule are typically assigned an name (`Example`), an input (`data.gz`), an output (`data/`), and a shell.

Here, our shell is written as `pigz -d {input} > {output}`.
The `pigz -d` command will decompress a file with the `.gz ` ending.
The `{input}` and `{output}` in the shell command are variables respresenting the defined inputs and outputs. This means that the shell command will take the `data.gz` file and decompress it into the `data/` directory. Additionally, while shell uses terminal commands, you can replace `shell:` with `run:` to use Python code instead.

While this workflow performs the operation correctly, it has potential to be far more useful.

## Wildcards

Wildcards are a fundamental part of Snakemake. They allow for the generalization of rules to fit a variety of inputs/outputs. For example, we can expand the rule above using wildcards. This would then look like:

```python
rule example_with_wc:
  input:
    "{data}.gz"
  output:
    directory("{data}/")
  shell:
    """
    mkdir -p {output}
    pigz -d {input} > {output}
    """
```

If we were to have the files `set_one.gz`, `set_two.gz`, and `set_three.gz`, running the workflow with this new wildcard functionality would yield the `set_one/`, `set_two/` , and `set_three/` directories.

## The All rule

According to the Snakemake documentation, "Snakemake always wants to execute the first rule in the snakefile". This allows for the use of the all rule. The all rule signals Snakemake that this is the target rule. This works regardless of the position of the rule in the workflow. With regards to the previous workflow, it could be written using an all rule. 
      
```python
datasets= glob_wildcards("{data}.gz").data

rule all:
  input:
    expand("{data}/", data=datasets)
  
rule example_with_wc:
  input:
    "{data}.gz"
  output:
    directory("{data}/")
  shell:
    """
    mkdir -p {output}
    pigz -d {input} > {output}
    """
```
This workflow will perform the same as the previous workflow, but this new workflow showcases an interesting feature of Snakemake. 
The `expand() ` takes the target files / files with a variable placeholder (note that the `{data}` in the input of the all rule is NOT a wildcard) and provides each output based on the set provided. In this case, we create a dictionary using `glob_wildcards` to take any file in the working directory with the `.gz` ending. This dictionary is named `datasets` and is specified to be used by the `data=datasets`. 

# Basic Functionality

To chain multiple rules together in a workflow, Snakemake examines the input and outputs of each rule to determine which rules have "dependencies" (ie. which rules depend on the outputs of others). These dependencies can be modeled with a directed acyclic graph. A directed acyclic graph is a "directed graph that has no cycles". 
![Directed Acyclic Graph][

# Integration

Snakemake integrates seamlessly into various computational environments to enhance workflow efficiency and portability. It supports Conda integration, allowing users to define isolated environments for each workflow step, ensuring reproducibility and compatibility across systems. With HPC and cloud integration, Snakemake can run workflows on HPC clusters like SLURM or cloud platforms such as AWS, enabling scalability and efficient resource utilization. Additionally, its modularity with Snakedeploy allows users to easily deploy and adapt pre-built workflows from repositories, making it straightforward to reuse and customize workflows for specific needs.

# How is Snakemake used at UCSD?

At UCSD, Professor Zhong's lab uses Snakemake as the workflow management system for **MUSIC**, a bioinformatics tool designed to analyze specific types of biological data. Snakemake ensures that MUSIC's workflows are efficient, reproducible, and easy to manage by automating tasks, handling dependencies, and streamlining complex data analyses. This integration allows researchers in the lab to focus on scientific insights rather than the logistical challenges of managing computational pipelines.

# Significance of Music 

MUSIC is a powerful tool for analyzing molecular interactions, such as DNA-DNA, RNA-DNA, and RNA-RNA. What makes it stand out is its ability to incorporate factors like sex, age, and genetics into a joint analysis, offering deeper insights into chromatin and RNA interactions. To handle these workflows efficiently, Snakemake comes into play. It automates and scales the process, ensuring reproducibility and managing complex tasks seamlessly. Together, they create a robust framework for exploring the intricate relationships in human genetics and aging.
