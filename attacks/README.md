# Attacks

Text is discrete, so neither attack here can walk a continuous gradient the way FGSM does on a pixel grid; each instead solves its own combinatorial search problem over words or tokens. `whitebox/` assumes access to the model's gradients with respect to its input embeddings; `blackbox/` assumes only query access to its output probabilities, the more realistic assumption against a deployed API.

## whitebox/

`01_HotFlip` implements Ebrahimi et al.'s 2018 gradient-guided word substitution: a first-order Taylor approximation of how much replacing a given token would increase the loss, computed as a single matrix-vector product against the whole embedding matrix rather than by trying every candidate word one at a time. It flips the best (position, word) pair greedily, repeating until the prediction changes or a flip budget is exhausted. Against DistilBERT-SST2, 2 to 3 flips are usually enough, sometimes landing on a nonsensical BERT subtoken, an artifact of operating on subword vocabulary rather than whole words.

## blackbox/

`01_SynonymSubstitution` never touches a gradient: it first ranks words by importance (how much removing each one drops the true class's probability), then walks that ranking substituting WordNet synonyms of matching part of speech, keeping whichever candidate hurts the original class most, until the prediction flips or the substitution budget runs out. It succeeds far less often than HotFlip and costs dozens of queries per sentence, the explicit price of not having gradient access; WordNet's synonyms are also not filtered for fluency, so successful attacks often read as grammatically valid but semantically odd.

## A note on what these attacks do not optimize for

Neither attack here enforces a fluency or semantic-similarity constraint the way a full TextFooler or PWWS implementation would. That is a known, intentional simplification, not an oversight, and is the natural place to extend this folder if a stealthier, more natural-reading attack becomes the goal.
