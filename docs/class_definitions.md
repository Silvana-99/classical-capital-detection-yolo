# Class definitions and label rules

The bounding box should include the visible column capital (the transition between shaft and entablature), not the entire facade or whole column. Use source annotations as ground truth in the frozen version and keep class IDs in the published YAML order.

| ID | Class | Visual rule |
| --- | --- | --- |
| 0 | Doric capital | Plain square abacus above a rounded or shallow echinus; no prominent volutes or acanthus leaf basket. |
| 1 | Ionic capital | Recognizable spiral volutes (scrolls) on one or both sides of the capital. |
| 2 | Corinthian capital | Tall, richly carved capital with rows of acanthus leaves and usually small volutes. |

Do not label a freestanding scroll, balcony bracket, entablature molding, bare shaft, or an entire building as a capital. For ambiguous or partially occluded examples, flag for human review instead of forcing a class. The source dataset's original labels may differ and should be checked before relabeling additional images.
