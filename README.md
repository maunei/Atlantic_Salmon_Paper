![Papers RAG - NotebookLM infographic overview](infographic_app2.png)

# Reproducibility Assessment

This repository documents the application of a human-in-the-loop, AI-assisted system designed to facilitate the reproduction of computational analyses presented in a bioinformatics/genomics paper.

The reproducibility assessment begins on a local workstation with limited computational resources. The system first ingests the paper using the workflow described in the following repository and summarized in the infographic above:

https://github.com/maunei/papers-rag_and_reproducibility

After conducting preliminary analyses and documenting the workstation's limitations, the workflow moves to a high-performance computing (HPC) cluster, where the goal is to reproduce the paper's computational analyses independently and as completely as possible.

Beyond evaluating reproducibility, this effort generates reusable intermediate files, workflows, and documentation that can support new independent analyses.

The assessment focuses on rerunning a substantial portion of the paper’s analyses on a high-performance computing (HPC) cluster. The workflow starts from publicly deposited raw data whenever possible and explicitly documents cases in which processed objects or other author-derived information were required.


Claude Science is used on the HPC cluster to direct computations, while a more polished report presenting the paper and its analysis is produced through AI–human interaction.

## Full reproducibility report

> **Paper analysed:** Carbonneau et al. (2026), *Evolutionary relationships between landlocked and anadromous Atlantic Salmon populations in the North Shore region of the Gulf of St. Lawrence*.

The complete illustrated account of the analysis is available in [Reproducibility Report v3](comprehensive_report_v3.md).