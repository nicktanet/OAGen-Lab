# OAGen: Object Allocation Graph Generation for Android Forensics

OAGen is a post-execution and app-agnostic semantic analysis framework for Android designed to help investigators establish concrete evidence by identifying the provenance and relationships between in-memory objects in a process memory image.

OAGen utilizes Points-to analysis to reconstruct an Android Runtime (ART) object allocation network. The resulting graph is then fed as an input into our semantic analysis algorithms to determine an object's origin, context, and scope within the network footprint.

---

## Key Modernization Accomplishments

### 1. Python 3 Codebase Migration & Dependency Management
- **Syntax Modernization:** Audited and refactored legacy diagnostics core utilities (`artFlowGraph.py`, `artProj.py`, and `utils.py`), eliminating deprecated language patterns and standardizing package structures under unified Python 3 parameters.
- **Dynamic Path Optimization:** Standardized execution environment behaviors inside localized virtual environments (`venv`), replacing error-prone static paths with stable environment controls.
- **Package Standardization:** Re-architected repository system imports for modern graph structures (`pygraphviz`, `networkx`), resolving severe native module extraction errors (such as missing `_graphviz` sub-module bindings) during clean verification blocks.

### 2. Runtime & JIT Engine Performance Tuning
- **Functional Optimization:** Eliminated heavy, sequential conditional loops within the references lookup process (`jvm2.dumpRefs`), substituting high-speed **lambda functions** to isolate memory bottlenecks.
- **JIT Compiler Integration:** Configured and validated codebase operations to execute seamlessly via the **PyPy JIT compilation engine**, substantially accelerating generation overhead when rendering large-scale layout matrices.

### 3. Architecture Scaling & Memory Alignment Fixes
- **64-Bit Compatibility:** Overhauled utility configurations to eliminate static structures hard-coded for older 32-bit spaces. Refactored the core pointer-reading infrastructure (`readPointer` inside `artHeap.py`) to properly accommodate memory addresses across both 4-byte (32-bit) and 8-byte (64-bit) target systems libraries (such as modern versions of mobile `libart.so`).
- **Codec & String Decoding Resiliency:** Debugged object traversal paths (`decodeObject → hp.getObject → jvm2.dumpRefs`) to eliminate execution-breaking `UnicodeDecodeError` blocks. Created safe algorithmic fallback loops to capture non-ASCII character byte strings within raw RAM arrays, forcing reliable hex-splitting (`:`) when application field strings hit byte alignment layout anomalies.

---

## Dependencies & System Requirements

### Framework Dependencies
- **PyPy Engine:** [pypy.org](https://www.pypy.org) (Just-in-Time compilation accelerator)
- **pygraphviz:** [pygraphviz.github.io](https://pygraphviz.github.io) (Python interface to Graphviz data structures)
- **networkx:** [networkx.github.io](https://networkx.github.io) (Complex network manipulation package)

### Environment Prerequisites
- **ProcMem:** Extract per-process memory from an Android device or emulator environment using **Memfetch**. *(Note: This framework has been explicitly evaluated on Android 8.0.0).*
- **Heapdump:** Dump raw heap allocations using the **DroidScraper** utility engine (`git@github.com:apphackuno/DroidScraper.git`).

> **Note:** Request baseline test images and corresponding heapdump targets by emailing `apphackuno@gmail.com`.

---

## Execution & Processing Matrix

### OAG Graph Generation
To generate a comprehensive object allocation graph for a specific memory target image, execute the following runtime string:

```bash
pypy artFlowGraph.py Graph [path-to-ProcMem] [path-to-Heapdump] [graph-out-file.dot]
```

**Practical Testing Example:**
```bash
pypy artFlowGraph.py Graph /ProcMem/org.thoughtcrime.securesms/ /HeapDumps/signal.out signal.dot
```

### Strings Utility Analysis
For execution-wide file extraction or target keyword parsing, invoke the text matching strings layout:

- **General String Search Loop:**
  ```bash
  pypy artFlowGraph.py Strings whatApp2AllObj.dot
  ```
- **Targeted Expression Query Matching:**
  ```bash
  pypy artFlowGraph.py Strings whatApp2AllObj.dot "Tree of Life, City of Bridges"
  ```

### Context Determination & Subgraph Extraction
To isolate and examine the specific allocation context for a target hex pointer address (e.g., `0x136b4078`), execute the target evaluation loop:

1. **Output Nodelist Subgraph Array Only:**
   ```bash
   pypy artFlowGraph.py Context whatApp2AllObj.dot 0x136b4078 -d 50
   ```
2. **Plot and Visualize the Complete Structural Subgraph:**
   ```bash
   pypy artFlowGraph.py Context whatApp2AllObj.dot 0x136b4078 -d 50 Plot
   ```

***Processing Notice:** Due to structural updates introduced via code review, efficiency optimizations, and variations in system CPU or volatile memory configurations, output results may scale differently regarding the final size of the OAG (total number of nodes and edges) as well as global parsing execution times.*

---

## Research Impact & Background
The optimization and functional validation of these forensic code-recovery assets directly empowered the underlying data-collection mechanics used in the platform-wide privacy analysis of mobile browser runtime security. This collaborative research won a **Distinguished Paper Award** in **IEEE (10/2024)**.
