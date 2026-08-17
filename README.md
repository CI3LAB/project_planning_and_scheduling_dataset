# project_planning_and_scheduling_dataset

## Overview

This repository provides a curated collection of optimization-modeling instances for two project planning and scheduling problem families:

1. **Resource-Constrained Project Scheduling Problem (RCPSP)**; and
2. **Work Package Sizing under Strict Precedence (WPS-SP)**.

The repository serves two complementary purposes. First, it provides an open research resource for researchers, students, and practitioners studying project scheduling, optimization modeling, exact and heuristic algorithms, and optimization-oriented large language models (LLMs). Second, selected problems are prepared as contributions to **OR-Bench**, the community benchmark for evaluating LLMs on optimization modeling tasks.

> **Attribution requirement.** If you use, redistribute, adapt, benchmark on, or publish results based on any problem instance, problem statement, formulation, data file, or solver implementation from this repository, please provide appropriate attribution to this repository and, where applicable, to the original source identified in the instance metadata.

---

## Scope and Research Objectives

This repository is designed to support research on:

- optimization modeling and mathematical formulation;
- project planning and scheduling;
- exact algorithms, decomposition methods, heuristics, and metaheuristics;
- MILP solver performance and computational benchmarking;
- LLM-based optimization modeling;
- model elicitation from vague business descriptions;
- translation from precise operational specifications to mathematical programs;
- formulation correctness, constraint completeness, and objective-function interpretation;
- reproducibility and comparison of optimization methods across standardized instances.

The repository is not intended merely as a collection of numerical test files. Each benchmark problem should be interpretable as an operational decision problem and should contain sufficient information to reconstruct, formulate, implement, and validate the optimization model.

---

## Problem Family I: Resource-Constrained Project Scheduling Problem

### Business-Level Description

We manage a project made up of many interdependent activities, such as fabrication, assembly, inspection, testing, and other production or engineering tasks. Some activities must be completed before others can start, while several activities may compete for the same limited workers, machines, equipment, or other resources. We need to decide when each activity should be carried out so that all technological dependencies are respected, resource capacities are not exceeded, and the entire project is completed as early as possible.

### Problem Definition

Let \(N=\{1,\ldots,n\}\) denote the set of project activities and \(K\) the set of renewable resources. Activity \(i\in N\) has processing duration \(p_i\) and requires \(r_{ik}\) units of resource \(k\in K\) while it is being processed. Resource \(k\) has capacity \(R_k\). Technological dependencies are represented by a precedence set \(E\), where \((i,j)\in E\) means that activity \(j\) cannot start before activity \(i\) is completed.

The principal scheduling decision is the start time \(S_i\) of each activity. A feasible schedule must satisfy all precedence relations, all renewable-resource capacity constraints at every relevant time, and non-preemptive activity execution unless an individual instance explicitly states otherwise. The standard objective is to minimize the project makespan \(C_{\max}\).

A generic formulation can therefore be summarized as

\[
\min C_{\max},
\]

subject to

\[
S_j \ge S_i+p_i, \qquad \forall (i,j)\in E,
\]

and

\[
\sum_{i:\,S_i\le t<S_i+p_i}r_{ik}\le R_k,
\qquad \forall k\in K,\ \forall t.
\]

Individual benchmark instances may use a time-indexed MILP or an equivalent exact formulation. The instance documentation is authoritative regarding horizon definitions, dummy activities, resource types, and any additional restrictions.

---

## Problem Family II: Work Package Sizing under Strict Precedence

### Business-Level Description

We manage a complex production or engineering project consisting of many small, interdependent tasks. Before execution, we must decide how these tasks should be bundled into work packages and assigned to responsible teams or managers. Many small work packages provide greater scheduling flexibility and tighter control, but they increase administrative, reporting, coordination, and monitoring effort. Larger work packages can reduce these costs and create economies of scale, but they may restrict parallel work and delay downstream activities. We therefore want to determine how tasks should be grouped so that the project remains operationally feasible, meets its delivery deadline, and achieves the lowest overall project cost.

### Problem Definition

Let \(N=\{1,\ldots,n\}\) be the set of elemental tasks and \(A\) the task-level precedence relation. Each task \(a\in N\) has a duration \(t_a\) and work content \(x_a\). The project must be completed no later than deadline \(d\).

The key decision is to partition eligible tasks into work packages

\[
W_1,\ldots,W_p.
\]

For a work package \(W\),

\[
x_W=\sum_{a\in W}x_a
\]

denotes its total work content. Its duration is determined by the critical path of the tasks contained in the package, according to the instance specification.

Under **strict precedence**, work packages are treated as scheduling-level activities. If a task in work package \(W_i\) precedes a task in work package \(W_j\), the induced package-level relationship requires \(W_i\) to precede \(W_j\). Consequently, a grouping decision is feasible only if the resulting work-package precedence network remains acyclic. Forming larger work packages may remove task-level opportunities for concurrent processing and can therefore increase project makespan.

The optimization problem determines the number and composition of work packages to minimize total project cost while satisfying the deadline:

\[
\min TC
\]

subject to

\[
C_{\max}\le d
\]

and feasibility of the strict-precedence work-package network.

Depending on the benchmark instance, \(TC\) may include fixed work-package administration cost, estimation-related cost, monitoring and control cost, economies-of-scale effects, and discounted cash-flow effects. Any mandatory or preassigned task groups are explicitly identified in the corresponding instance metadata.

The WPS-SP benchmark family is based on the strict-precedence work-package perspective studied in:

> Li, C.-L., & Hall, N. G. (2019). *Work Package Sizing and Project Performance*. **Operations Research, 67**(1), 123–142. https://doi.org/10.1287/opre.2018.1767

Where an instance is adapted from, generated according to, or otherwise materially based on published work, the relevant source must also be cited.

---

## Dataset Description

This repository contains **1,590 benchmark instances** across two project optimization problems.

### RCPSP Dataset

The RCPSP instances are generated using **RanGen2** with four control parameters:

| Parameter | Values |
|---|---|
| Number of tasks \(n\) | 20, 30, 40 |
| Network structure \(I_2\) | 0.2, 0.5, 0.8 |
| Resource factor \(RF\) | 0.25, 0.50, 0.75, 1.00 |
| Resource strength \(RS\) | 0.20, 0.50, 0.70, 1.00 |

Ten instances are generated for each parameter combination, giving

\[
3 \times 3 \times 4 \times 4 \times 10 = \mathbf{1,440}
\]

RCPSP instances.

Here, \(I_2\) characterizes the serial/parallel structure of the project network: smaller values indicate more parallel networks, whereas larger values indicate more serial networks. \(RF\) controls the extent of resource usage, while \(RS\) controls resource-capacity tightness.

### WPSP-SP Dataset (Still being supplemented)

The WPSP-SP instances are generated following the framework of Li and Hall (2019), *Work Package Sizing and Project Performance*. The main parameters are:

| Parameter | Values |
|---|---|
| Number of tasks \(n\) | 20, 30, 40 |
| Network structure \(I_2\) | 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0 |
| Inactive-task percentage | 0.1, 0.2, 0.3, 0.4, 0.5 |

Ten instances are generated for each parameter combination, resulting in

\[
3 \times 10 \times 5 \times 10 = \mathbf{1500}
\]

WPSP-SP instances.

The different \(I_2\) levels provide project networks ranging from highly parallel to fully serial, allowing the benchmark to capture how network structure affects work-package formation and project performance.

---



## OR-Bench Alignment

Selected problems in this repository are prepared for contribution to **OR-Bench: A Community Benchmark for LLMs on Optimization Modeling**.

The repository is structured to support the three core artifacts required for an OR-Bench-ready problem:

1. a **precise problem statement**, together with associated data or artifacts;
2. a **corresponding mathematical formulation and working solver implementation**; and
3. a **vague, business-level description**, written as a domain expert might initially describe the operational problem.

This repository also supports two complementary modeling settings consistent with the scope of OR-Bench.

### Business-Description-to-Model Evaluation

The model receives only a vague operational description. It must identify the relevant entities, decisions, parameters, objective, and constraints before producing a valid optimization formulation.

### Precise-Specification-to-Model Evaluation

The model receives a detailed problem statement and associated data. It must translate the specification into a mathematically correct and computationally executable optimization model.

For benchmark integrity, a model being evaluated should **not** be given the reference formulation, solver implementation, or reference solution unless those materials are explicitly part of the evaluation protocol.

The OR-Bench project is available at:

`https://github.com/CoraLiang01/OR-Bench`

This repository is an independent research artifact. Preparation or submission of a problem to OR-Bench does not by itself imply acceptance, endorsement, or incorporation into the official OR-Bench release.

---

## Attribution and Citation

Use of this repository requires attribution.

At minimum, publications, reports, datasets, software repositories, course materials, and benchmark studies using these materials should:

1. cite this repository;
2. identify the specific benchmark family and instance IDs used;
3. cite the original publication when an instance is adapted from published work; and
4. indicate any modifications made to the original instance, formulation, or data.

A repository-level BibTeX entry can be provided in the following form and should be updated with the final repository metadata:

```bibtex
@misc{<citation_key>,
  author       = {Yaning ZHANG, Xiao LI},
  title        = {project_planning_and_scheduling_dataset},
  year         = {2026},
  howpublished = {https://github.com/CI3LAB/project_planning_and_scheduling_dataset},
  note         = {V1.0}
}
```

For work-package-sizing instances based on Li and Hall (2019), please also cite:

```bibtex
@article{LiHall2019WorkPackageSizing,
  author  = {Li, Chung-Lun and Hall, Nicholas G.},
  title   = {Work Package Sizing and Project Performance},
  journal = {Operations Research},
  year    = {2019},
  volume  = {67},
  number  = {1},
  pages   = {123--142},
  doi     = {10.1287/opre.2018.1767}
}
```

If an individual instance contains an additional `source_citation`, that source should also be cited.

---

## Licensing

To support open research use while preserving attribution, the repository is intended to use a dual-license structure consistent with the OR-Bench contribution requirements:

- **Problem statements, benchmark data, and associated textual materials:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
- **Source code:** [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0), unless a file explicitly states otherwise.

Under CC BY 4.0, users may share and adapt the problem statements and data, including for research and commercial purposes, provided appropriate credit is given and modifications are indicated.

The Apache License 2.0 permits broad reuse of the code subject to its license conditions, including preservation of required copyright and notice information.

Third-party materials, if any, remain subject to their original licenses. Inclusion of a citation does not automatically grant redistribution rights. Each contributed or adapted benchmark should therefore document its provenance and confirm that the repository has the right to distribute the included material.

---

## Acknowledgment

This repository was developed to support reproducible research in optimization modeling and project operations. Selected problems have also been prepared for contribution to the OR-Bench community benchmark. We thank the operations research and management science community for developing and maintaining open benchmark resources that enable transparent comparison of optimization and AI-based modeling methods.

