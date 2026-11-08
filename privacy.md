# Privacy Statement

**Shotsieve** (bundle ID `com.subbotin.shotsieve`)
Effective date: 22 September 2026

This statement describes what the app does with your data: what leaves your computer, when, and to
whom; what it stores on your computer; and what it never does. It is written from the app's source
code and is updated with the app.

## 1. Summary

- The app has no user accounts, no analytics, no telemetry, no crash reporting, no update check and
  no advertising. The developer receives nothing from the app unless you send it yourself.
- The app opens a network connection in exactly one situation: when you press **Generate** in the
  Tags dialog, it sends the current photo and the text you typed to the AI service or tool you chose
  in Settings, under your own account or API key. Section 2 lists every backend and what each one
  sends.
- Everything else the app does stays on your computer: it reads the folders you open and writes
  metadata, labels, preferences and a log file, as described in sections 4 to 6.

## 2. What leaves your computer

Nothing leaves your computer until you press Generate (`Shift+Enter`) or Background (`Ctrl+Enter`)
in the Tags dialog. Each generation sends, to the backend selected in Settings:

- the current photo, as described for each backend below;
- the text you typed into the Tags dialog: **Concept**, **Location**, **Notes** and, when ticked, the
  **Editorial** date;
- the app's built-in instructions for the model, which describe how to write a stock-photo title,
  keywords and place; they contain no information about you or your computer.

The answer that comes back is the generated title, keywords and place. The app keeps it in the
dialog and, depending on your autosave settings, writes it into your files (section 5): out of the
box the JPEG is written automatically and the sidecar only once you switch that on in Settings; the
Save button, shown once you switch it on in Settings, writes whatever is not written
automatically.

A video is never sent. When the current file is a video, the app takes still frames out of it - as
many as **Frames of a video sent to the AI** in Settings says: 1 to 9, three out of the box, spread
evenly from the first frame to the last - puts them side by side on one reduced JPEG (long edge 1568 pixels, quality 85, no metadata)
and sends that picture wherever a photo would have gone, with a line saying that the file is a
video. Where the operating system gives the app only one frame of a video, one frame is sent.

The app never sends your name, your e-mail, your account details, other photos, your log file or
anything else. The photo's full path, folder names included, is handed only to the Claude CLI, as
described below; every other backend receives the file name alone.

The backends are, as named in Settings:

### Local model

The app sends the request to the server address configured in Settings, by default
`http://localhost:11434/v1` on your own computer, over the OpenAI-compatible chat completions API.
The photo is sent as a reduced JPEG copy (long edge 1568 pixels, quality 85, re-encoded, so the
copy carries no EXIF or XMP metadata) together with its file name, never its folder. When the
model returns fewer than 50 keywords, the app asks it up to two more times for the missing ones,
and each follow-up sends the photo again.

If you set the server address to a machine other than your own, every generation goes to that
machine and its operator, over the connection you configured (`http` or `https`). The optional key
in Settings is sent to that server as a bearer token.

When the address is `localhost` on port `11434`, no server answers and Ollama is installed, the app
starts `ollama serve` for you; that server keeps running after the app exits. When the server does
not know the configured model, the app asks the server to download it; the server, not the app,
then fetches the model from Ollama's registry over the internet. The default model is `gemma4:26b`.

### Claude CLI

The app runs the `claude` command line tool installed on your computer, with the photo's folder
as its working directory, and passes it the photo's full path and the text you typed. The tool
reads the original photo file and sends it, with the text and the app's instructions, to Anthropic
under your own Claude subscription. The app holds no credentials for this: it never sees your login
and never stores a key for it. How the app starts the tool's sign-in for you is in the section
"Signing in to a CLI" below.

For a video the tool is never pointed at the video or at its folder: the app writes the picture of
its frames as `photo.jpg` into a temporary directory of its own in your system temporary
directory, runs the tool there and passes it that path. The directory is removed when the run
ends.

The tool is started with `--allowedTools Read`, `--permission-mode dontAsk`, `--safe-mode`,
`--no-session-persistence` and `--model opus`, so it may only read files, cannot write or run
anything, and keeps no session on disk. The app's instructions are handed over through a
temporary file in your system temporary directory, deleted when the run ends. What the tool itself
stores and sends is governed by Anthropic's terms and privacy policy.

### Claude API

The app sends the request over HTTPS to `https://api.anthropic.com`, authenticated with the API key
you entered in Settings. The photo goes as the same reduced JPEG copy as for the Local model, with
its file name, never its folder. The default model is `claude-opus-5`. With **Cache the prompt**
ticked, the request marks the app's built-in instructions as cacheable on Anthropic's side for a few
minutes; nothing extra is sent. After each generation the app writes the model name and the token
counts to its log file (section 6), never the key and never the photo. The app ignores environment
variables such as `ANTHROPIC_API_KEY`; only the key from Settings is used. Anthropic's terms and
privacy policy govern their handling of the request.

### Codex CLI

The app writes the reduced JPEG copy of the photo as `photo.jpg`, together with the expected answer
format, into a temporary directory of its own in your system temporary directory, and runs the
`codex` command line tool installed on your computer from that directory with `--ephemeral`,
`--sandbox read-only` and `--skip-git-repo-check`. The tool sends the photo, the text you typed and
the app's instructions to OpenAI under your own ChatGPT or Codex sign-in; the app holds no
credentials for it. The temporary directory is removed when the run ends. What the tool stores and
sends is governed by OpenAI's terms and privacy policy.

### Signing in to a CLI

When a generation through the Claude CLI or the Codex CLI fails, the app may ask the tool whether
anyone is signed in, by running `claude auth status --json` or `codex login status`. It does so
after a failure that does not explain itself, and again after a sign-in it started; it does not
after a generation that was stopped, ran out of time or hit a usage limit. It reads one thing from
the answer: whether a sign-in exists. The answer of the Claude tool also names the
e-mail address and the organisation of the account; the app does not keep, show or log them.

When nobody is signed in, or when the failed generation itself said that the sign-in is the
trouble, the app runs `claude auth login --claudeai` or `codex login`. The tool
then opens its provider's sign-in page in your browser and, while it waits, listens on your own
computer for the browser to come back. You sign in on the provider's page, not in the app. The
tool stores the result itself, where it keeps it when you sign in from a terminal; the app never
receives, reads or stores the credential. The app then runs the generation once more.

A generation that works starts none of this, and the app never signs you out.

### OpenAI API

The app sends the request over HTTPS to `https://api.openai.com/v1/chat/completions`,
authenticated with the API key you entered in Settings, with the reduced JPEG copy of the photo and
its file name. The default model is `gpt-5`. Every request carries the fixed prompt cache key
`photo-stock-tags`, which lets OpenAI reuse the app's instructions between generations and
identifies nothing about you. OpenAI's terms and privacy policy govern their handling of the
request.

### Builds from the stores

A build sold through the Mac App Store runs in Apple's sandbox and may offer fewer backends than
listed here: it offers the Claude CLI and the Codex CLI only when it can run them on your Mac, which
it finds out at launch by starting each one once with `--version`. It signs in to a CLI the same
way as every other build. The Settings window shows which
backends it can use. That build tells a CLI it runs your real home folder, since the sandbox gives the
app a home of its own. Every build puts the folder of the CLI's own binary first on the `PATH` the
CLI is run with. Nothing else is changed in its environment.

## 3. Credentials

The API keys for the Claude API, the OpenAI API and the optional key for a Local model server are
stored, as you typed them, in the app's preferences file (section 5, `preferences.json`), in plain
text, protected only by your user account's file permissions. Their preference names are
`tagsBackend.api.apiKey`, `tagsBackend.openai.apiKey` and `tagsBackend.local.apiKey`. They are sent
only to the service they belong to, are never written to the log file and never read from
environment variables. To remove a key, clear its field in Settings or delete the preferences file.

The Claude CLI and the Codex CLI keep their own sign-in outside the app; the app never reads it.
It asks the tool whether a sign-in exists, and can start the tool's own sign-in (section 2,
"Signing in to a CLI").

## 4. What the app reads

- The folders you open, and their subfolders for the folder tree. Folders whose name starts with a
  dot are skipped in the tree.
- Your JPEG files: the image, the EXIF block and the embedded XMP packet, for display, rating,
  dates and existing tags.
- Your video files (`.mp4`, `.mov`, `.m4v`), on macOS and Windows: the header of the container,
  for the length and the recording date, and single frames, which the operating system decodes for
  the app - macOS through AVFoundation, Windows through Media Foundation or, where that gives nothing, as
  the thumbnail its shell makes of the file - for
  display and for the picture sent to an AI (section 2). A video is never read whole. One that the
  system can decode no frame of is left out of the list and not touched. On Windows the system may
  keep a thumbnail its shell made in its own thumbnail cache, as it does when Explorer shows the folder;
  the app does not write to that cache and does not read it. On Linux videos are not listed.
- `.xmp` sidecar files and `.photo-colors.json` files in those folders.
- File modification and creation times, for sorting by time, for the photo's date when the EXIF
  carries none, and to tell whether a camera wrote the recording time of a video in UTC or by its
  own clock.
- The folders where the `claude`, `codex` and `ollama` tools are usually installed, to find them,
  and whether VLC is installed (section 5, Playing a video).

The app never reads other folders, your browser, your mail or anything outside the folders you
open, except its own preferences and log files.

## 5. What the app writes on your computer

**XMP sidecar files.** A `.xmp` file next to the RAW when the photo has a RAW pair, next to the
JPEG when it has none, and for a video a file named after the whole video file
(`C0001.MP4.xmp`), with the title (`dc:title`, `dc:description`), keywords (`dc:subject`),
location (`Iptc4xmpCore:Location`, `photoshop:City`, `photoshop:State`, `photoshop:Country`) and,
in the app's own `photo:` namespace, `photo:Concept`, `photo:Notes`, `photo:Editorial` and
`photo:EditorialDate`, and, while **Editorial** is ticked, Shutterstock's editorial marker in
`exif:UserComment`, which replaces a comment another program left there. Unticking removes only
that marker. Properties written by other programs, such as Lightroom develop settings, are
preserved.

**Your JPEG files.** A favorite (`F`) writes `xmp:Rating` in place inside the file's embedded XMP
packet, with the file's size and layout unchanged. Title, keywords and location go into the same
packet in place when it has room; when it has none, the whole file is rewritten with the title and
keywords in the EXIF fields `ImageDescription`, `XPTitle` and `XPKeywords`, and the app says so.
Ticking **Editorial** writes Shutterstock's editorial marker into the EXIF field `UserComment`,
replacing whatever comment is there, and unticking removes that marker; either change rewrites the
whole file, and the app says so.
Every whole-file write goes through a temporary file named `<photo>.<number>.tmp` in the same
folder, which then replaces the original; the photo keeps its modification time. RAW files are
never rewritten.

**Your video files.** Never written. The tags of a video live in its sidecar alone, and a video
takes no favorite.

**`.photo-colors.json`.** One file per folder you label in, holding the file names of the labelled
photos and their colours, written through a `.tmp` file next to it. No paths, only names.

**Copies.** Copy writes the JPEG, the RAW when you choose so, and the RAW's sidecar with it, into
the folder you pick, keeping the modification time. A video is copied with its sidecar. The app remembers that folder and the copy
mode. In the Mac App Store build the app can write only into a folder you picked with Browse; a
folder you typed is refused by macOS, and the app shows that error.

**Deletion.** Delete moves the JPEG, its RAW pair and their sidecar - or the video and its
sidecar - to the Trash on macOS and the Recycle Bin on Windows, so they can be taken back. On volumes that keep no trash, such as memory
cards and network shares, and on Linux, the files are deleted permanently, and the app says which
of the two happened. The deleted photo's entry is removed from `.photo-colors.json`.

**Clipboard.** `Y` puts the current image on the system clipboard, through the system pasteboard
(AppKit) inside the app's own process on macOS and through PowerShell on Windows; a video is not copied. The Tags dialog can put the title or the keyword line on the clipboard
as text. Report a problem can put the end of the log file there (section 6).

**Playing a video.** Play (`P`) asks the operating system to open the video in VLC when VLC is
installed, and otherwise in the player set as the default for it. To know which, the app asks
macOS at launch whether an app with VLC's identifier is installed, and on Windows looks for
`vlc.exe` in VLC's folder under Program Files; it reads nothing else about your apps. The app
sends nothing with the request but the file to open, and what the player does with the file is
its own business.

**Preferences.** Fyne, the toolkit the app is built on, keeps a `preferences.json` in:

- macOS: `~/Library/Preferences/fyne/com.subbotin.shotsieve/`; the Mac App Store build runs in
  Apple's sandbox and keeps the same folder inside its container,
  `~/Library/Containers/com.subbotin.shotsieve/Data/Library/Preferences/fyne/com.subbotin.shotsieve/`
- Windows: `%AppData%\fyne\com.subbotin.shotsieve\`; the Microsoft Store build uses the same
  folder. Uninstalling the app, from the Store or from Settings > Apps, does not delete this folder
  or the API keys in it: delete the folder yourself if you want them gone.
- Linux: `$XDG_CONFIG_HOME/fyne/com.subbotin.shotsieve/` (by default `~/.config/fyne/com.subbotin.shotsieve/`)

It holds: `lastRoot` and `lastDirectory` (the folders you last opened), `copyDestination` and
`copyMode`, `lastRootBookmark` and `copyDestinationBookmark` (macOS only: a bookmark of the folder
you last opened and of the copy folder, made by macOS and stored as text; it holds the location of
that folder and is what lets the sandboxed app open the folder again after a restart, and it
follows the folder when you rename or move it), `sortOrder` and `sortDescending`, `showTags`, `autoSaveXMP`, `autoSaveJPEG`,
`showSaveButton`, `videoTagFrames` (how many frames of a video are sent to an AI), `helpShown`,
`onboardingShown`, `tagsBackend` (the selected backend, or `none`;
when none was chosen yet, the app sets it at launch by looking on disk for the `claude`, `codex`
and `ollama` programs, a check that sends nothing anywhere),
`claudePath` (the path to the `claude` tool when you typed one), and the settings of each backend:
`tagsBackend.api.apiKey`, `tagsBackend.api.model` and `tagsBackend.api.promptCache` for the Claude
API; `tagsBackend.codex.path` and `tagsBackend.codex.model` for the Codex CLI;
`tagsBackend.openai.apiKey` and `tagsBackend.openai.model` for the OpenAI API;
`tagsBackend.local.baseURL`, `tagsBackend.local.model` and `tagsBackend.local.apiKey` for the Local
model. Nothing else is persisted: window size, zoom and the photo cache live in memory only.

**Temporary files for generations.** The Claude CLI backend writes the app's instructions to a
`photo-tags-*.txt` file and, for a video, a `photo-claude-*` directory with the picture of its
frames; the Codex CLI backend writes a `photo-codex-*` directory with the reduced photo copy. All
of them are in your system temporary directory and are removed when the run ends.

**Nothing else.** The app keeps no thumbnail, video frame or image cache on disk, no database, no history and
no cloud copy of anything.

## 6. Log file and problem reports

The app writes a log file next to its preferences (paths in section 5): `photo.log`, rotated to
`photo.log.1` when it grows past 2 MiB at the next start, so at most two log files exist. A crash the
app could not recover from is written to `crash.log` and kept as `crash-last.log` until the next
clean start. The files are readable by your user account only.

The log holds the app's version and platform, error messages and, when something goes wrong, the
name or full path of the photo or folder involved, the address of the AI server that answered and a
short excerpt of its error message, panic stack traces, and the token counts of Claude API
generations. It never holds your API keys, the photo itself, the generated tags or the text you
typed.

The log is never sent anywhere by the app. Help (`H`) > Report a problem offers three actions, each
of which you start yourself: open the log folder; copy the last 64 KiB of the log to the clipboard;
or open a new message in your mail program addressed to the developer, with the app version in the
subject and the log's path in the body, and no attachment. You see and send that message yourself,
or not at all.

## 7. Other connections the app can open

The Settings window and the first-launch onboarding show links to the setup pages of Claude Code,
the Anthropic and OpenAI key consoles, the Codex CLI and the Ollama download page. They open in
your browser only when you click them and carry no data. Report a problem opens the log folder in
your file manager and your mail program as described above.

## 8. Third parties

The app itself talks to no one else. Depending on the backend you choose, your photo and text
reach:

- **Anthropic**, through the Claude CLI or the Claude API, under their terms and privacy policy
  (https://www.anthropic.com/privacy);
- **OpenAI**, through the Codex CLI or the OpenAI API, under their terms and privacy policy
  (https://openai.com/policies/privacy-policy);
- **Ollama** (https://ollama.com), which the Local model backend can start on your computer and
  which downloads models from Ollama's registry; the photo itself stays on your computer when the
  server runs there;
- the **operator of any other server** you enter as the Local model address.

The developer has no access to any of these accounts and receives nothing from them.

## 9. Changes and contact

This statement is updated together with the app whenever the behaviour it describes changes; the
effective date at the top says when it last changed.

Questions about this statement: Nikita Subbotin, subbotin.na@gmail.com.
