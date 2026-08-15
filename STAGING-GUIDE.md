# NMAO Membership Platform — STAGING

This is the **staging mirror** of the live app (`NMAO1/NMAO-Membership-Platform` → `app.nmao.us`).
Use it to preview a change **before** real schools and members ever see it.

- **Staging URL:** `https://nmao1.github.io/NMAO-Membership-Platform-staging/` (set after you enable Pages, below)
- **Production URL:** `https://app.nmao.us`
- The only intended difference from prod is that this repo has **no `CNAME`** (so it lives at the github.io URL). Every HTML/JS/CSS file is byte-identical to prod, so "promote" is a clean copy.

---

## ⚠️ Read this first — staging shares the LIVE backend
This staging **frontend** talks to the **same live Supabase database + Stripe** as production.
It is a *preview*, not an isolated sandbox. That means:

- ✅ Safe: looking at layout, wording, navigation, and logic on any page.
- 🚨 NOT safe on real accounts: any button that **writes data** (create student, charge a card,
  cancel, send a message) hits **production data / real money**.
- **Rule:** when testing anything that saves or charges, do it **only inside the `taosd` test school**
  (it's already exempt from the billing gates). Never exercise write-actions against a real school here.

A fully isolated staging (its own Supabase project + Stripe test mode) is the future upgrade; this
preview closes most of the risk for the frontend changes you make day to day.

---

## One-time setup (≈5 min, your part)
1. **GitHub Desktop → File → Add local repository →** choose this folder
   (`~/Documents/GitHub/NMAO-Membership-Platform-staging`).
2. Click **Publish repository**. Keep the name `NMAO-Membership-Platform-staging`.
   Visibility can match prod (the served files are already public at app.nmao.us; the embedded
   Supabase anon key and Stripe publishable key are public-by-design and protected by RLS).
3. On github.com, open the new repo → **Settings → Pages →** Source: **Deploy from a branch**,
   Branch: **`main`**, Folder: **`/ (root)`** → **Save**.
4. Wait ~1 min, then load `https://nmao1.github.io/NMAO-Membership-Platform-staging/dashboard.html`.
   (Optional, later: add a `staging.nmao.us` DNS CNAME + a `CNAME` file here to use a nicer URL.)

---

## Everyday workflow — test on staging, then promote to prod
1. **Edit** the page(s) you're changing.
2. **Put the edited file(s) in THIS staging repo** (drag into GitHub Desktop or upload on github.com),
   commit, and let Pages redeploy (~1 min).
3. **Open the staging URL** and verify the change — including a click-through in the `taosd` test school
   if it touches a data flow.
4. Only once it looks right: **copy the *same* file(s) into the prod repo**
   (`NMAO-Membership-Platform`) and deploy there. Because the files are identical, this is a clean copy —
   nothing staging-specific leaks to prod (the `CNAME` difference is not a file you copy).

**Never** edit prod first. Staging is the dress rehearsal; prod is opening night.
