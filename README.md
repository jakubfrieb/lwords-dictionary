# LWords dictionary & level data

Word lists and puzzle levels used by **LWords**, a word puzzle game for Garmin watches
(Connect IQ). This repository exists so that the data is openly available under its licence,
**CC BY-SA 4.0** (see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE)). The game's source code is
not part of this repository.

## Contents

```
levels/en/levels_en_1.json … levels_en_4.json   English levels (4 tiers × 15)
levels/cs/levels_cs_1.json … levels_cs_4.json   Czech levels (4 tiers × 15)
wordlists/en/words.txt                          derived English word list (11 278 words)
wordlists/cs/words.txt                          derived Czech word list (13 319 lemmas)
fonts/OFL.txt                                   licence of the Rubik font used in the app
CREDITS.md                                      full credits of the app
PRIVACY.md                                      privacy statement of the app
LICENSE                                         CC BY-SA 4.0 legal code
NOTICE                                          source credits for the data
```

## Level format

Each `levels_<lang>_<tier>.json` is a JSON array of 15 levels. Tier 1–4 = 5 / 6 / 7 / 8 letters.
A level is an array of three items:

```json
[ ["L","T","E","Y","S"], ["STYLE","SET","YES","LET","YET"], 60 ]
```

1. **letters** – one string per tile, upper case; Czech letters with diacritics are separate
   letters (Č ≠ C). For 5–7 letters all tiles lie on the wheel, clockwise from 12 o'clock. For
   8 letters, 7 tiles lie on the wheel and the **last item is the centre tile**.
2. **target words** – all words of the tier vocabulary that can be built from the letters
   (each tile at most once), 3+ letters, base forms only, sorted longest first, then by
   frequency. The first word always uses all letters.
3. **time limit** in seconds for the TIME mode (20 s + 8 s per word).

## Word list format

`words.txt`: one word per line, upper case, UTF-8 (NFC), base forms only (singular nouns,
infinitives, masculine adjectives), 3–8 letters, sorted from the most frequent (line number =
rank). Proper nouns and abbreviations are removed, and so are the words on the game's
private blocklist of offensive words - this is exactly the vocabulary the level generator uses.

## How the data is made

- **English**: headwords of 12dicts *2of12id* (lower case only, no abbreviations from
  *abbr*), intersected with *3esl* and *ENABLE*; ranked by wordfreq frequency (headword plus its
  inflections).
- **Czech**: lemmas from the Universal Dependencies treebanks Czech-CAC, Czech-CLTT and
  Czech-PUD (release 2.18); proper nouns, abbreviations, foreign words and typos removed; ranked
  by the wordfreq frequency of the word forms assigned to each lemma.
- **Levels**: a deterministic generator picks a seed word of N letters from the most frequent
  words, takes **all** common words that can be built from its letters (so that the player is
  never refused a common word), checks word-count limits, avoids repeated letter sets, orders
  levels by difficulty and shuffles the wheel so that no target word can be read around it.

## Licence & attribution

The level files and the word lists in `wordlists/` are an adaptation (selected, filtered,
ranked, combined) of wordfreq data by Robyn Speer et al. and the Universal Dependencies Czech
treebanks, both CC BY-SA 4.0, and of the public-domain 12dicts and ENABLE lists. They are
licensed under the **Creative Commons Attribution-ShareAlike 4.0 International** licence
(https://creativecommons.org/licenses/by-sa/4.0/). Source credits: [`NOTICE`](NOTICE).

Suggested attribution when you reuse the data:

> LWords level data © 2026 Jakub Frieb, CC BY-SA 4.0, https://github.com/jakubfrieb/lwords-dictionary –
> adapted from wordfreq (Robyn Speer et al., incl. SUBTLEX by M. Brysbaert et al.) and
> Universal Dependencies Czech-CAC, Czech-CLTT, Czech-PUD (CC BY-SA 4.0); 12dicts and ENABLE
> (public domain).

The Rubik font shipped in the app is licensed separately under the SIL Open Font License 1.1
([`fonts/OFL.txt`](fonts/OFL.txt)).
