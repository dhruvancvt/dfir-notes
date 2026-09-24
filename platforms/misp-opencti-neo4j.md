# MISP vs OpenCTI (+ Neo4j visualization)

| | MISP | OpenCTI |
|--|------|---------|
| Model | Events → attributes / objects | STIX 2.1 knowledge graph |
| Strength | IOC sharing and operations, feeds, correlation | Relationships, ATT&CK-native, analysis |
| Best for | "Is this hash/IP known?" | "How do these actors, campaigns and techniques connect?" |

## MISP → Neo4j
- Research references: STIG (STIX graph), the CRUSOE data model.
- Rendering tiers: Neo4j **Bloom** (analyst UI) → **sigma.js** (web, mid-scale)
  → **Graphistry** (GPU, large graphs).
