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

## Part 3: Point spareatree.com at it

1. Still in Railway, on your service, go to **Settings → Networking**.
2. Click **+ Custom Domain**.
3. Type `spareatree.com` and click **Add**. Do the same again for `www.spareatree.com` right after — adding both means the site works whether someone types "www" or not.
4. For each one, Railway shows a target value like `xxxxx.up.railway.app` — keep this tab open.
5. Open a new tab and log into whichever site you bought **spareatree.com** through, then find its **DNS settings** (sometimes called "DNS Management" or "Manage DNS").
6. For `www.spareatree.com`: add a **CNAME record** — Host/Name: `www`, Value/Target: the railway.app value Railway gave you for that one.
7. For the bare `spareatree.com` (no "www"): a plain CNAME record often isn't allowed on the root domain. Look for an option called **ALIAS**, **ANAME**, or "CNAME flattening" and use that instead, pointed at the same railway.app value. If your registrar doesn't offer one of those, it may instead let you forward/redirect the bare domain to `www.spareatree.com` — that works too.
8. Save your DNS changes. They can take anywhere from a few minutes to a few hours to fully activate.
9. Once active, Railway automatically issues the HTTPS padlock — nothing else to do.

**Tell me who you bought spareatree.com through** (GoDaddy, Namecheap, Cloudflare, Squarespace, etc.) and I'll give you the exact click-by-click path for step 7 instead of the generic version above.

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
