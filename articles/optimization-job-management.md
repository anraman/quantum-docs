---
author: KittyYeungQ
description: This document provides a basic guide to managing jobs submitted for solving optimization problems in Azure Quantum using Python.
ms.author: kitty
ms.date: 02/01/2021
ms.service: azure-quantum
ms.subservice: optimization
ms.topic: how-to
title: Manage optimization jobs
uid: microsoft.quantum.optimization.job-management
---

# Job management

When a problem is submitted to a solver, a `Job` is created in Azure Quantum. The `Workspace` provides the following methods for managing jobs:

- **get_job**: Returns the `Job` metadata and results for a specific job
    (based on job `id`).
- **list_jobs**: Returns a list of all jobs in the workspace.
- **cancel_job**: Cancels a specific job.

See [Job Cancellation](xref:microsoft.quantum.azure-quantum-overview#job-cancellation) for more information on how cancellation requests are processed.

You can use the `list_jobs` method to get a list of all jobs in the workspace:

```py
jobs = [job.id for job in workspace.list_jobs()]
print(jobs)

> ['5d2f9cd70f55f149e3ed3aef', '23as12fs5d2f9cd70f55f', '1644428ea8507edb7361']
```

This shows how to submit a job asynchronously and call `get_job` to get the metadata (and results) for a previously submitted job, by `id`: 

```py
from azure.quantum.optimization import Problem, ProblemType, Term, ParallelTempering, SimulatedAnnealing

problem = Problem(name="MyOptimizationJob", problem_type=ProblemType.ising)
problem.add_term(c=-9, indices=[0])
problem.add_term(c=-3, indices=[1,0])
problem.add_term(c=5, indices=[2,0])

solver = SimulatedAnnealing(workspace)
job = solver.submit(problem)
print(job.id)

> 5d2f9cd70f55f149e3ed3aef

job = workspace.get_job(job.id)
results = job.get_results()
print(results)

> {'configuration': {'0': 1, '1': 1, '2': -1}, 'cost': -17.0}
```