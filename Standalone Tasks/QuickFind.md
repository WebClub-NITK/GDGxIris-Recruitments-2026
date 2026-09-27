# Task ID: QuickFind

#### `Data Structures & Algorithms`, `Search`, `Performance`

Mentors: TBD

Difficulty: `Medium`

---

## 📌 Project Overview: What is QuickFind?

**QuickFind** is an autocomplete/search engine you build entirely from scratch — no Fuse.js, Lunr, Elasticlunr, Algolia, or any search/fuzzy-matching library. Given a dataset (e.g. a list of ~50,000+ words, or a set of movie/book titles with descriptions), your engine must return ranked, relevant suggestions as the user types, including tolerance for typos.

The appeal of this task for evaluation is that "make an input box that filters an array with `.includes()`" technically works on a tiny dataset and completely falls over — visibly, in front of an interviewer — the moment the dataset gets large or the user makes a typo. There's no way to fake the data-structure and ranking work.

---

## 🎨 What Should the Final Output Look and Act Like?

1. **The Search Box:**
   - A single input field. As the user types, a dropdown of suggestions updates live (no submit button, no full page reload).
   - Suggestions should appear near-instantly even on a dataset of tens of thousands of entries — no visible lag per keystroke.

2. **The Suggestions List:**
   - Ranked, not just alphabetical — an exact prefix match should generally outrank a fuzzy/typo match, and more "popular"/shorter/closer matches should rank above distant ones (you define and justify your ranking function).
   - Must tolerate small typos (e.g. typing `stawberry` should still surface `strawberry`).

3. **A Benchmark Panel:**
   - A way to demonstrate query time (e.g. show the time taken, in ms, for the last query) at increasing dataset sizes (1k, 10k, 50k+ entries), so performance claims are visible, not just asserted.

---

## 🛠️ Core Features to Implement (Mandatory)

### 1. Indexing Structure (built by hand)
Implement **at least one** proper indexing structure — not a linear scan over the raw array on every keystroke:
- A **Trie** (prefix tree) for fast prefix lookups, or
- An **inverted index** (token → list of documents containing it) if working with multi-word entries like titles/descriptions.

### 2. Fuzzy Matching for Typo Tolerance
- Implement edit distance (Levenshtein, or a simpler bounded variant) from scratch to catch near-misses within a small edit-distance threshold.
- Naively computing full Levenshtein distance against every entry in a 50k-word dataset per keystroke will be visibly slow — you're expected to bound the search sensibly (e.g. only compute distance against candidates that share a prefix or first character, not the entire dataset).

### 3. Ranking
- Suggestions must be ordered by a ranking function you design and can justify — combining factors like: exact-prefix vs fuzzy match, edit distance, match length/position, and (optionally) a static popularity/frequency score if your dataset has one.
- Ties should be broken deterministically (e.g. alphabetically), not by insertion order accident.

### 4. Performance Under Load
- Must remain responsive (sub-~50ms perceived lag per keystroke is a reasonable target) on a dataset of at least 50,000 entries. If you can't hit this with a naive approach, that's the point — the indexing structure is what gets you there.
- Debounce input appropriately so you're not re-querying on every single keystroke if the user is typing fast — but the underlying query itself must still be fast on a single call.

---

## ⚙️ Technical Specifications & Implementation Guidelines

- **No shortcuts:** the trie/index, the edit-distance function, and the ranking logic must be your own code. You may use standard language data structures (arrays, maps/objects) as building blocks.
- **Big-O awareness:** be ready to state the time complexity of a prefix lookup in your trie vs. a linear scan, and of your bounded edit-distance search vs. naive full Levenshtein-against-everything.
- **Memory trade-off:** a trie/inverted index trades memory for speed — be able to roughly estimate how index size scales with dataset size, and what you'd do differently at, say, 10 million entries (you don't need to implement this, just reason about it).
- **Update handling:** briefly consider (doesn't need full implementation) what happens if entries are added/removed after the index is built — does your structure support incremental updates, or does it require a full rebuild?

---

## 🚀 Bonus Features (Optional)

1. **Multi-word / Phrase Search:** support queries matching across multiple tokens in an entry (e.g. searching `dark knight` should find "The Dark Knight" even with a word in between or out of order), using your inverted index.
2. **Highlighting:** highlight the matched portion(s) of each suggestion in the UI, including for fuzzy matches (not just exact substring matches).
3. **Real Dataset + Scale Test:** run against a genuinely large real-world dataset (e.g. a dump of Wikipedia titles, ~100k+ entries) and report actual benchmark numbers, not synthetic ones.

---

## 💡 Practical Technical Tips & Real-World Use Cases

- **Why are we building this?** This is the exact problem behind every search-as-you-type box you use daily — IDE autocomplete, search engine query suggestions, command palettes (VS Code's Ctrl+Shift+P), app search bars. The trie/index + ranking combo here is the same core idea at every scale, just with more infrastructure on top in production.
- **Interview Insight:** Be ready to explain, precisely, what happens when you query your trie for a prefix that doesn't exist at all — how many nodes does your code actually visit before giving up? And be ready to justify a specific ranking decision on a real example from your dataset ("why did X rank above Y for this query").

---

## 📚 Useful Technical Resources

- [Wikipedia: Trie](https://en.wikipedia.org/wiki/Trie)
- [Wikipedia: Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)
- [Wikipedia: Inverted index](https://en.wikipedia.org/wiki/Inverted_index)
- [Bounded edit-distance search over a trie (BK-tree alternative approach, for comparison)](https://en.wikipedia.org/wiki/BK-tree)