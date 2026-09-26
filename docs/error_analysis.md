# Error analysis

Validation reference: `results/evidence/validation_predictions/val_batch0_pred.jpg` compared with `results/evidence/annotation_examples/val_batch0_labels.jpg`. These are representative visual observations; the suggested causes are hypotheses. At confidence 0.25, the validation confusion matrix also shows predicted-background errors across all three classes, but it is not a substitute for individual review.

## Three false-positive observations in the validation montage

1. **Decorative scroll classified as Ionic** — top-left tile of `val_batch0_pred.jpg`: a standalone scroll-shaped architectural ornament gets an Ionic box, while the corresponding label montage has none. Likely cue: the spiral resembles an Ionic volute despite lacking a full column capital.
2. **Tiny Doric box on a balcony detail** — bottom-left tile of `val_batch0_pred.jpg`: a small architectural corner is boxed as Doric; the matching label tile has no capital. Likely cue: a projecting horizontal molding above a narrow support looks like a simplified abacus.
3. **Extra Doric box on one capital** — third row, rightmost tile of `val_batch0_pred.jpg`: two overlapping Doric predictions appear around a single annotated capital. One is an extra detection, likely caused by nested visual edges near the abacus and echinus. Review the exact matching criterion if counting this as an FP in a formal evaluation.

## Three false negatives on external photographs

1. `new_01_doric.jpg` — a weathered, detached stone Doric capital, seen at an unusual orientation, produced no detection. Likely cause: erosion and lack of a clear upright shaft.
2. `new_02_doric.jpg` — a close-up museum specimen of a damaged Doric capital produced no detection. Likely cause: detached artifact context and altered proportions/texture.
3. `new_03_ionic.jpg` — a large frontal Ionic capital with scrolls produced no detection. Likely cause: much tighter framing and different carving style than the training examples; exact cause is unknown without saliency or additional controlled tests.

At confidence 0.10, the same three external photographs still had no detection. Two other external examples were correctly labelled: Ionic `new_04` at confidence 0.44 and Corinthian `new_05` at 0.95. These five cases are an illustrative domain-shift check, not a representative performance estimate.

## Prioritized improvements

1. Add and annotate at least 30 licensed, weathered/detached Doric capitals across multiple sites and orientations; reserve sites unseen during training for evaluation.
2. Add 20–30 close-up Ionic capitals, including atypical scrolls, and about 30 hard negatives such as brackets, balcony supports, scroll ornaments and moldings.
3. Split by building/site (not merely by image), audit near-duplicate scenes and ambiguous labels, retrain for >=30 epochs, then compare class-wise recall and external-image outcomes before claiming improvement.
