# Brood Mind

A desktop control center for AI coding agents that run in terminals, for macOS and Linux.

Agents keep running in your own terminal (macOS Terminal, iTerm2, PyCharm, any Linux terminal) and stay
native there: your scrollback, selection and search. Brood Mind shows every session in one resizable window:
what each agent is doing (working, needs you, your turn), its model and context, a live view of its screen,
and a box to type into it. Permission requests can be allowed or denied right from the app. Local only: no
telemetry, no accounts; it goes online only when you ask it to check for updates.

Works with **Claude Code** (`claude`), **Codex CLI** (`codex`) and **Antigravity CLI** (`agy`), and with
any other command in a plain terminal view.

- [Install](#install): [macOS](#macos) · [Linux](#linux)
- [First run](#first-run)
- [Starting a session](#starting-a-session)
- [The window](#the-window)
- [Typing and keys](#typing-and-keys)
- [Permission requests](#permission-requests)
- [Recent: resume past sessions](#recent-resume-past-sessions)
- [Settings](#settings)
- [Ending sessions](#ending-sessions)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Updating Brood Mind](#updating-brood-mind)
- [Troubleshooting](#troubleshooting)
- [Uninstall](#uninstall)
- [License](#license)

## Install

You need:

- **macOS** on Apple Silicon (M1 or newer), or **Linux** on x64 or arm64 (Ubuntu 22.04 or newer, or a
  distribution of the same age);
- at least one agent, installed and logged in the usual way: Claude Code (`claude`), Codex CLI (`codex`)
  or Antigravity CLI (`agy`). Check that it runs in a terminal on its own first.

Download the latest release from the
[Releases page](https://github.com/bnikiforakis/BroodMind-releases/releases).

### macOS

1. Download `Brood.Mind_<version>_aarch64.dmg`, open it, and drag **Brood Mind** onto **Applications**.
   Always start the app from Applications, not from the disk image (eject it afterwards).
2. Open **Brood Mind** from Applications. The app isn't signed by Apple, so the first time macOS refuses
   to open it:
   - open **System Settings › Privacy & Security**, scroll down to *"Brood Mind" was blocked*, click
     **Open Anyway**, and confirm; or
   - in a terminal: `xattr -dr com.apple.quarantine "/Applications/Brood Mind.app"`, then open it again.
3. Allow **notifications** when asked, to hear when an agent needs you or has finished.
4. Continue with [First run](#first-run).

Two more permissions are asked for the first time you start a session *from the app* (not needed for
sessions you start in a terminal yourself):

- **Automation:** "Brood Mind wants to control Terminal" (or iTerm). Click **Allow**, so the app can open
  a terminal running your session.
- **Accessibility** (Terminal.app only): Terminal has no scripting command for new tabs, so Brood Mind
  presses ⌘T for you. Turn **Brood Mind** on under **System Settings › Privacy & Security ›
  Accessibility**. Without it, sessions open in a new window instead of a new tab.

### Linux

Pick one:

- **Debian, Ubuntu and derivatives:** download `Brood.Mind_<version>_amd64.deb` (or `_arm64.deb`), then

  ```sh
  sudo apt install ./Brood.Mind_<version>_amd64.deb
  ```

  This adds Brood Mind to your applications menu and puts the `brood` command in `/usr/bin`, so you can
  skip step 1 of [First run](#first-run).
- **Any distribution (AppImage):** download `Brood.Mind_<version>_amd64.AppImage` (or `_aarch64`),
  then

  ```sh
  chmod +x Brood.Mind_*.AppImage
  ./Brood.Mind_*.AppImage
  ```

  Keep the AppImage where it is (e.g. in `~/Applications`): the `brood` command the app installs is a
  copy taken from it and kept up to date when you run a newer AppImage.

Notifications use your desktop's notification service. Brood Mind can open sessions in **GNOME Terminal,
Konsole, kitty, Alacritty, WezTerm** or your distribution's default terminal (`x-terminal-emulator`),
whichever are installed.

## First run

1. **Put `brood` on your PATH.** Agents run under the small `brood` command, which is how the app sees
   and types into them. When it isn't set up yet, the app's start screen and a banner at the bottom of
   the session list say so: click **Install command-line tool** (also in **Settings › Setup**). It needs
   no password:
   - it puts `brood` in the first folder your shell searches that it may write to: `~/.local/bin`,
     `/opt/homebrew/bin` or `/usr/local/bin` (on macOS a link into the app, so it follows app updates);
   - if that folder isn't on your PATH yet, it shows the line to add to your shell profile
     (`~/.zshrc` or `~/.bashrc`), e.g. `export PATH="$HOME/.local/bin:$PATH"`;
   - it never replaces a `brood` it didn't install, and tells you if another one comes first on your
     PATH.
2. **Open a new terminal window** (so it picks up the new command) and check: `brood --version`.
3. **Start an agent**, from the app with **+ New** or from any terminal with `brood claude` (see
   below). It appears in the app's session list at once.

**Settings › Agents** shows which agents Brood Mind found, with their versions.

## Starting a session

### From any terminal

Put `brood` in front of the agent you'd normally run:

```sh
brood claude                    # Claude Code (same as: brood run -- claude)
brood codex                     # Codex CLI
brood agy                       # Antigravity CLI
brood claude --model sonnet     # arguments pass through to the agent
brood run --name api -- bash    # any other command, with a name for the list
```

The agent runs right there, exactly as before: your terminal's scrollback, selection and search keep
working, and you can type in the terminal and in Brood Mind at the same time. **Without the app running,
`brood` behaves exactly like running the agent directly**; if the app quits or crashes, your agents keep
going.

This works in any terminal, including the ones built into IDEs (PyCharm and other JetBrains IDEs, VS Code).
On macOS the session list shows each terminal's own app icon.

### From the app

Click **+ New** (⌘N on macOS, Ctrl+N on Linux):

1. **Agent:** Claude Code, Codex, Antigravity, Shell (`bash`) or Custom (any command).
2. **Model** and **Thinking** (for new agent sessions):
   - Claude Code: Fable, Opus, Sonnet or Haiku, thinking low / medium / high / xhigh / max. Starts at
     **Opus · medium**.
   - Codex: the models your Codex knows, each with the thinking levels it supports. **Default** uses
     your own Codex settings.
   - Antigravity: its models (the list takes a moment to load; the thinking level is part of the model's
     name, e.g. *Gemini 3.8 Flash (Low)*). **Default** uses agy's own choice.
3. **Command and arguments:** filled in from your choices (e.g. `claude --model haiku --effort low`). You
   can edit it; your additions stay when you change a picker.
4. **Project folder:** type it, click **Choose…**, or pick a recent one.
5. **Name** (optional): shown in the list; defaults to the folder's name.
6. **Open in:**
   - **Terminal / iTerm2** (macOS) or your **Linux terminal**: Brood Mind opens a new tab (a new window if
     none is open) running the session, then click **Start**;
   - **PyCharm or another terminal (copy a command):** click **Copy command**, then paste the copied line
     (`cd … && brood run …`) into that terminal.

## The window

On the left, the **session list** (drag its edge to make it wider or narrower); on the right, the selected
session's screen with a message box under it. **Sessions / Recent** at the top switch between running
sessions and [past ones](#recent-resume-past-sessions).

Each session's box shows:

1. a state dot, the terminal it runs in, its name, the session's title and its state;
2. the folder it runs in;
3. the agent, model, thinking level and how full its context is (bar, %, tokens);
4. when it matters: what it asks for, or why it failed;
5. your account's usage: the 5-hour and weekly windows with when they reset (Antigravity: weekly);
6. the permission mode, e.g. `⏵⏵ auto mode`, `⏸ plan mode`, `⏵⏵ accept edits`.

| Dot | State |
|---|---|
| orange, with a ping | **Needs you**: a permission request or a question |
| blue, pulsing | **Working** |
| green | **Your turn**: it finished (bold until you open it) |
| white | **Idle**: your turn, already seen |
| red ! | **Failed**: an error stopped it |
| grey | **Ended** or **Exited** |
| amber | **Reconnecting** |

Sessions are grouped into Needs you, Working, Waiting and Exited. A **notification** arrives when a
session needs you or finishes (unless Brood Mind is the window in front); click it to jump to that
session.

The **screen view** shows the agent's terminal as text that wraps to the window, in its colours, with the
whole session above it (scroll up to read back; it follows new output again once you scroll to the
bottom). Tables are shrunk to fit instead of wrapping. For Claude Code the view hides Claude's own input
box and banner, since Brood Mind's message box replaces them; menus and questions are always shown in
full.

When a Claude Code or Codex session has used **85% or more of its context**, a red **Compact** button
appears on its box and above its screen. It sends `/compact` (the agent summarises the conversation so
far), and only works while the agent waits for you.

## Typing and keys

- Clicking a session puts the cursor in the message box.
- **Enter** sends what you typed (followed by Enter). **Shift+Enter** or **Ctrl+J** start a new line;
  a multi-line message arrives as one paste. You can also click the ➤ button.
- Keys go to the session as in a terminal:
  - always: **Shift+Tab** (switch the agent's mode) and **Esc** (interrupt the agent);
  - when the message box is empty: **↑ ↓ ← →**, **Enter**, **Tab**, **Backspace**, **Ctrl+C**, for the
    agent's menus and pickers;
  - while the agent asks something and you aren't typing in the box: **1–9**, **y**, **n**.
- **Claude's suggestion:** when Claude Code proposes your next message (grey text in its terminal), the
  empty box shows it with "(Tab to use it)": **Tab** puts it in the box to edit or send.
- Keys typed in other fields (search, dialogs) stay there; Ctrl+C with text selected copies it.

## Permission requests

When **Claude Code** asks permission to use a tool, its session moves to **Needs you**, a notification
arrives, and an orange card appears above the message box with exactly what it wants: the command, the
file and the lines to be removed (red) and added (green), or the URL. Choose:

- **Allow**, or one of Claude's own "from now on" options (e.g. *Allow, and accept edits for this
  session*);
- **Deny**, or **Deny with message…** to tell Claude why or what to do instead;
- **Answer in terminal** to leave it to the terminal.

The terminal's own prompt stays open at the same time: answer it there (or with keys in Brood Mind) and
the card disappears by itself. Questions where Claude asks you to pick an option, and plan approvals, are
answered with keys or in the terminal, never by the card.

**Antigravity** gets the same card once its approval menu is on screen; its buttons press agy's own
keys (**Allow**, its "always allow in this conversation" options, **Deny**, **Answer in terminal**).
Options that would change agy's global settings are never offered. agy's "Do you trust the contents of
this project?" is answered in the terminal.

On Linux, a permission notification also has **Allow** and **Deny** buttons. On macOS, answer in the card.
Every decision taken in Brood Mind is logged in `decisions.jsonl` in the app's data folder (see
[Uninstall](#uninstall)).

## Recent: resume past sessions

**Recent** (top of the session list) lists every past Claude Code, Codex and Antigravity session on this
computer, including ones started without Brood Mind, newest first.

- **Search** by title or folder, and filter by agent: All, Claude Code, Codex, Antigravity.
- Group **By date** (Today, Yesterday, …) or **By project** (by folder).
- Hover a row and click **▶ Resume**: the session dialog opens with the resume command
  (`claude --resume <id>`, `codex resume <id>` or `agy --conversation <id>`) and the folder filled in;
  pick where to open it as usual.
- Sessions already open in Brood Mind show **Running**.

## Settings

Open **Settings** at the top of the session list (⌘, on macOS, Ctrl+, on Linux):

- **Brood Mind:** the app's version and **Check for updates** (see
  [Updating Brood Mind](#updating-brood-mind)).
- **Font size:** Small, Medium or Big for everything in Brood Mind (⌘+ / ⌘− or Ctrl+ / Ctrl− also work).
- **Agents:** Claude Code, Codex and Antigravity, each with its version and where it's installed (or
  *Not installed*). Codex also shows *"<version> available"* when it already knows of a newer version.
  **Update** runs the agent's own updater (`claude update`, `codex update`, `agy update`), which goes
  online and installs a newer version if there is one; the row then says **Up to date** or **Updated
  from A to B**, or shows the updater's error. Running sessions keep their version until you restart
  them. Antigravity can't be updated while one of its sessions is open.
- **Setup:** whether the `brood` command is on your PATH and is this app's version, and tmux (optional),
  with a hint when something is off; **Install command-line tool** / **Reinstall command-line tool**.
- **Usage:** per agent, your account's 5-hour and weekly use with when each resets.

## Ending sessions

- **End session** (top right of the screen view) asks the agent to stop, like closing its terminal, and
  forces it after 3 seconds.
- A session that ends normally (it exits, you end it, you close its terminal or press Ctrl+C) leaves the
  list; it's still in **Recent** to resume. A failed session stays so you can read the error; **Remove**
  clears it.
- Quitting Brood Mind doesn't stop any agent. When you open it again, the sessions reappear within a
  couple of seconds.

## Keyboard shortcuts

| macOS | Linux | Does |
|---|---|---|
| ⌘N | Ctrl+N | New session |
| ⌘, | Ctrl+, | Settings |
| ⌘1 … ⌘9 | Ctrl+1 … Ctrl+9 | Select the 1st … 9th session in the list |
| ⌘+ / ⌘− | Ctrl++ / Ctrl+− | Font size |
| ⌘⇧A / ⌘⇧D | Ctrl+Shift+A / Ctrl+Shift+D | Allow / deny the permission request on screen |
| Enter | Enter | Send the message (empty box: Enter to the session) |
| Shift+Enter, Ctrl+J | Shift+Enter, Ctrl+J | New line in the message box |
| Esc | Esc | Interrupt the agent |
| Shift+Tab | Shift+Tab | Switch the agent's mode |

## Updating Brood Mind

Open **Settings** and click **Check for updates**. Brood Mind asks the latest release on GitHub; this is
the only time it goes online, and it never checks by itself.

- **"Brood Mind is up to date"**: nothing to do.
- **"Version X is available"**: click **Update to X and restart**. Brood Mind downloads the new version,
  checks its signature (so only genuine releases are installed), replaces itself and restarts. Your
  agent sessions keep running in their terminals and reappear in the list a moment later.
  - **Debian / Ubuntu (`.deb`):** installing asks for your password, as any system package does.
  - **AppImage:** the AppImage file is replaced in place; the `brood` command follows on the next start.
  - **macOS:** keep Brood Mind in **Applications** (an app started from the disk image or from
    Downloads can't replace itself).
- If the download fails (e.g. no network), the error shows with the **Update** button again, to retry.

Agents (Claude Code, Codex, Antigravity) are updated separately, from **Settings › Agents**.

## Troubleshooting

**`command not found: brood`.** Click **Settings › Setup › Install command-line tool**, then open a
*new* terminal window. If Setup says the folder isn't on your PATH, add the `export PATH=…` line it shows
to `~/.zshrc` (macOS) or `~/.bashrc` (Linux), then open a new window.

**Setup says another `brood` comes first on your PATH.** An older copy (e.g. one built with
`cargo install`) is found before the app's. Remove it (`cargo uninstall brood-cli` for a cargo copy),
then click **Reinstall command-line tool**. If agent sessions started from that copy are still running,
end them first: they keep using its path.

**A session doesn't appear in the app.** It must be started through `brood` (`brood claude`, not
`claude`). Sessions started before you installed or updated Brood Mind may need restarting to report their
state, model and context.

**macOS: "Brood Mind can't be opened" / "is damaged".** The app isn't signed by Apple; see step 2 of
[macOS](#macos). Make sure you open the copy in Applications, not the one on the disk image.

**macOS: sessions open in a new window, not a tab (Terminal.app).** Turn Brood Mind on under
**System Settings › Privacy & Security › Accessibility**, then quit and reopen Brood Mind.

**macOS: starting a session from the app does nothing.** Brood Mind may have been denied control of
Terminal or iTerm. Allow it under **System Settings › Privacy & Security › Automation › Brood Mind**.

**"Another Brood Mind app is already running".** Only one Brood Mind runs at a time. Quit the other one
(check the Dock, or `pkill brood-mind`), then open it again.

**No notifications.** Allow them for Brood Mind in your system's notification settings. They don't show
while Brood Mind is the window in front.

**An agent misbehaves under Brood Mind.** Run it without `brood` to compare; Brood Mind never changes
your agents' own configuration. If it only happens with `brood`, please open an
[issue](https://github.com/bnikiforakis/BroodMind-releases/issues).

## Uninstall

**macOS**

1. Quit Brood Mind and drag **Brood Mind** from Applications to the Trash.
2. Remove the `brood` link: `rm ~/.local/bin/brood` (or wherever Setup put it; `which brood` shows it).
3. Optionally remove its data (saved sessions, `decisions.jsonl`):
   `rm -r ~/Library/Application\ Support/io.github.bnikiforakis.broodmind`

**Linux**

1. `.deb`: `sudo apt remove brood-mind` (`dpkg -l | grep -i brood` shows the package's name).
   AppImage: delete the AppImage, and the `brood` copy it installed (`rm ~/.local/bin/brood`, or see
   `which brood`).
2. Optionally remove its data: `rm -r ~/.local/share/io.github.bnikiforakis.broodmind`

Your agents, their settings and their past sessions are untouched either way.

## License

Brood Mind is free to download and use, for any purpose, personal or commercial. It is proprietary
software, not open source: see [LICENSE](LICENSE) for the terms. The open-source components it includes
are listed, with their licenses, in [THIRD_PARTY_LICENSES](THIRD_PARTY_LICENSES). Agent and terminal
logos come from [Simple Icons](https://simpleicons.org) (CC0) and are trademarks of their owners.
