# Food–Microbe–Disease Knowledge Graph (KG)

This repository provides the source data and reproducible instructions for constructing a Food–Microbe–Disease knowledge graph in Neo4j (Aura or self-hosted), along with an optional NeoDash dashboard configuration for visualization.

## Contents

- `D-M-GSC-KG.csv`  
  Disease–Microbe associations extracted/curated from GSC.  
  **Required columns**: `DISEASE`, `MICROBE`, `RELATION`  
  **Optional columns** (can be kept or removed): `EVIDENCE`, `QUESTIONS`

- `F-M-GSC-KG.csv`  
  Food/Dietary–Microbe associations extracted/curated from GSC.  
  **Required columns**: `DIETARY`, `MICROBE`, `RELATION`

- (Optional) `neodash_dashboard.json`  
  A NeoDash dashboard configuration file (if you export it and add it here).

## Data model

Nodes:
- `(:Disease {DISEASE})`
- `(:Dietary {DIETARY})`
- `(:Microbe {MICROBE})`

Relationships:
- `(:Disease)-[:ASSOCIATED_WITH {RELATION}]->(:Microbe)`
- `(:Dietary)-[:ASSOCIATED_WITH {RELATION}]->(:Microbe)`

Where `RELATION ∈ {positive, negative, relate, NA}`.

Microbe nodes are **merged across datasets** using the `MICROBE` property as the unique identifier.

## Quick start (Neo4j Aura / Neo4j 5+)

### 1) Create constraints (recommended)
Run the following in Neo4j Browser / Aura Query:

```cypher
CREATE CONSTRAINT disease_unique IF NOT EXISTS
FOR (d:Disease) REQUIRE d.DISEASE IS UNIQUE;

CREATE CONSTRAINT dietary_unique IF NOT EXISTS
FOR (f:Dietary) REQUIRE f.DIETARY IS UNIQUE;

CREATE CONSTRAINT microbe_unique IF NOT EXISTS
FOR (m:Microbe) REQUIRE m.MICROBE IS UNIQUE;
