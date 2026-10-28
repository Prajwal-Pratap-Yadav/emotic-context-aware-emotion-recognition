# Results

`summary.csv` is a machine-readable transcription of the result values supplied in the project's study summary. It is intended for documentation and table generation, not as a substitute for the original per-seed outputs.

- Scores are mAP percentage points.
- Means and standard deviations are reported across five seeds, as described in the study summary.
- A blank comparison field means no paired difference or test statistic was supplied for that row.
- The summary values have not been independently recomputed from per-seed run files in this repository.
- Resolve the annotation-aggregation discrepancy documented in [the protocol](../docs/experimental_protocol.md) before treating the full evaluation pipeline as protocol-verified.

For a reproducible release, add versioned per-seed results and regenerate this summary programmatically from those records. Avoid publishing test-set artifacts that violate dataset terms.
