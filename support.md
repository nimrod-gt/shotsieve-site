# Shotsieve Support

Shotsieve is a photo viewer and sorter for macOS and Windows, sold in the Mac App Store and the
Microsoft Store. This page tells you how to get help.

## Contact

Write to **subbotin.na@gmail.com**. The developer, Nikita Subbotin, reads every message.

The quickest way to write is from the app itself: press `H`, then **Report a problem…**. The
dialog shows the version and the platform, and has three buttons:

- **Email support** opens a new mail to the support address with the version in the subject.
  Attach the log file to it.
- **Open log folder** shows the log file, `photo.log`, in Finder or Explorer.
- **Copy log** puts the last 64 KB of the log on the clipboard, for pasting into the mail.

The log holds the errors the app met, with the names and paths of the photos and folders involved.
It never holds your API keys, your photos or the tags. You can read it before you send it. The
[privacy statement](https://nimrod-gt.github.io/shotsieve-site/privacy), section 6, lists everything it holds.

## Where the log is

- Mac App Store: `~/Library/Containers/com.subbotin.shotsieve/Data/Library/Preferences/fyne/com.subbotin.shotsieve/photo.log`
- Windows: `%AppData%\fyne\com.subbotin.shotsieve\photo.log`

When the app closes because of an error it could not recover from, it writes `crash.log` in the
same folder, and the next start offers to report it.

## Questions

**Do I pay for the tags the app generates?**
Shotsieve charges nothing per photo. The AI that writes the tags is your own: the AI provider bills
your own account or subscription, and a local model costs nothing per photo. The app holds no
account of its own.

**Which AI can I use?**

- Mac App Store: the Claude API or the OpenAI API with an API key of your own, or a vision model
  on your Mac through Ollama, LM Studio or llama.cpp. When the Claude CLI (Claude Code) or the
  Codex CLI is installed on your Mac and the app can run it, Settings lists it too. The Settings
  window (`I`) shows every backend the app can use on your Mac.
- Microsoft Store: the same three, plus the Claude CLI (Claude Code) and the Codex CLI on your own
  Claude or ChatGPT subscription.

Help (`H`) > AI setup tells how to set up each one.

**A generation ends with "fix it in Settings (I)". What now?**
The settings can fix the error: a missing or rejected key, an unknown model, or a server that does
not answer. Press `I`, check the backend and its rows, and generate again.

**I deleted a photo by mistake.**
Delete moves the JPEG and its RAW to the Trash (macOS) or the Recycle Bin (Windows), so you can put
them back from there. A memory card or a network share keeps no trash; the app tells you when the
files on it are gone for good.

**Where are my tags?**
In the `.xmp` sidecar beside the RAW and in the JPEG itself, as the switches in Settings say.
Lightroom reads the sidecar but sorts its keywords alphabetically, so upload the JPEG the app wrote
to keep the keywords in their order.

## Privacy

The app collects nothing and sends nothing to the developer. The
[privacy statement](https://nimrod-gt.github.io/shotsieve-site/privacy) lists what leaves your computer, when, and to whom.
