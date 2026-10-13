# KestrelAnalytics

**The business question:** which patterns across our leaders are real enough to change a programme, a standard or an investment, and which are noise?

A leadership function can see a lot of data and still decide on anecdote. The risk is the opposite as well: a dashboard that finds a pattern in every small group. KestrelAnalytics is the analytics engine behind [AquilaDB](https://github.com/FranciscoSerranoBaptista/AquilaDB). It looks for patterns across the whole leader population, and it says "not yet" when the data cannot carry a claim.

This repository holds a description, screenshots and public issues. The source code is private.

## What a leadership function gets

- **Patterns across the population.** Communities of themes, the leaders and topics that connect them, and how they move over time.
- **Stability before action.** A pattern must hold across repeated runs before it counts. A pattern that changes with every run is reported as unstable, not as a finding.
- **Comparisons that respect the floor.** Groups can be compared, for example by level or programme, only when each group is large enough. Small cells are suppressed.
- **A record of every question.** Each research run is planned, approved and stored with its lineage, so a result can be traced back to the question that produced it.
- **An honest limit.** When the data is too thin, the answer is "not enough data yet". That is the limit of what the system can say today, not a finding.

## How it works

KestrelAnalytics runs inside the AquilaDB server. It loads a graph into memory and runs analytics as jobs: PageRank, connected components, betweenness, Louvain and Leiden communities, consensus and stability across runs, and comparison between runs. Results are stored as runs with lineage. The API follows the shape of the ArangoDB Graph Analytics Engine.

## The stack

| Part | The business question it answers |
|---|---|
| **[AquilaDB](https://github.com/FranciscoSerranoBaptista/AquilaDB)** | Where do the records of development live, safely, over time? |
| **KestrelAnalytics** | What patterns hold across the population, and are they stable enough to act on? |
| **[PeacockUI](https://github.com/FranciscoSerranoBaptista/PeacockUI)** | Can a leadership team see those patterns and decide what to do next? |

## Screenshots

<!-- add images to docs/screenshots/ -->

## Issues

Use the issues here for questions, ideas and collaboration requests. The working backlog is private.

© 2026 Francisco Serrano Baptista. All rights reserved.
