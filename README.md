# 听写本 Tīngxiě

A Chinese vocabulary app for spaced-repetition flashcards and dictation (听写) practice. Import your word list, hear each word read aloud, and write it from memory.

It runs entirely in your browser as a single HTML file, with no account, no server, and no install.

## Features

**Review** shows flashcards: see the character in a 田字格 grid, recall the pinyin and meaning, then flip to check. Rate each card **Again**, **Hard**, **Good**, or **Easy**, or mark it **Mastered** so it never comes back.

Each review starts with a choice of session:

- **Spaced review:** Words that are due come first, then new words up to your daily limit. Your ratings decide when each word comes back.
- **Study all:** Every word you haven't mastered, in random order, as extra practice. Ratings don't change your review schedule, and the daily new-word limit doesn't apply. Words you've practised are skipped until you start a fresh round.

Pick a session size of 5, 10, 20, or all of the available words. When a session ends you can continue with the next batch.

**Quiz** is the dictation (听写) practice. It reads 10, 20, or all of your words in random order, then shows your score and what you missed.

- Single characters are read the way a teacher would in class (诗，古诗的诗) so sound-alike characters can be told apart. Choose **Phrases** to be dictated the example phrases instead.
- Answer by **typing**, or by **writing on paper** and marking yourself.
- When the answer is shown, the stroke order animation pops up, with the buttons to mark yourself or go to the next word.
- **Retry the missed** runs a new round with only the words you got wrong.
- Missed words are added to your flashcard reviews automatically.

**笔顺 Stroke order** animates how to write a word, stroke by stroke. It pops up in the Quiz, and you can open it from the back of a flashcard. Choose **Write it yourself** to trace each stroke in order: wrong strokes are rejected, and a hint appears after 2 misses. Stroke order needs an internet connection.

**Reading** shows your textbook passages with pinyin above each character. Tap any word to hear it and see its meaning, along with any matching words in your deck. Words from your deck are underlined, and the deck words in each lesson are listed at the top. Play a single line or the whole lesson aloud, and turn the pinyin or underlining off. Passages come from lesson packs (see below).

**Decks** lets you import word lists from CSV, Excel, or lesson packs, switch between decks, mark words mastered, set a daily limit for new words in Spaced review, pick the voice that reads words aloud under **Voice** (with a **Test** button), choose a light, dark, or system theme under **Appearance**, and back up or restore your progress.

## Importing words

Upload a `.csv`, `.tsv`, `.xlsx`, or `.xls` file with these columns:

| word | pinyin | translation_en | example | example_en |
|------|--------|----------------|---------|------------|
| 诗 | shī | poem; poetry | 古诗、诗人、诗歌 | ancient poem, poet, poetry |
| 首 | shǒu | head; measure word for songs | 一首歌、首先、首都 | a song, first of all, capital city |

- Separate example phrases with `、` and their translations with commas, in the same order.
- Only `word` is required. Common alternative headers such as `hanzi`, `汉字`, `meaning`, and `拼音` are also recognized.
- If there's no header row, columns are read in the order shown above.
- Saving a CSV from Excel? Choose **CSV UTF-8** so the Chinese characters survive.

### Passages

To read your own text in the **Reading** tab, import a `.json` file that holds just the Chinese text:

```json
{ "passage": { "title": "月亮", "text_zh": "快中秋了，阳历是九月。…\n\n连着几个台风来，…" } }
```

- The app splits the text into words and adds pinyin automatically. Check words with more than one reading, such as 长 or 还.
- `title` is optional. You can type or change it when importing. Blank lines (`\n`) start a new paragraph.
- `text` or `content` also work in place of `text_zh`, and `"passages": [...]` imports several at once.
- Adding pinyin needs an internet connection the first time.
- Tapping a word shows its pinyin and stroke order, plus the meaning if the word is in your deck. For your own meanings, use a lesson pack instead.

### Lesson packs

A lesson pack is a `.json` file that adds words and textbook passages to a deck in one import. Its passages appear in the **Reading** tab.

```json
{
  "app": "tingxie-pack",
  "deckName": "一年级下册 L10–12",
  "cards": [
    {"w": "诗", "p": "shī", "m": "poem; poetry", "ex": "古诗、诗人", "exen": "ancient poem, poet"}
  ],
  "lessons": [
    {
      "id": "l10", "num": 10, "title": "古诗二首", "pages": "62–63",
      "passages": [
        {
          "title": "池上", "py": "chí shàng",
          "era": "唐", "eraPy": "táng", "author": "白居易", "authorPy": "bái jū yì",
          "poem": true,
          "paras": [
            [["小娃", "xiǎo wá", "little child"], ["撑", "chēng", "to pole (a boat)"], ["，"]]
          ]
        }
      ]
    }
  ]
}
```

- `cards` use the same fields as a spreadsheet row: `w` word, `p` pinyin, `m` meaning, `ex` examples, `exen` example translations.
- Each paragraph or line in `paras` is a list of words, each written as `[text, pinyin, meaning]`. Punctuation is just `[text]`.
- `py` and the other `…Py` fields hold the pinyin for titles, eras, and authors, one syllable per character, separated by spaces.
- Set `"poem": true` to center the lines. `num` sets the lesson order, and `pages` is shown under the lesson title.
- Importing a lesson with the same `id` again replaces it.

## Spaced repetition

Scheduling uses **FSRS-4.5** (Free Spaced Repetition Scheduler) with its published default parameters. FSRS models how stable each word is in your memory and schedules the next review for when your chance of remembering it drops to your target, 90% by default. You can change the target to 85% or 95% under **Decks → Memory target**.

For a brand-new word, the first intervals are:

| Again | Hard | Good | Easy |
|-------|------|------|------|
| 1 day | 1 day | 4 days | 14 days |

After that, intervals adapt to how each word goes. **Again** also brings the word back a few cards later in the same session until you get it right.

## Keyboard shortcuts

| Key | Review | Quiz |
|-----|--------|------|
| `Space` | Flip card | Show answer (paper mode) |
| `1`–`4` | Again, Hard, Good, Easy | `1` missed, `2` got it (paper mode) |
| `0` | Again | |
| `M` | Mark as mastered | |
| `R` | Replay audio (after flipping) | Replay audio |
| `Enter` | Start or continue a session | Check answer, then next word |
| `Esc` | Close stroke order | Leave the answer box, or close stroke order |

## Requirements

- **A Chinese keyboard** (pinyin input) to type answers.
- **A Mandarin voice on your device.** Audio uses your browser's built-in text-to-speech. Pick a voice under **Decks → Voice**. In Chrome, "Google 普通话（中国大陆）" usually sounds best and is chosen automatically when available. If the app warns that no Chinese voice was found, add Mandarin (Chinese, China) speech in your system's language or accessibility settings and reload.
- A modern browser: Chrome, Edge, Safari, or Firefox.

## Your data

Decks and progress are saved in your browser's local storage on each device. Nothing is uploaded anywhere.

Because of that, progress doesn't sync between devices automatically. To move it, use **Decks → Download backup** on one device and **Restore from backup** on the other. Restoring replaces the decks and progress on that device.

## Running it yourself

The whole app is one file, `index.html`.

- **Locally:** Download it and open it in your browser.
- **On GitHub Pages:** Fork or copy this repository, then go to **Settings → Pages** and deploy from the `main` branch, `/ (root)` folder.

## Credits

- Scheduling algorithm: [FSRS](https://github.com/open-spaced-repetition) by the open-spaced-repetition project.
- Spreadsheet import: [SheetJS](https://sheetjs.com/).
- Pinyin for imported passages: [pinyin-pro](https://github.com/zh-lx/pinyin-pro).
- Stroke order: [Hanzi Writer](https://hanziwriter.org/), with stroke data from [Make Me a Hanzi](https://github.com/skishore/makemeahanzi).
- Fonts: Figtree and Noto Serif SC from Google Fonts.
