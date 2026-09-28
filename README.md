# RRT Volunteer Hub

A single web page for RRT coordinators to run volunteer recruitment for each event: the volunteer roster, who has been reached out to, who has agreed, and each event's details (type, RRT support type, times, set-up, volunteers required, people served).

- Live data is stored in Firebase (Firestore) and syncs in real time across every coordinator's phone and computer.
- Only coordinators on the list in `firestore.rules` can sign in and see anything.
- No volunteer names are stored in this repository. They live only in Firebase and in the coordinators' own browsers.

## What's in this repository

| File | What it is |
|---|---|
| `index.html` | The hub itself. GitHub Pages serves it as the website. |
| `firestore.rules` | The security rules that decide who can read and edit the data. |
| `.gitignore` | Stops backup files (which contain names) being uploaded by mistake. |

## One-off setup

Do these in order. The Firebase steps must be finished before coordinators switch to the new link.

### A. Put the hub on GitHub Pages

1. On GitHub, create a new repository called `rrt-volunteer-hub`. It needs to be **Public** to use Pages on a free account. That's fine: the code contains no personal data, and the data itself is protected by sign-in.
2. Open the repository, choose **Add file → Upload files**, drag in `index.html`, `firestore.rules`, `README.md` and `.gitignore`, then **Commit changes**.
3. Go to **Settings → Pages**. Under *Build and deployment*, set *Source* to **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
4. After a minute or two the site is live at `https://YOUR-GITHUB-USERNAME.github.io/rrt-volunteer-hub/`. Note this address.

### B. Update Firebase (project `rrt-volunteer-hub`)

In the Firebase console (console.firebase.google.com):

1. **Turn on Google sign-in.** Authentication → Sign-in method → Add new provider → **Google** → Enable → Save.
2. **Allow the GitHub address.** Authentication → Settings → Authorised domains → Add domain → `YOUR-GITHUB-USERNAME.github.io` (just the domain, no `https://` and no folder).
3. **Publish the coordinator rules.** Open `firestore.rules`, replace the two example emails with each coordinator's Google account email, then paste the whole file into Firestore Database → Rules and press **Publish**.
4. **Turn off anonymous sign-in.** Authentication → Sign-in method → Anonymous → Disable. The old version of the hub used it; the new one doesn't.

Once step 3 is published, old copies of the hub file (sent by email or saved on phones) stop syncing. That's intended: everyone should use the new link.

### C. Share it with coordinators

Send coordinators the GitHub Pages link. Each person:

1. Opens the link and taps **Sign in with Google**, using the account on the list.
2. Adds it to their home screen. iPhone (Safari): Share → Add to Home Screen. Android (Chrome): ⋮ → Add to Home screen.

If someone sees "not on the coordinator list", add their email to `firestore.rules` and publish the rules again.

## Day-to-day

- **Changing the hub:** open `index.html` on GitHub, click the pencil icon, edit, then **Commit changes**. The site updates within a couple of minutes. Or upload a new version with **Add file → Upload files**.
- **Undoing a change:** every version is in the repository's **Commits** history. Open an earlier commit to see or restore the old file.
- **Backups:** use the **Backup** button in the hub regularly and keep the file somewhere private (OneDrive, for example). Never upload backups to this repository. The `.gitignore` blocks `.json` files for that reason.
- **Restoring:** the **Restore** button loads a backup and replaces the data for everyone.

## Security notes

- The Firebase `apiKey` in `index.html` is meant to be public. It identifies the project; it does not grant access. Access is controlled entirely by `firestore.rules` and Google sign-in.
- The page asks search engines not to index it (`noindex`), but treat the link as shareable rather than secret. Sign-in is what protects the data.
- Optional extra: in Google Cloud console → APIs & Services → Credentials, restrict the browser API key to HTTP referrer `YOUR-GITHUB-USERNAME.github.io/*` (and `rrt-volunteer-hub.firebaseapp.com/*`, which Google sign-in uses).
