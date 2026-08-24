# Getting Snr. Buns "Spare-A-Tree" Live on spareatree.com

Since you already have git set up in Command Prompt, this uses that instead of dragging files in a browser. Three parts:

1. Push your website files to GitHub
2. Tell Railway to host them
3. Point **spareatree.com** at Railway

---

## Part 1: Push the site to GitHub

**A. Create the empty repo on GitHub first**

1. Go to **github.com/new** (log in if it asks).
2. Repository name: `spare-a-tree` (or whatever you like).
3. Keep it **Public**.
4. Leave every checkbox under "Initialize this repository" **unchecked** — no README, no .gitignore, no license. We're pushing files that already exist locally, so an empty repo avoids a conflict.
5. Click **Create repository**. On the next page, copy the URL it shows under "…or push an existing repository from the command line" — it looks like:
   `https://github.com/YOUR-USERNAME/spare-a-tree.git`

**B. Push your local files up to it**

Open **Command Prompt** and run these one at a time:

```
cd "C:\Save A Tree"
git init
git add .
git commit -m "Spare A Tree site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/spare-a-tree.git
git push -u origin main
```

Replace the `git remote add origin ...` line with the actual URL you copied in step A5.

If it pops up a browser window asking you to sign in to GitHub, do that — it's just confirming it's really you. If it asks for a **username and password** in the terminal itself instead, note that GitHub no longer accepts your regular password there — it needs a Personal Access Token. Send me a screenshot if you hit that and I'll walk you through generating one.

Once `git push` finishes without errors, refresh the GitHub repo page in your browser — you should see `index.html`, `snr-buns-logo.png`, and `tree-scene.gif` all sitting there.

---

## Part 2: Deploy it on Railway

1. Go to **railway.com** and click **Login** (top-right).
2. Choose **Login with GitHub** — one click, no new password. Approve the connection if asked.
3. Click **New Project**.
4. Choose **Deploy from GitHub repo**.
5. Pick the repo you just pushed — `spare-a-tree`.
6. Railway will notice it's a plain HTML site and deploy it automatically — no settings to touch. Wait about a minute.
7. Click into the deployed service → **Settings** tab → under **Networking** click **Generate Domain**. You'll get a free test link like `spare-a-tree-production.up.railway.app` — click it to confirm the site is actually live before moving to the domain step.

**Money note:** new Railway accounts get a 30-day trial with $5 of free credit, no card required. A one-page site like this barely uses any of it — you'll likely never pay. If you ever do run out, their cheapest plan is $5/month.

---

## Part 3: Point spareatree.com at it (GoDaddy)

GoDaddy specifically **does not allow a CNAME record on the bare root domain** (`spareatree.com` with no "www"). So the plan is: the real connection to Railway happens on `www.spareatree.com`, and the bare `spareatree.com` gets redirected to `www.spareatree.com` using GoDaddy's own forwarding feature. End result: both work.

**A. Get the target from Railway**

1. In Railway, on your service, go to **Settings → Networking**.
2. Click **+ Custom Domain**.
3. Type `www.spareatree.com` and click **Add**. (Just this one — not the bare domain.)
4. Railway shows a target value that looks like `xxxxx.up.railway.app`. Keep this tab open.

**B. Add the CNAME record in GoDaddy**

1. Go to **dcc.godaddy.com** and sign in.
2. Click on **spareatree.com** to open its domain settings.
3. Click the **DNS** tab.
4. Click **Add New Record**.
5. Set:
   - **Type:** `CNAME`
   - **Name:** `www`
   - **Value:** the `xxxxx.up.railway.app` value from Railway (paste it without `https://`)
   - Leave **TTL** at its default.
6. Click **Save**.

**C. Forward the bare domain to www**

1. Still on the **DNS** tab for spareatree.com, click **Forwarding** (near the top, next to DNS Records).
2. Click **Add Forwarding**.
3. Forward type: **Domain**.
4. Forward to: `www.spareatree.com`
5. Protocol: **https://**
6. Forward type: **Permanent (301)**
7. Leave masking off (masking hides the real URL in the address bar — you don't want that here).
8. Click **Save**.

That's it. `www.spareatree.com` connects straight to Railway via the CNAME, and typing plain `spareatree.com` redirects visitors to `www.spareatree.com` automatically.

DNS changes can take anywhere from a few minutes to a few hours to fully activate. Once active, Railway automatically issues the HTTPS padlock for `www.spareatree.com` — nothing else to do.

---

## Updating the site later

1. Tell me what you want changed, or edit the files yourself.
2. Save the updated file(s) into **C:\Save A Tree**.
3. In Command Prompt:
   ```
   cd "C:\Save A Tree"
   git add .
   git commit -m "update"
   git push
   ```
4. Railway notices the new push and redeploys automatically within a minute or two — no need to touch Railway or your DNS again.

---

## If something goes wrong

Screenshot whatever you're seeing (including any red error text in Command Prompt) and send it to me — I'll tell you exactly what to do next.
