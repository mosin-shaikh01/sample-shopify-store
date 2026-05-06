# 🚀 Quick Start: Manual Deployment to Shopify

## In 3 Minutes: Complete Setup

### 1️⃣ Install Theme Access App (Shopify)
```
Shopify Admin → Settings → Apps and integrations → 
Search "Shopify Theme Access" → Add app → Create password
```
**Save the password** (you'll need it for GitHub)

---

### 2️⃣ Add GitHub Secrets (GitHub Repository)
**Go to:** `Settings → Secrets and variables → Actions → New repository secret`

Add these 3 secrets:

```
Secret 1: SHOPIFY_STORE
Value:    yourstore.myshopify.com

Secret 2: SHOPIFY_THEME_ID
Value:    123456789012  (your development theme ID from Shopify)

Secret 3: SHOPIFY_THEME_PASSWORD
Value:    shptka_xxxxx  (Theme Access password from step 1)
```

---

### 3️⃣ Deploy (Whenever You Want)
**Go to:** `GitHub → Actions → Manual Deploy to Shopify Dev Theme → Run workflow`
1. Type: `DEPLOY`
2. Click: **Run workflow**
3. Wait for ✅ green checkmark
4. View preview URL in logs

---

## 📋 GitHub Secrets Reference

| Secret | What It Is | Where to Get It |
|--------|-----------|-----------------|
| `SHOPIFY_STORE` | Your store URL | Shopify Admin → Settings → Store details |
| `SHOPIFY_THEME_ID` | Dev theme ID | Shopify Admin → Themes → Your dev theme → Copy from URL |
| `SHOPIFY_THEME_PASSWORD` | Theme Access auth | Shopify Theme Access app → Create password |

---

## 🔗 After Deployment: Preview URLs

**Development Theme Preview:**
```
https://yourstore.myshopify.com/cdn/shop/t/{THEME_ID}/preview
```

**Theme Editor (to customize):**
```
https://yourstore.myshopify.com/admin/themes/{THEME_ID}/editor
```

*Replace `{THEME_ID}` with your actual theme ID and `yourstore.myshopify.com` with your actual store URL*

---

## ⚡ Safety Checklist

- ✅ Manual trigger only (no automatic deployments)
- ✅ Development theme only (never live theme)
- ✅ `--nodelete` enabled (no accidental deletions)
- ✅ `config/settings_data.json` excluded (preserves Theme Editor changes)
- ✅ GitHub Secrets encrypted (credentials safe)
- ✅ Requires "DEPLOY" confirmation (prevents accidents)

---

## 📚 Full Documentation

See `THEME_ACCESS_SETUP.md` for:
- Detailed troubleshooting
- How to generate Theme Access password
- How to find all GitHub Secrets
- How to revert deployments
- Complete workflow explanation

---

## 🎯 One-Line Deployment Process

1. Make code changes locally
2. Push to GitHub `main` branch
3. Go to GitHub Actions
4. Run "Manual Deploy to Shopify Dev Theme" workflow
5. Type "DEPLOY" to confirm
6. Check preview URL in logs
7. View live theme in Shopify
8. Publish to live when ready

**That's it!**

---

## ❌ What Won't Happen (Protected)

- ❌ Automatic deployment on push
- ❌ Deployment to live/published theme
- ❌ File deletions (--nodelete)
- ❌ Theme Editor settings overwritten
- ❌ Deployment without your explicit "DEPLOY" confirmation

---

## 🔄 Workflow Trigger Instructions

**Step-by-step to trigger deployment:**

1. Go to: `https://github.com/YOUR-USERNAME/sample-shopify-store`
2. Click: **Actions** tab (top of page)
3. Left sidebar: Click **"Manual Deploy to Shopify Dev Theme"**
4. Click: **Run workflow** button (right side)
5. Input field appears: Type `DEPLOY`
6. Click: **Run workflow** button (blue)
7. Watch the workflow run (should complete in 1-2 minutes)
8. Scroll down to see preview URLs in logs
9. Click preview URL to see your theme live

---

## 💡 Pro Tips

- **Dry run?** Make small test change first to verify workflow works
- **Issues?** Check GitHub Actions logs for error messages
- **Settings?** Update theme settings only in Shopify Theme Editor, not in code
- **Rolling back?** Revert commits on GitHub and re-run workflow
- **Team?** Only you trigger deployments—no auto-deploy surprises

---

## 📖 Need Help?

- GitHub Actions errors → Check workflow logs in GitHub
- Theme Access issues → See THEME_ACCESS_SETUP.md
- Deployment failed → Verify all 3 GitHub Secrets are set
- Preview not showing → Wait 30 seconds, clear cache, hard refresh

---

**Ready to deploy? Go to GitHub Actions and run the workflow! 🚀**
