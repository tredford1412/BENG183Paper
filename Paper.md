# Snakemake
Hugh (Trevor) Redford, Noah Skinner, and Yifei Ding

# What is Snakemake
Before we begin, it is crucial to learn what Snakemake does. Snakemake is a workflow management system designed to produce reproducible data analyses that is highly adaptable with powerful tools for generalization. 

# Snakemake Background
Snakemake is utilized a lot in bioinformatics. It significantly enhances research efficiency by allowing researchers to focus on data analysis and interpretation rather than managing complex workflows or troubleshooting dependency issues. By automating repetitive tasks, it reduces the time and effort required to conduct analyses. It also ensures data integrity and accuracy through clear documentation of inputs, outputs, and steps, which enhances transparency and trust in the results. Additionally, Snakemake only re-runs affected tasks when changes occur, ensuring consistent and reliable outputs.

Collaboration is made easier with Snakemake's declarative workflows, which enable researchers from different institutions or disciplines to share and run identical workflows, fostering interdisciplinary research. This also supports open science by lowering barriers to entry for less experienced users, democratizing access to powerful data analysis tools. Moreover, Snakemake optimizes resource allocation and avoids unnecessary computations, reducing the environmental footprint of large-scale analyses.

In summary, Snakemake transforms data analysis by promoting reproducibility, minimizing manual errors, and streamlining processes across diverse computational environments. Its profound impact spans fields like bioinformatics and machine learning, improving the quality, transparency, and efficiency of research.

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
    "data/"
  shell:
    "pigz -d {input} > {output}"
```

In Snakemake, rule are typically assigned an name (`Example`), an input (`data.gz`), an output (`data/`), and a shell.

Here, our shell is written as `pigz -d {input} > {output}`.
The `pigz -d` command will decompress a file with the `.gz ` ending.
The `{input}` and `{output}` in the shell command are variables respresenting the defined inputs and outputs. This means that the shell command will take the `data.gz` file and decompress it into the `data/` directory.
