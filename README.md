# KestrelAnalytics

**Graph and population analytics for research on people.** KestrelAnalytics runs inside the [AquilaDB](https://github.com/FranciscoSerranoBaptista/AquilaDB) server. It finds structure in a research graph and patterns across a population of persons. It says "not yet" when the data cannot carry a claim.

This repository is the public face of the project: the description and the public issues. The source code is private.

## Heritage

The API follows the shape of the [ArangoDB Graph Analytics Engine](https://docs.arangodb.com): load a graph, run an algorithm as a job, store the result on the vertices. KestrelAnalytics keeps that shape and adds what research on people needs: stability across runs, lineage over time, statistics on tables, a consent gate and a cohort floor on each output. Like AquilaDB, it contains no ArangoDB source code.

## What it computes

**On a graph:**

- **Centrality:** PageRank (with a seed attribute), betweenness, closeness, degree, LineRank.
- **Components and communities:** weakly and strongly connected components, label and attribute propagation, Louvain, and Leiden, where every community is connected.
- **Consensus and stability:** a community algorithm runs many times. Each vertex gets a stability value. A pattern that does not hold across runs is reported as unstable, not as a finding.
- **Lineage:** communities are tracked across runs and time windows, with ten event kinds (birth, growth, merge, split and others).
- **Partition comparison:** two runs compared member by member.
- **Embedding map:** a PCA or UMAP projection to 2-D, with a trust value for each vertex.

**On a table of persons and sessions:**

- `describe`, `frequencies`, `correlate` (Spearman, Kendall), `factor` (loadings, Cronbach's alpha).
- `variance` (ICC and random-intercept mixed models, REML or ML) and `trend` (random-slope models over time).
- `profiles` (latent profiles by Gaussian mixtures). **Kestrel never picks the number of profiles for you.**
- Bootstrap intervals that resample persons, not rows.

Each statistics method is checked against numpy, scipy, statsmodels and scikit-learn.

## What makes it different

| Concern | A general-purpose analytics library | KestrelAnalytics |
|---|---|---|
| **Small groups** | The analyst must remember to hide small cells. | Each output on clinical data passes a cohort floor. Small levels and cells are pooled or suppressed. |
| **Consent** | Not modelled. | A person without consent is removed before the analysis starts. Their data shapes no community, embedding, model or fact. The gate fails closed. |
| **Unstable patterns** | A community appears on each run. | Consensus runs measure stability. An unstable community is not a finding. |
| **Change over time** | One snapshot. | Scheduled recipes run over time windows and link the windows by lineage. |
| **Fishing in the data** | Any caller can run tests until a result looks good. | A study is **pre-registered**. The plan is hashed before any evidence exists. An admin approves that hash, the server signs the approval, and the study runs once. |
| **An agent as analyst** | The agent gets the rows. | A `researcher` key reaches only the research endpoints and only the approved studies. It sees results and their hashes, never the rows. |

## The two kinds of data

- **The research corpus** (works, chapters, ideas, concepts, traditions) is one connected graph. Centrality, communities, lineage and the embedding map apply here.
- **Clinical data** is analysed through tables, time windows and trajectories, under the floor and the consent gate.

## Where it is going

The next work supports the research console in [PeacockUI](https://github.com/FranciscoSerranoBaptista/PeacockUI). Most of it exposes computations that already exist:

- **Endpoints for k-core, bridges, articulation points, link prediction and Katz.** These run today only inside a survey.
- **Named communities:** a term label for each community, stored with the run.
- **A structural-gap matrix:** for each pair of communities, the distance in meaning against the links that join them. The result is a ranked list of gaps and of candidate bridges.
- **Ideas by decade:** communities per time window on the year of each work, linked by lineage.
- **A validated projection:** idea-to-idea links kept only when they pass a hypergeometric test, so that hubs do not join every community.
- **Burst detection:** which topics rise fast in a window.
- **Extraction checks:** near-star hubs that a language model created by summary, listed for review and not ranked as influential.

## The stack

| Part | Role |
|---|---|
| **[AquilaDB](https://github.com/FranciscoSerranoBaptista/AquilaDB)** | The engine: the records, their relations over time, and the privacy rules. |
| **KestrelAnalytics** | Graph and population analytics: communities with stability, centrality, statistics, pre-registered studies. |
| **[PeacockUI](https://github.com/FranciscoSerranoBaptista/PeacockUI)** | The research console: start from a question, read ranked answers, keep the question with its finding. |

## Status

Ships with AquilaDB `v1.1.1`, the latest Aquila release (2026-10-12).

## Issues

Use the issues here for questions, ideas and collaboration requests. The working backlog is private.

**Do not post personal, clinical or coaching data in an issue.** An issue is public. To report a vulnerability, see [SECURITY.md](SECURITY.md).

© 2026 Francisco Serrano Baptista. All rights reserved.
