# [EACL 2027 Findings] Beyond Free-Form Generation: Measuring Interface Composition in Competitive NLP Systems

**Xinchen Xiao** · Xinjiang University

**Status:** Accepted to **Findings of EACL 2027**.  
**Paper:** ACL Anthology link will be added after publication.  
**Contents:** Final data and a concise annotation guide accompanying the paper.

## Overview

A system that uses a decoder LLM can combine free generation with scoring, ranking, retrieval, or selection. This resource describes those interface combinations in publicly documented NLP competition systems. Each system is annotated in five fields: **task, backbone, output interface, adaptation method, and system method**.

| Corpus | Systems | Competitions or task years |
|---|---:|---:|
| Kaggle | 517 | 42 |
| SemEval, TREC, and WMT shared tasks | 76 | 26 |
| Total | 593 | 68 |

The release contains the final annotations, public source locators, and aggregate statistics. A second annotator independently annotated 179 systems; agreement was measured before adjudication, and resolved labels were incorporated into the final corpus. The annotation process and reliability results are reported in the paper.

Interfaces describe the complete documented pipeline, including intermediate stages. The annotations capture documented systems selected under the paper's screening protocol. Among eligible generation systems with decoder LLMs, 18 of 55 Kaggle systems and 17 of 20 Shared Task systems are hybrid. Giving each competition or task year equal weight yields hybrid shares of 0.327 and 0.876.

## Data files

| File | Rows | Contents |
|---|---:|---|
| [`data/annotations_final_593.csv`](data/annotations_final_593.csv) | 593 | Final system annotations |
| [`data/registry.csv`](data/registry.csv) | 68 | Competition and task year metadata, official URLs, and field coverage |
| [`data/evidence_locators.csv`](data/evidence_locators.csv) | 1,136 | Public source URLs and available locators |
| [`data/entity_map.csv`](data/entity_map.csv) | 611 | Source records mapped to system IDs |
| [`data/aggregate_statistics.csv`](data/aggregate_statistics.csv) | 44 | Reference counts, estimates, and agreement summaries |

[`ANNOTATION_GUIDE.md`](ANNOTATION_GUIDE.md) combines the essential annotation instructions with field and label definitions. [`MANIFEST.csv`](MANIFEST.csv) lists the released files and their SHA-256 checksums.

## Using the data

The CSV files use UTF-8. Start with `data/annotations_final_593.csv`; each row represents one documented system. Label sets use `|` as the separator. A blank label cell accompanies an unknown or inapplicable field. See the annotation guide for field states and derived interface classes.

```python
import csv
from pathlib import Path

path = Path("data/annotations_final_593.csv")
with path.open(encoding="utf-8", newline="") as stream:
    systems = list(csv.DictReader(stream))

interfaces = [
    row["output_interfaces"].split("|")
    for row in systems
    if row["output_status"] == "known"
]
```

Use `source` and `cluster_id` to join the annotations to the registry, and `system_id` to connect them to evidence and source records. Task labels can overlap. The paper's estimates give equal weight to competitions or task years; the Kaggle and Shared Task corpora use separate denominators.

## Citation

If you use the data, please cite the paper:

> Xinchen Xiao. 2027. Beyond Free-Form Generation: Measuring Interface Composition in Competitive NLP Systems. In *Findings of the Association for Computational Linguistics: EACL 2027*. Accepted; to appear.

```bibtex
@inproceedings{xiao-2027-beyond,
  title = {Beyond Free-Form Generation: Measuring Interface Composition in Competitive {NLP} Systems},
  author = {Xiao, Xinchen},
  booktitle = {Findings of the Association for Computational Linguistics: EACL 2027},
  year = {2027},
  publisher = {Association for Computational Linguistics},
  note = {Accepted; to appear}
}
```

The citation will be updated with the ACL Anthology URL, DOI, and page numbers after publication. Machine-readable entries are in [`CITATION.bib`](CITATION.bib) and [`CITATION.cff`](CITATION.cff).

## License

Project annotations, metadata, and documentation are licensed under **CC BY 4.0**. See [`LICENSE`](LICENSE) and [`LICENSES.md`](LICENSES.md). Linked source pages and papers retain their original terms; their text and images are not redistributed or relicensed by this repository.

## Contact

For questions or corrections, open an issue or contact **Xinchen Xiao** at `xiaoxinchenn@outlook.com`. Please identify the file and record ID when reporting a data issue.
