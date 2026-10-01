# Publishing an HTML file with GitHub Pages

This guide turns a single HTML file (for example, the Encounter Tracker) into a live web page at an address like:

```
https://YOUR-USERNAME.github.io/encounter-tracker/
```

GitHub Pages is free for public repositories. Private repositories can use Pages only on a paid plan (Pro, Team or Enterprise).

---

## Before you start

1. **A GitHub account.** Sign up at https://github.com/signup if you don't have one.
2. **Name the file `index.html`.** GitHub Pages serves `index.html` as the front page of the site. Other HTML files get their own address based on their file name. The Encounter Tracker comes as two files that are already named correctly:
   - `index.html` is the tracker you run the game from.
   - `display.html` is the read-only screen for a TV, tablet or phone.
3. **Keep the file self-contained.** Everything the page needs should be inside the one HTML file, or in files you upload alongside it. Links to other websites, such as Google Fonts, work fine.

---

## Option A: Upload through the website (no software needed)

### 1. Create the repository

1. Go to https://github.com/new
2. **Repository name:** `encounter-tracker`. This becomes part of the web address, so use lowercase letters and hyphens.
3. Set it to **Public**.
4. Tick **Add a README file**, so the repository isn't empty.
5. Click **Create repository**.

### 2. Upload the HTML file

1. On the repository page, click **Add file**, then **Upload files**.
2. Drag `index.html` and `display.html` onto the page together, or click **choose your files** and select both.
3. In the **Commit changes** box at the bottom, type a short note such as `Add encounter tracker`.
4. Leave **Commit directly to the main branch** selected.
5. Click **Commit changes**.

### 3. Turn on GitHub Pages

1. In the repository, click **Settings** (the tab with the gear icon).
2. In the left sidebar, under **Code and automation**, click **Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
4. Under **Branch**, choose **main**, leave the folder as **/ (root)**, and click **Save**.

### 4. Open the page

Wait about a minute, then refresh the **Pages** settings screen. A banner appears at the top reading **Your site is live at** followed by the address. Click **Visit site**.

To watch the deployment happen, open the **Actions** tab. A workflow called **pages build and deployment** runs each time you publish. A green check means it's done, and a red X means it failed (click it to see why).

---

## Option B: Use Git from the command line

Use this if you already have Git installed or plan to update the page often. Check whether Git is installed:

```bash
git --version
```

If that prints a version number, you're set. If not, install it from https://git-scm.com/downloads

### 1. Create an empty repository on GitHub

1. Go to https://github.com/new
2. **Repository name:** `encounter-tracker`
3. Set it to **Public**.
4. Leave **Add a README file** unticked, so the repository starts empty.
5. Click **Create repository**.

### 2. Push your file from your computer

Open a terminal (Terminal on macOS, PowerShell or Git Bash on Windows). Run these commands one at a time, replacing `YOUR-USERNAME` with your GitHub username and the folder path with wherever your file lives:

```bash
cd ~/Documents/encounter-tracker
git init
git branch -M main
git add index.html display.html
git commit -m "Add encounter tracker"
git remote add origin https://github.com/YOUR-USERNAME/encounter-tracker.git
git push -u origin main
```

The first push asks you to sign in to GitHub. A browser window usually opens to handle it. If you're asked for a password in the terminal instead, your normal GitHub password won't work. Create a personal access token at https://github.com/settings/tokens and paste that in its place.

### 3. Turn on GitHub Pages

Follow **Option A, step 3** above: **Settings**, then **Pages**, then **Deploy from a branch**, then branch **main**, folder **/ (root)**, then **Save**.

---

## Updating the page later

**Through the website:** open the repository, click **Add file**, then **Upload files**, and upload the new `index.html` and `display.html`. They replace the old files with the same names. Commit the change.

**With Git:** replace `index.html` in your folder, then run:

```bash
cd ~/Documents/encounter-tracker
git add index.html display.html
git commit -m "Update encounter tracker"
git push
```

Either way, the live page updates within a minute or two. If you still see the old version, force a fresh copy with **Ctrl + Shift + R** (Windows or Linux) or **Cmd + Shift + R** (macOS).

---

## Hosting more than one page in the same repository

Any other HTML file in the repository gets its own address. For example, if you upload both versions of the tracker:

```
index.html     →  https://YOUR-USERNAME.github.io/encounter-tracker/
display.html   →  https://YOUR-USERNAME.github.io/encounter-tracker/display.html
```

---

## Live display on other screens

The tracker can mirror the turn order and timer to any number of other screens (a TV, tablets, players' phones) as it runs. Only the device running `index.html` controls the game. The display pages just watch.

### Going live

1. Open `https://YOUR-USERNAME.github.io/encounter-tracker/` on the device you run the game from.
2. Click the **Live display off** button under the round counter, or **Set up** in the **Live display** panel on the setup screen.
3. Click **Go live**. The status turns green and reads **Live, no screens watching**, and a QR code and room code appear.

### Connecting a screen

Use any one of these:

- **Scan the QR code** with a phone or tablet camera.
- **Open the link** shown under the room code (click **Copy link** to send it to someone).
- **Type it in:** go to `https://YOUR-USERNAME.github.io/encounter-tracker/display.html` and enter the room code.

Each screen that connects bumps the count on the tracker, for example **Live: K7QM2X, 2 watching**.

On the display page:

- **Full screen** hides the browser bars, which is best on a TV.
- **Sound off / Sound on** plays the time-out buzzer on that screen. Browsers block sound until you tap something, which is why this starts off.
- Tap the screen once and it stays awake while the display is open, on devices that support it.
- **Room** switches to a different room code.

The display remembers its last room, so reopening `display.html` on the same device reconnects on its own.

### How it works, and its limits

GitHub Pages only hosts files, so the pages pass updates through a free public relay server using the MQTT messaging protocol. There's no account or setup. Things to know:

- **It needs internet on every device.** If the relay drops, the tracker keeps working normally, and displays show **The tracker is offline. Showing its last update.** until it's back.
- **Anyone with the room code can watch.** Codes are six random characters, so nobody will stumble onto yours, but don't put anything private in combatant names. If a code gets passed around, click **New room code** (two taps) and reconnect your screens.
- **The relays are free test services** run by HiveMQ and EMQX, with no uptime guarantee. If screens won't connect, open **Live display** on the tracker, switch **Relay server** to the other one, and reconnect the screens using the new link or QR code. The relay choice travels in the link, so screens opened from the link or QR code always match the tracker.
- **Timers stay within about a second** across screens. Each display counts down on its own and corrects itself from a heartbeat the tracker sends every second.
- **It only works from GitHub Pages (or another web host).** The copy published on claude.ai can't reach outside servers, so live sync won't connect there.

## Notes specific to the Encounter Tracker

- **Saving the session log works as a normal download.** Outside claude.ai, "Save session log" downloads the `.json` file straight to the browser's Downloads folder, with no confirmation prompt.
- **The log is stored per browser, per device.** A session log started on the tablet stays on the tablet. The log you built up on claude.ai does not carry over to the GitHub version. Save it from claude.ai first if you want to keep it.
- **Two versions on the same account share one log.** Every GitHub Pages site under `YOUR-USERNAME.github.io` counts as the same website to the browser. If you publish both tracker versions, even in different repositories, they read and write the same session log. Use only one at the table, or click **Start a new log** when you switch.
- **Put it on a tablet's home screen.** Open the page in the tablet's browser and choose **Add to Home Screen** (Safari: the Share button; Chrome: the ⋮ menu). It then opens like an app.

---

## Troubleshooting

| What you see | Likely cause and fix |
| --- | --- |
| **404 "There isn't a GitHub Pages site here"** | The site may still be deploying, so wait two minutes and check the **Actions** tab. If it's still missing, confirm the file is named exactly `index.html` in lowercase, and that Pages is set to branch **main**, folder **/ (root)**. |
| **The Pages settings won't let you pick a branch** | The repository is private on a free account, or it's empty. Make it public (**Settings**, then **General**, then **Danger Zone**, then **Change visibility**) and make sure at least one file is committed. |
| **The page loads but looks unstyled or broken** | Something the page links to isn't in the repository. Upload any image, CSS or script files it references, with the same folder structure. |
| **Changes don't show up** | Hard refresh (**Ctrl/Cmd + Shift + R**) and check that the latest run in **Actions** has a green check. |
| **A display stays on "Connecting" or "Offline, retrying"** | Some venue, school or office networks block the relay's port. Switch relays in **Live display** on the tracker and reconnect the screens, or try a phone hotspot to confirm it's the network. |
| **A display shows "Waiting for the tracker"** | The tracker hasn't clicked **Go live**, or the display is using a different room code or relay. Reconnect with the tracker's current QR code or link. |
| **`git push` is rejected with "fetch first"** | The GitHub repository has a file your computer doesn't, often a README made during setup. Run `git pull --rebase origin main`, then `git push` again. |

---

## Optional: use your own domain

To serve the page from an address like `tracker.example.com`:

1. Go to **Settings**, then **Pages**, then **Custom domain**. Enter the domain and click **Save**.
2. At your domain registrar, add a **CNAME** record pointing `tracker` to `YOUR-USERNAME.github.io`.
3. Once the domain check passes, tick **Enforce HTTPS**.

DNS changes can take anywhere from a few minutes to a few hours to take effect.
