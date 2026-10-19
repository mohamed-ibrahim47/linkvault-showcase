# LinkVault

**All the links people send you, sorted by topic, on your phone. No logins, no servers.**

That YouTube lecture from a group chat, the article a friend sent last month, the course you saved on Facebook: LinkVault collects them in one place and sorts them by what they're about, in Arabic or English.

**[Try the clickable mockup →](https://mohamed-ibrahim47.github.io/linkvault-showcase/)**

> The source code is private. This page shows what the app does and how it works.

## Screenshots

<!-- Add phone screenshots here: Library, a category, Unsorted, Import, the quick-save sheet -->
| Library | Category | Import |
|---|---|---|
| _soon_ | _soon_ | _soon_ |

## The core idea: it never sees a password

Links come in only two ways, both started by you:

- **The share sheet.** Tap Share on any link in any app and pick LinkVault. It's saved instantly, and a small sheet shows where it was sorted, with a chance to change it.
- **Exports you download yourself.** WhatsApp's "Export chat", Meta's "Download your information", your browser's bookmarks file. LinkVault reads the file on your phone. It never logs in anywhere.

Everything is stored and processed on the device. There is no LinkVault server.

## What it does

- **Imports**
  - **WhatsApp chats**: Android, iPhone and Arabic-locale export formats, `.txt` or `.zip`.
  - **Facebook and Messenger**: saved items and chats from Meta's download.
  - **Browser bookmarks**: Chrome, Edge, Firefox, Safari, Brave or Google Takeout. Your folder names become sorting hints.
- **No duplicates.** Import the same chat again and only the new links come in. The same link from two people is one link with two sources.
- **Previews.** Titles and thumbnails are read in the background, with a Wi-Fi-only option and polite pacing per site.
- **Organisation.** Two-level categories you can edit (Tech › ML, Programming…; Entertainment › Movies…), tags, and a status for each link: New, Done or Archived. Tap a link and it opens in its own app (YouTube, LinkedIn…).
- **Search** by title, description, tags, your notes, or who sent it and in which chat.
- **Arabic and English** with full right-to-left layout.

## How the sorting works

Sorting runs entirely on the phone, in two layers:

1. **Keywords first.** A fast scorer works from the moment you install, with no download.
2. **Then meaning.** The app downloads a small multilingual AI model (multilingual-e5-small, 8-bit) once, from a public source, with nothing about you sent.
   - Each link's text becomes a "meaning" vector. Arabic and English land in the same space, so an Arabic title can sort next to English ones.
   - A new link is compared with your already-sorted links (nearest neighbours), each category's average, and each category's description.
   - **Your corrections count more.** When you move a link, it becomes a stronger example than one the app placed itself, so sorting adapts to how *you* organise.
   - If the model can't run on a phone, sorting quietly falls back to keywords.

## How it's built

- **Flutter**, Android first (Android 8+), with iOS planned.
- **SQLite** on the device, plus a JSON export of your library.
- The AI model runs through **ONNX Runtime** in native Android code on a background thread. Tokenizing and the sorting logic are pure Dart, so they're unit-tested and run in background isolates.
- A foreground notification keeps long imports and preview reads alive when you leave the app.
- Unit tests cover the parsers, URL cleaning, the tokenizer, the AI sorter, previews and the database.

## Coming next

- Instagram saved posts, YouTube (Takeout) and LinkedIn imports
- iPhone version with a Share Extension
- One-line summaries with on-device models (Gemini Nano / Apple Foundation Models)
- Automatic backup to Google Drive / iCloud

---

Made by [Mohamed Ibrahim](https://github.com/mohamed-ibrahim47).
