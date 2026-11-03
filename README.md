# Chat Insights

An offline, single-file dashboard for exploring an Instagram chat export. Open `chat_insights.html` in any modern browser, add your data, and get charts, streaks, reply speeds, emoji stats and a reader for the real conversation.

## Privacy

Nothing leaves your device.

- No uploads, accounts, analytics or tracking.
- The file makes no network requests of its own and uses no browser storage (no cookies, `localStorage` or IndexedDB).
- Your chats are read into memory only and are forgotten when you close or refresh the tab.
- The only link that goes to the internet is "Open on Instagram" on a shared reel, and only when you click it.

Keep the HTML file private only if you have loaded data into it; the file itself contains no chat data until you add some.

## Quick start

1. Download your data from Instagram: **Accounts Center → Your information and permissions → Download your information → Messages**. Choose the **JSON** format.
2. Unzip the download and find `your_instagram_activity/messages/inbox`.
3. Open `chat_insights.html` in a browser.
4. Drop the whole `inbox` folder onto the page, or use **Choose inbox folder**. You can also choose individual `message_1.json` files (select `message_2.json`, `message_3.json` and so on together for long chats).
5. Pick a chat from the list, then choose which person you are.

Supported: one-to-one chats and group chats, any number of participants.

## Features

### Loading
- Reads a full inbox folder, a single chat folder or loose JSON files. Drag-and-drop and the file picker both work.
- Merges `message_1.json`, `message_2.json`, ... for each chat.
- Searchable chat list with title, message count, date range and group size.
- Guesses which person you are (the name that appears in most chats) and asks you to confirm.
- Time zone picker (local time by default).
- Repairs Instagram's broken text encoding (UTF-8 bytes stored as Latin-1 escapes) so emojis and special characters display correctly.

### Dashboard tabs
| Tab | What it shows |
| --- | --- |
| Overview | Message totals and split per person, date span, active days, longest daily streak, longest silence, content types |
| Timeline | Weekly or monthly message volume per person, with the busiest period called out |
| Rhythm | 7 x 24 hour heatmap, plus a daily density calendar. Select a day to open its summary |
| Conversations | Sessions, who starts them, median reply time, conversation habits, marathon and longest conversations |
| Words and emojis | Top 10 emojis and top keywords per person, words per text |
| Reels and reactions | Most-shared accounts, hashtags in captions, reactions given and received |
| Busiest days | The 12 highest-volume days with hourly velocity, what was sent and the longest texts |
| Read the chat | Search, date jump, first message, latest message |

### Conversation reader
Opens as a panel from the right and shows the real messages as chat bubbles, with your messages on the right.

- Date headings and "2 h later" gap markers.
- Emoji reactions under each message.
- Shared reels appear as cards with the account, caption and an "Open on Instagram" link.
- Photos, voice notes and other media appear as a placeholder, because the export contains no media content.
- Load 40 earlier or later messages at a time. Press Esc to close.
- Open it from marathon conversations, a busy day, a "Longest texts" row, the habits table or any search result.

## How the numbers are calculated

- **Total messages:** every event in the export, including likes and reaction notices.
- **Session (conversation):** a new session begins whenever more than 30 minutes pass between messages.
- **Initiation rate:** the share of sessions started by each person.
- **Reply time:** the gap between a real message and the next real message from a different person, inside the same session. Likes and reactions are ignored. The median is reported.
- **Last word:** the person who sent the final real message of a session.
- **Messages in a row:** consecutive real messages from the same sender within a session.
- **Streak and silence:** consecutive calendar days with at least one message, and the longest run of days with none.
- **Content types:** text, reels and links (shares), photos, voice and other media, emoji reaction notices, likes and other.
- **Emojis:** counted from text messages, "Reacted ..." notices and message reactions.
- **Keywords:** words from text messages only, with common English and Hinglish filler words removed.
- **Time zone:** all hours, days and calendar cells use the zone chosen on the upload screen.

## Limitations

- Instagram does not label photos versus voice notes, so both appear as media.
- The export does not say which message a reaction replied to.
- Photos and videos are not viewable, because the JSON only references them.
- Reaction and "Liked a message" detection is based on English wording.
- Very large inboxes (hundreds of chats) take a moment to scan, since every file must be read to build the list.

## Tech notes

- React 18, Recharts and Tailwind CSS, compiled into one self-contained HTML file with the fonts embedded (Bricolage Grotesque and Figtree). It works offline.
- Built with esbuild (JSX and bundling) and the Tailwind CLI. The source is a single React file with this structure: data loading and encoding repair, an `analyze()` function that computes all metrics, then the tab components and the reader.
- To change the design or add a metric, edit the source, run Tailwind and esbuild, and inline the output CSS and JS into the HTML template.

## Files

- `chat_insights.html` is the app. Open it in a browser.
- `README.md` is this document.
