# LLM_Tokenization
Subword Tokenization Methods: BPE, WordPiece, and Unigram
Modern NLP models use subword tokenization to handle unknown words, reduce vocabulary size, and improve efficiency. Three popular methods are Byte-Pair Encoding (BPE), WordPiece, and Unigram.

1. Byte-Pair Encoding (BPE) – Used in GPT models
How it works:

Start with individual characters as tokens.
Example: "complex" → ["c", "o", "m", "p", "l", "e", "x"]
Count the most frequent adjacent character pairs in the dataset.
Suppose "co" appears 500 times, "mp" appears 450 times, etc.
Merge the most frequent pair into a new token.
"co" → ["co", "m", "p", "l", "e", "x"]
Repeat until reaching the vocabulary size limit (e.g., 30,000 tokens).
Example Tokenization:
If "complex" is rare in the training data, BPE might break it down as:
["comp", "lex"]
If "complex" is common, it may be stored as a single token:
["complex"]

Pros & Cons:
✅ Efficient for handling rare words.
✅ Works well with unknown words.
❌ Splits may not always be linguistically meaningful.

2. WordPiece – Used in BERT
How it differs from BPE:

WordPiece uses likelihood-based merges instead of purely frequency-based ones.
It prioritizes high-probability subwords rather than just frequent adjacent pairs.
Steps:

Start with individual characters.
Instead of merging the most frequent pair, compute likelihood scores for different subword candidates based on a statistical model.
Merge the best subword candidates first.
Example Tokenization:

"complex" → ["com", "##plex"]
(## means "plex" is a suffix, not a standalone word).
"running" → ["run", "##ning"]
Pros & Cons:
✅ Reduces vocabulary size while keeping meaningful subwords.
✅ Helps with morphologically rich languages.
❌ More computationally complex than BPE.

3. Unigram – Used in SentencePiece (T5, ALBERT)
How it works:

Instead of merging pairs, Unigram starts with a full vocabulary and gradually removes the least important tokens.
It keeps only the most probable subwords.
Steps:

Train an initial large vocabulary containing many possible subword splits.
Compute probabilities for different segmentations of words.
Drop low-probability subwords and keep only the most useful ones.
Example Tokenization:

"complex" → ["complex"] (if frequent)
"complexity" → ["complex", "ity"] (if rare)
"unbelievable" → ["un", "believ", "able"]
Pros & Cons:
✅ More flexible than BPE/WordPiece.
✅ Supports multiple possible tokenizations and chooses the best one.
❌ Needs more computation during training.

Summary Comparison
Method	Merging Strategy	Example Tokenization for "complex"	Pros	Cons
BPE	Merges most frequent character pairs	["comp", "lex"]	Simple & fast	Splits may not be meaningful
WordPiece	Merges high-probability subwords	["com", "##plex"]	More meaningful subwords	Higher complexity
Unigram	Removes low-probability subwords	["complex"] or ["comp", "lex"]	Most flexible	More computation needed
Each method is designed to balance efficiency and linguistic meaning. Transformer models (GPT, BERT, T5, etc.) choose tokenizers based on their specific needs!
