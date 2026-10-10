# LWords dictionary & level data

Word lists and puzzle levels used by **LWords**, a word puzzle game for Garmin watches
(Connect IQ). This repository exists so that the data is openly available under its licence,
**CC BY-SA 4.0** (see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE)). The game's source code is
not part of this repository.

## Contents

```
levels/en/levels_en_1.json … levels_en_4.json   English levels (4 tiers × 15)
levels/cs/levels_cs_1.json … levels_cs_4.json   Czech levels (4 tiers × 15)
wordlists/en/words.txt                          derived English word list (6 742 nouns)
wordlists/cs/words.txt                          derived Czech word list (10 161 nouns)
fonts/OFL.txt                                   licence of the Rubik font used in the app
CREDITS.md                                      full credits of the app
PRIVACY.md                                      privacy statement of the app
LICENSE                                         CC BY-SA 4.0 legal code
NOTICE                                          source credits for the data
```

## Level format

Each `levels_<lang>_<tier>.json` is a JSON array of 15 levels. Tier 1–4 = 5 / 6 / 7 / 8 letters.
A level is an array of five items:

```json
[ ["O","Í","T","S","M"], ["MÍT","MÍSTO","STO","OSM","MOST","SÍTO","OSÍT"], 60, 5, 3 ]
```

1. **letters** – one string per tile, upper case; Czech letters with diacritics are separate
   letters (Č ≠ C). For 5–7 letters all tiles lie on the wheel, clockwise from 12 o'clock. For
   8 letters, 7 tiles lie on the wheel and the **last item is the centre tile**.
2. **words** – every word of `wordlists/<lang>/words.txt` (base forms, offensive words
   removed) that can be built from the letters (each tile at most once), at least minLen letters, sorted
   from the most frequent (words added by the committee, see below, come last). The game accepts any of them. At least one word uses all letters.
3. **time limit** in seconds for the TIME mode (20 s + 8 s per needed word).
4. **need** – how many words (any of the list) complete the level; it equals the number of
   common words of the level (rank below the tier's vocabulary cut-off: 2000 / 2500 / 3000 /
   3500), so the difficulty does not grow with rare words. Added in game version 0.5.0.
5. **minLen** – the shortest accepted word: 3 for 5- and 6-letter levels, 4 for 7- and
   8-letter levels (all listed words respect it). Added in game version 0.8.0.

## Word list format

`words.txt`: one word per line, upper case, UTF-8 (NFC), common nouns in the base form only
(nominative singular; plural-only nouns such as DVEŘE in the nominative plural), 3–8 letters, sorted from the most frequent (line number =
rank; the few committee-approved additions, see below, are appended at the end). Proper nouns and abbreviations are removed, and so are the words on the game's
private blocklist of offensive words - this is exactly the vocabulary the level generator uses.

## How the data is made

- **English**: noun headwords of 12dicts *2of12id* (lower case only, no abbreviations from
  *abbr*), intersected with *3esl* and *ENABLE*, checked against the noun share of the word in
  the UD English-EWT treebank; ranked by wordfreq frequency (headword plus its
  inflections).
- **Czech**: noun lemmas from the Universal Dependencies treebanks Czech-CAC, Czech-CLTT and
  Czech-PUD (release 2.18) together with the Czech common nouns of the English Wiktionary
  (kaikki.org extract; archaic, rare, dialectal, slang, vulgar and similar senses do not
  count); proper nouns, abbreviations, foreign words and typos removed; ranked by the wordfreq
  frequency of the word forms of each lemma (forms shared with other words are split).
- **Levels**: a deterministic generator picks a seed word of N letters from the most frequent
  words, checks the number of common words that can be built from its letters, avoids
  repeated letter sets, orders levels by difficulty and shuffles the wheel so that no word of
  the level can be read around it. The level then lists **all** words of the word list
  buildable from its letters, so the player is never refused a dictionary word.

- **Committee supplement**: for each level, AI agents (Claude Haiku and Sonnet) proposed further
  nouns buildable from its letters; the generator verified that they are buildable, and a
  committee of three AI reviewers plus a coordinator approved them (EN 91, CS 20 words). They are our own selection from general vocabulary, not copied from any other
  source, are appended at the end of `words.txt`, are accepted wherever they can be built, and do
  not influence which letters are chosen or the `need` value.

## Licence & attribution

The level files and the word lists in `wordlists/` are an adaptation (selected, filtered,
ranked, combined) of wordfreq data by Robyn Speer et al., the Universal Dependencies Czech and
English-EWT treebanks and Wiktionary (via kaikki.org), all CC BY-SA 4.0, and of the public-domain 12dicts and ENABLE lists. They are
licensed under the **Creative Commons Attribution-ShareAlike 4.0 International** licence
(https://creativecommons.org/licenses/by-sa/4.0/). Source credits: [`NOTICE`](NOTICE).

Suggested attribution when you reuse the data:

> LWords level data © 2026 kubabit (at) digitalmind.cz, CC BY-SA 4.0, https://github.com/jakubfrieb/lwords-dictionary –
> adapted from wordfreq (Robyn Speer et al., incl. SUBTLEX by M. Brysbaert et al.) and
> Universal Dependencies Czech-CAC, Czech-CLTT, Czech-PUD, English-EWT and Wiktionary (Wiktionary
> contributors, via kaikki.org) (CC BY-SA 4.0); 12dicts and ENABLE (public domain).

The Rubik font shipped in the app is licensed separately under the SIL Open Font License 1.1
([`fonts/OFL.txt`](fonts/OFL.txt)).
