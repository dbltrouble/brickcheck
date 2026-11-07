# BrickCheck setup guide

This is a one-time setup. Plan on about 45 minutes, and do it on your MacBook, not your phone. You don't need to know anything about coding. You'll be copying, pasting, and clicking buttons.

There are four parts:

1. **Rebrickable:** get a free key that lets the app read Rebrickable's set inventories.
2. **Cloudflare:** set up the free "relay," a small program that runs on Cloudflare's servers. It fetches part lists for the app and stores your saved inventories so your devices stay in sync.
3. **GitHub:** put the app online so you can open it from any device.
4. **Your devices:** connect each device to the relay.

## Before you start

Have these three files saved somewhere easy to find, like a folder on your Desktop called `BrickCheck`:

- `index.html` (the app)
- `worker.txt` (the relay)
- `SETUP.md` (this guide)

Open the Notes app and create a new note called **BrickCheck keys**. You'll collect four values along the way, and pasting each one into this note as you get it keeps you from losing track. Keep the note private, because these values are like passwords.

When you copy a value, make sure you don't pick up extra spaces at the start or end. Stray spaces are the most common cause of problems later.

## Part 1: Rebrickable API key (about 5 minutes)

1. In your browser, go to **rebrickable.com** and sign in. If you don't have an account, click **Sign Up** and create a free one.
2. Click your username or profile picture in the top-right corner, then click **Settings**.
3. Click the **API** tab.
4. Click **Generate new API Key**. If it asks for a description, type `BrickCheck`.
5. A long string of letters and numbers appears. Copy it into your note, labeled **Rebrickable key**.

## Part 2: Cloudflare relay (about 20 minutes)

Cloudflare's website changes its layout from time to time. If a button isn't named exactly as written here, look for the closest match.

### 2a. Create a Cloudflare account

1. Go to **dash.cloudflare.com** and click **Sign up**.
2. Enter your email and a password, then confirm your email from the message Cloudflare sends.
3. If Cloudflare asks what you want to do or offers a paid plan, choose the **Free** plan. If it asks you to add a website or domain, skip that step. You don't need one.

### 2b. Create the relay

1. In the left sidebar, click **Workers & Pages**. You may need to click **Compute** first to see it.
2. Click **Create** (or **Create application**).
3. Choose **Start with Hello World!**
4. Cloudflare suggests a random name. Change it to `brickcheck-relay`.
5. Click **Deploy**. Cloudflare creates a simple test program. That's expected; you'll replace it next.
6. Click **Edit code**. A code editor opens with a file called `worker.js` (or similar) showing a few lines of code.
7. Click inside the code, press **Command-A** to select everything, then press **Delete**. The editor should be empty.
8. On your Mac, open your `worker.txt` file in TextEdit. If it opens as formatted text, choose **Format → Make Plain Text**. Press **Command-A**, then **Command-C** to copy it all.
9. Back in the Cloudflare editor, click inside it and press **Command-V** to paste.
10. Click **Deploy** (top right). Wait for a message saying it was deployed.
11. Click the back arrow, or click `brickcheck-relay` at the top, to return to the relay's main page.

### 2c. Create the storage for syncing

1. In the left sidebar, find **Storage & Databases**, then click **KV** (it may be called **Workers KV**).
2. Click **Create** (or **Create instance** / **Create namespace**).
3. Name it `brickcheck-sync` and click **Create** (or **Add**).

### 2d. Connect the storage to the relay

1. In the left sidebar, click **Workers & Pages** again, then click `brickcheck-relay`.
2. Click the **Settings** tab.
3. Find the **Bindings** section and click **Add** (or **Add binding**).
4. Choose **KV namespace**.
5. In **Variable name**, type exactly `SYNC`, in capital letters.
6. In the namespace dropdown, choose `brickcheck-sync`.
7. Click **Add binding** (or **Save** / **Deploy**).

### 2e. Make up your app key

The app key is a password you invent. It keeps anyone else from using your relay. Use letters and numbers only, with no spaces or symbols, and make it at least 20 characters. A few random words run together plus some numbers works well, for example `bluegravelotterpine8842`. Don't use that example; make up your own.

Paste it into your note, labeled **App key**. You now have two values.

### 2f. Add your secret values to the relay

You'll add two secrets, one at a time. They're stored privately on Cloudflare and can't be seen once saved.

1. Still in the relay's **Settings** tab, find **Variables and Secrets** and click **Add**.
2. Set **Type** to **Secret**.
3. In **Variable name**, type the name exactly as shown below, in capital letters with underscores.
4. In **Value**, paste the matching value from your note.
5. Repeat for both:

| Variable name | Value from your note |
|---|---|
| `RB_KEY` | Rebrickable key |
| `APP_KEY` | App key |

6. Click **Deploy** (or **Save**). If Cloudflare asks whether to deploy the changes, say yes.

### 2g. Copy the relay's address

1. On the relay's main page, look for its address. It ends in `.workers.dev` and looks like `https://brickcheck-relay.yourname.workers.dev`.
2. Copy it into your note, labeled **Relay URL**. You now have three values.
3. To check it's working, paste the address into a new browser tab and press Enter. You should see a short line containing `Wrong app key`. That's correct: the relay is running and correctly refusing visitors who don't have your key.

## Part 3: Put the app on GitHub (about 10 minutes)

GitHub stores files online, and its free **GitHub Pages** feature turns a file like `index.html` into a website you can open anywhere. Here's the vocabulary you'll see:

- A **repository** (or **repo**) is just a folder on GitHub.
- **Commit** means "save this change."

### 3a. Sign in

1. Go to **github.com** and sign in with the account you used for the Kingdomino tracker. If you can't remember it, use **Forgot password**, or create a new free account with **Sign up**.

### 3b. Create a repository

1. Click the **+** in the top-right corner and choose **New repository**.
2. In **Repository name**, type `brickcheck` in lowercase.
3. Choose **Public**. GitHub Pages is free only for public repositories. Your inventories and keys are not stored here; this folder holds only the app's code.
4. Check **Add a README file**. This makes the repository easier to work with.
5. Click **Create repository**.

### 3c. Upload the files

1. On your new repository's page, click **Add file**, then **Upload files**.
2. Drag `index.html`, `worker.txt`, and `SETUP.md` from your Mac into the box on the page. Or click **choose your files** and select them.
3. Wait until all three appear in the list below the box.
4. Scroll down and click the green **Commit changes** button.
5. You should see the three files listed on the repository's page.

The file must be named exactly `index.html`, all lowercase. If your Mac renamed it (for example `index (1).html`), rename it before uploading.

### 3d. Turn on GitHub Pages

1. On the repository's page, click **Settings** (the tab with the gear icon, near the top).
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
4. Under **Branch**, choose `main`, leave the folder as `/ (root)`, and click **Save**.
5. Wait one or two minutes, then refresh the page. A box at the top will say your site is live and show its address, something like `https://yourname.github.io/brickcheck/`.
6. Copy that address into your note, labeled **App address**.
7. Click the address. The BrickCheck app should open.

## Part 4: Connect each device

Do this once on your MacBook, iPhone, and iPad.

1. Open the app address in your browser (Safari or Chrome).
2. Tap **Settings** in the top right.
3. Paste your **Relay URL** into the first box and your **App key** into the second.
4. Under **Part data source**, make sure **Rebrickable** is selected.
5. Tap **Save and test**. You should see **Connected to Rebrickable. Sync is on.** If you see an error, check the troubleshooting list below.
6. Tap **Back**.
7. **On iPhone and iPad only:** tap the **Share** button (the square with an arrow pointing up), scroll down, and tap **Add to Home Screen**, then **Add**. From now on, open BrickCheck from that home screen icon. This matters: Safari can erase saved data for websites you don't visit for a while, but home screen apps are protected.

To get the values onto your phone and iPad, open the **BrickCheck keys** note there. It syncs through iCloud if your Notes use iCloud.

### Try it

On any device, enter a small set number to test, tap **Start inventory**, and mark a couple of parts. Then open the app on another device. Within about a minute, after pulling down to refresh or reopening the app, you should see the same marks.

## Troubleshooting

When you tap **Save and test**, these are the most likely messages and fixes:

- **"Could not reach the relay"**: the Relay URL is wrong. Copy it again from Cloudflare. It must start with `https://`.
- **"The relay rejected the app key"**: the app key in the app doesn't exactly match the `APP_KEY` secret on Cloudflare. Check for typos or extra spaces. If you're unsure, add the `APP_KEY` secret again on Cloudflare with the value from your note, deploy, and try again.
- **"The relay doesn't have a Rebrickable key yet"**: the `RB_KEY` secret is missing or misspelled. Add it again in step 2f, deploy, and try again.
- **"Rebrickable:" followed by a message about an invalid key**: the Rebrickable key was pasted wrong. Copy it again from Rebrickable's API tab and replace the `RB_KEY` secret.
- **A message mentioning "IP"** (BrickLink only): the BrickLink access token wasn't set to `0.0.0.0`. Create a new token on BrickLink with `0.0.0.0`, then replace `BL_TOKEN` and `BL_TOKEN_SECRET` on Cloudflare.
- **A message mentioning "signature" or "consumer"** (BrickLink only): one of the four BrickLink values on Cloudflare was pasted wrong. Add all four again carefully, deploy, and try again.
- **"The relay has no KV namespace bound as SYNC"**: step 2d didn't save. Repeat it, and make sure the variable name is exactly `SYNC`.

If something else goes wrong, take a screenshot and send it to Claude.

## Updating the app later

When Claude gives you a new version of `index.html`:

1. Go to your `brickcheck` repository on GitHub.
2. Click **Add file**, then **Upload files**, and drag in the new `index.html`. It replaces the old one because the name is the same.
3. Click **Commit changes**.
4. Wait a minute or two, then reload the app. On iPhone or iPad, close the app fully and reopen it.

When Claude gives you a new `worker.txt`, open the relay on Cloudflare, click **Edit code**, select everything and delete it, paste the new code, and click **Deploy**. Your secrets and saved inventories stay in place.

## Adding BrickLink later

BrickLink adds part values (6-month average prices, new and used) and uses BrickLink's own inventories. Once BrickLink is back online:

1. Go to **https://www.bricklink.com/v2/api/register_consumer.page** while signed in to BrickLink. Accept the API terms if asked. If it asks you to set up seller or store information first, stop and let Claude know.
2. Register a new consumer. Copy the **Consumer Key** and **Consumer Secret** into your note.
3. Add a new access token. For the **IP address**, type `0.0.0.0`, which means "allow from anywhere." Cloudflare's servers don't have one fixed address. Copy the **Token Value** and **Token Secret** into your note.
4. On Cloudflare, open `brickcheck-relay`, go to **Settings → Variables and Secrets**, and add four more secrets, the same way as step 2f:

| Variable name | Value from your note |
|---|---|
| `BL_CONSUMER_KEY` | Consumer Key |
| `BL_CONSUMER_SECRET` | Consumer Secret |
| `BL_TOKEN` | Token Value |
| `BL_TOKEN_SECRET` | Token Secret |

5. Click **Deploy**.
6. In the app on each device, go to **Settings**, choose **BrickLink** under **Part data source**, and tap **Save and test**. You should see **Connected to BrickLink. Sync is on.**

New inventories will then use BrickLink. Inventories you already started keep their original source.

## Limits to know

- **Rebrickable:** limits how quickly requests can be made. Loading a set takes only a handful of requests, plus one per minifig. If Rebrickable asks the app to slow down, it pauses briefly and retries on its own.
- **BrickLink: 5,000 requests per day.** Prices cost 2 requests per *unique part and color* (one for new, one for used), not per piece. If you hit the limit, the app stops loading prices, keeps everything you've done, and picks up where it left off when you reopen the inventory the next day. Prices are reused for 14 days. Checking parts never uses BrickLink requests, so you can keep inventorying while prices load.
- **Cloudflare: 1,000 sync saves per day on the free plan.** The app saves to the relay at most every 15 seconds while you tap, which works out to roughly 100 to 200 saves for an hour of inventorying.
