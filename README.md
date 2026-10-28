# Context-Aware Emotion Recognition on EMOTIC

Research notebooks for context-aware, multilabel emotion recognition on the **EMOTIC** dataset. The experiments study visual/person-level inputs, data augmentation, Grounded Emotion Queries (GEQ), alternative multilabel heads, and parameter-efficient adaptation using an InstructBLIP-based architecture.

> **Reproducibility note:** the notebooks are the primary experiment records. Before treating the results as independently reproducible, reconcile the annotation-aggregation discrepancy documented in [the experimental protocol](docs/experimental_protocol.md), and record the exact runtime/package versions used for the final runs.

## Research questions

1. Do person crops, region information, and bounding-box geometry improve the baseline?
2. Does label-preserving geometric/color augmentation improve validation-selected test performance?
3. Do emotion-definition-conditioned Grounded Emotion Queries improve multilabel prediction?
4. Can LoRA preserve performance while substantially reducing the number of trainable parameters?

## Repository contents

| File | Purpose |
|---|---|
| [Phase A — baseline, no augmentation](emotic-phasea-v7-without-aug.ipynb) | Baseline reproduction, annotation/preprocessing checks, visual-feature inputs, and seed-based comparisons |
| [Phase B — augmentation](emotic-phaseb-augmentation.ipynb) | Longer-schedule and augmentation comparisons, test-time mirroring, and partial encoder fine-tuning |
| [Phase C — GEQ](emotic-phasec-geq.ipynb) | Grounded Emotion Queries, alternative multilabel heads, ablations, and LoRA experiments |
| [Experimental protocol](docs/experimental_protocol.md) | Evaluation design, interpretation, and reproducibility checks |
| [Reported result summary](results/summary.csv) | Machine-readable summary of the results reported for this study |
| [Results notes](results/README.md) | Metric definitions and caveats |
| [Citation metadata](CITATION.cff) | Repository citation metadata |

The notebooks are currently kept at the repository root so their existing paths and execution records are not changed during initial cleanup.

## Method at a glance

The shared approach combines visual context from **InstructBLIP's EVA-ViT-g encoder and Q-Former** with contextual image descriptions generated using **LLaVA**. Phase C adds **Grounded Emotion Queries (GEQ)**: 26 emotion-specific queries initialized from EMOTIC category definitions, an asymmetric attention mask intended to preserve the baseline pathway, and classifiers generated from encoded category definitions. LoRA experiments evaluate parameter-efficient adaptation of a frozen Q-Former.

## Reported results

The table below transcribes the results supplied in the study summary. Values are **mean mAP ± standard deviation across five seeds**, expressed in mAP percentage points. They are not a claim that this repository has independently rerun every experiment.

| Phase | Configuration | Mean mAP (%) | SD | Interpretation |
|---|---|---:|---:|---|
| A | Reproduced baseline | 38.51 | 0.07 | Baseline reproduction; 38.52 was the cited reported value |
| A | Additional person/region/geometry inputs | 38.70 | 0.09 | Higher than baseline in all five paired seeds |
| B | Augmentation | 38.77 | 0.24 | Matched-control comparison below |
| B | Matched control | 38.87 | 0.22 | Reference for augmentation comparison |
| B | Partial encoder fine-tuning | 38.48 | 0.23 | Did not improve over the corresponding reference |
| C | Baseline | 38.85 | 0.15 | GEQ comparison baseline |
| C | GEQ | 38.87 | 0.08 | Difference of +0.02 mAP points; 95% CI: −0.15 to +0.18 |
| C | LoRA | 38.96 | 0.11 | Parameter-efficient adaptation |
| C | GEQ + LoRA | 39.10 | 0.23 | Reported non-inferior to the fully fine-tuned baseline under a 0.30-point margin |

### Statistical takeaways

- **Augmentation:** reported difference versus matched control was −0.10 mAP points (paired test, *p* = 0.163); this does not establish an improvement.
- **GEQ alone:** the reported +0.02-point difference has a 95% confidence interval spanning zero, so it does not establish a gain.
- **GEQ + LoRA:** reported difference versus the Phase C fully fine-tuned baseline was +0.25 points (95% CI: −0.05 to +0.56), assessed with a prespecified non-inferiority margin of 0.30 mAP points. Interpret this claim using the exact analysis and protocol in the final manuscript.
- **External comparison:** the study summary cites 39.13 mAP for EmotiCon with a contextual causal intervention module (CCIM). Verify and cite the source paper before using this comparison in a publication; differences in data and evaluation protocols can limit direct comparability.

A CSV transcription is available at [results/summary.csv](results/summary.csv).

## Running the notebooks

These notebooks were developed for a GPU notebook workflow and may rely on environment-specific paths, cached features/descriptions, dataset mounts, and pretrained model files. In particular, Phase A contains references to Kaggle-style `/kaggle/input` and `/kaggle/working` paths.

1. Open the notebook in an environment with compatible GPU memory and the required packages.
2. Obtain EMOTIC and pretrained model assets through their authorized sources; configure paths to match the selected runtime.
3. Inspect the first setup cells before running downstream cells. Confirm that dataset paths, cached descriptions, split files, and checkpoint locations point to the intended artifacts.
4. Run the phases and seeds using the protocol and configuration recorded for the experiment.
5. Preserve per-seed metrics and logs, then regenerate aggregate tables from those files rather than copying aggregate values by hand.

**Environment capture is still required for a fully reproducible release.** Record the Python, PyTorch, Transformers, CUDA/driver, GPU, and other dependency versions from the environment that actually produced the reported results. Do not install arbitrary latest package versions and assume they match the original runs.

## Evaluation safeguards

- Select configurations using validation data only; reserve the official test split for final evaluation.
- Keep all seeds and comparisons paired as specified by the experiment.
- Report per-seed results alongside mean, standard deviation, and confidence intervals.
- Do not tune thresholds, hyperparameters, or model selection using official test results.
- Keep the annotation aggregation rule consistent across the manuscript, preprocessing code, cached labels, and reported metrics.

See [docs/experimental_protocol.md](docs/experimental_protocol.md) for the protocol checklist and an outstanding annotation-aggregation issue.

## Data, checkpoints, and licenses

This repository does not intentionally redistribute the EMOTIC dataset or pretrained model weights. Obtain them from their authorized sources and follow their access terms and licenses. Do not commit API keys, access tokens, personal data, raw datasets, or large model checkpoints. The MIT license in this repository applies to original repository code only; third-party components and data retain their own terms.

## Citation

If you use this repository, cite it using [CITATION.cff](CITATION.cff). Add the final paper citation, DOI, or preprint URL when the manuscript is publicly available. Do not cite this repository as a published paper unless and until it is published.

## License

Original code in this repository is released under the [MIT License](LICENSE), subject to institutional requirements and the licenses of third-party materials.
