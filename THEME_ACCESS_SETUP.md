# Shopify Theme Access Deployment Setup

## Overview
This deployment system uses **Shopify Theme Access** (password-based authentication) instead of Admin API tokens. This provides a more lightweight, theme-specific approach to deployments.

**Key Features:**
- ✅ Manual deployment only (no automatic triggers)
- ✅ Development theme only (never live theme)
- ✅ Uses Shopify Theme Access app for authentication
- ✅ Safe deployment with `--nodelete` and settings_data.json excluded
- ✅ Explicit GitHub user control required

---

## Prerequisites: Create Shopify Theme Access

### Step 1: Install Shopify Theme Access App
1. Go to your Shopify Store Admin
2. **Settings** → **Apps and integrations** → **App and sales channel settings**
3. Search for **"Shopify Theme Access"** (official Shopify app)
4. Click **Add app**
5. Grant required permissions
6. Once installed, go to **Settings** → **Apps and integrations** → **Installed apps**
7. Click on **Shopify Theme Access**

### Step 2: Generate Theme Access Password
1. In the Shopify Theme Access app dashboard
2. Click **Create password** or **Generate password**
3. Give it a name: `GitHub Deployment`
4. Copy the generated **password** (this is your `SHOPIFY_THEME_PASSWORD`)
5. ⚠️ **Save it securely** — you won't see it again

---

## GitHub Secrets Configuration

Add these 3 secrets to your GitHub repository:

**Settings** → **Secrets and variables** → **Actions** → **New repository secret**

| Secret Name | Value | Example |
|---|---|---|
| `SHOPIFY_STORE` | Your store URL | `my-store.myshopify.com` |
| `SHOPIFY_THEME_ID` | Development theme ID | `123456789012` |
| `SHOPIFY_THEME_PASSWORD` | Theme Access password | `shptka_abc123...` |

### How to Find Each Value:

**`SHOPIFY_STORE`**
- Shopify Admin → Settings → Store details
- Format: `yourstore.myshopify.com`

**`SHOPIFY_THEME_ID`**
- Shopify Admin → Online Store → Themes
- Click on your development theme
- Copy the theme ID from the URL: `https://yourstore.myshopify.com/admin/themes/{THEME_ID}`

**`SHOPIFY_THEME_PASSWORD`**
- From Shopify Theme Access app (see Step 2 above)
- Format: `shptka_...`

---

## Shopify Setup Requirements

### Development Theme
- Must have a **development (unpublished) theme** in Shopify
- This theme is used for preview/testing only
- Never deploy to the live/published theme
- To create one:
  1. Shopify Admin → Online Store → Themes
  2. Click **Add theme**
  3. Choose "Create blank theme" or "Duplicate existing theme"
  4. Make sure it's **NOT published** (status shows "Unpublished")
  5. Copy its theme ID to `SHOPIFY_THEME_ID` secret

---

## How to Deploy

### Trigger Deployment from GitHub UI

**When you want to deploy changes to your development theme:**

1. Go to your GitHub repository
2. Click **Actions** tab
3. Select **"Manual Deploy to Shopify Dev Theme"** workflow (left sidebar)
4. Click **Run workflow** button
5. A dialog appears with a confirmation field
6. Type exactly: `DEPLOY`
7. Click **Run workflow** button

**⏱️ Deployment will:**
- Start immediately
- Pull code from `main` branch
- Deploy to your development theme
- Exclude `config/settings_data.json` (preserves Theme Editor changes)
- Use `--nodelete` flag (prevents accidental file loss)
- Display preview URL on completion

### Monitor Deployment

1. Stay on the Actions page
2. Watch the workflow run in real-time
3. Green checkmark = successful deployment
4. Red X = deployment failed (check logs)

---

## Preview Your Changes

After successful deployment:

### Option 1: Development Theme Preview
- Direct URL: `https://yourstore.myshopify.com/cdn/shop/t/{THEME_ID}/preview`
- *(Replace {THEME_ID} with your actual theme ID)*
- This shows your development theme in action

### Option 2: Theme Editor
1. Shopify Admin → Online Store → Themes
2. Click your development theme → **Customize**
3. See live changes as customers would see them
4. Test on mobile and desktop

### Option 3: Shared Preview Link (Optional)
1. In Theme Editor, click **Preview** → **Share** 
2. Generate a shareable preview link
3. Send to stakeholders for feedback

---

## Safety & Control

### ✅ What's Protected:

| Protection | How |
|---|---|
| Manual only | `workflow_dispatch` trigger only |
| Dev theme only | `SHOPIFY_THEME_ID` points to unpublished theme |
| No file deletion | `--nodelete` flag active |
| Editor changes safe | `config/settings_data.json` ignored |
| Credentials secure | GitHub Secrets encryption |

### ✅ What You Control:

- When to deploy (you trigger manually)
- What gets deployed (main branch code)
- Where it goes (development theme only)
- Preview before going live (Theme Editor or preview URL)

### ⚠️ Important Rules:

- ❌ Never use `SHOPIFY_THEME_ID` of a published/live theme
- ❌ Don't share Theme Access password publicly
- ❌ Don't commit `SHOPIFY_THEME_PASSWORD` to GitHub
- ✅ Always use GitHub Secrets for credentials
- ✅ Always test in preview before publishing theme

---

## Troubleshooting

### "Authentication failed" or "Invalid password"
- Verify `SHOPIFY_THEME_PASSWORD` is correct in GitHub Secrets
- Check Theme Access app is still installed in Shopify
- Try regenerating password in Theme Access app
- Re-add the new password to GitHub Secrets

### "Theme not found"
- Verify `SHOPIFY_THEME_ID` is correct
- Check theme still exists in Shopify (not deleted)
- Confirm it's an unpublished/development theme

### "Deployment succeeded but changes not showing"
- Wait 30-60 seconds for Shopify to process
- Clear browser cache
- Try hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
- Check Theme Editor to verify files were pushed

### "config/settings_data.json not updated"
- This is intentional! Excluded to preserve Theme Editor changes
- Update theme settings only through Shopify Admin Theme Editor
- Don't modify settings_data.json in code files

---

## Reverting a Deployment

If a deployment causes issues:

**Option 1: Deploy previous version**
1. Revert commits in GitHub (create new commit undoing changes)
2. Run workflow again with corrected code
3. New deployment will overwrite previous version

**Option 2: Manual revert in Shopify**
1. Shopify Admin → Online Store → Themes
2. Click development theme → **Customize**
3. Use Theme Editor's version history or manually fix issues
4. Changes take effect immediately

---

## Files Involved

- `.github/workflows/manual-deploy.yml` — GitHub Actions workflow
- `THEME_ACCESS_SETUP.md` — This setup guide
- `.gitignore` — Prevents committing sensitive files
- `.env.example` — Environment variable documentation

---

## Workflow YAML Features

The `.github/workflows/manual-deploy.yml` includes:

```yaml
on:
  workflow_dispatch:          # Manual trigger only
    inputs:
      confirm:               # Requires "DEPLOY" confirmation
        required: true
```

**Key safety features:**
- No automatic triggers
- Manual confirmation required
- Uses Theme Access authentication (not API tokens)
- Validates credentials before deployment
- Displays preview URLs
- Clear deployment logs
- Error handling

---

## Quick Reference

| Action | Steps |
|--------|-------|
| **Deploy** | Actions → Manual Deploy → Run workflow → Type "DEPLOY" → Run |
| **Preview** | `https://store.myshopify.com/cdn/shop/t/{THEME_ID}/preview` |
| **Edit** | Shopify Admin → Themes → Theme → Customize |
| **Revert** | Revert code in GitHub + re-run workflow |
| **Secrets** | GitHub → Settings → Secrets → Add SHOPIFY_STORE, SHOPIFY_THEME_ID, SHOPIFY_THEME_PASSWORD |

---

## Support

For issues:
1. Check GitHub Actions logs for error messages
2. Verify all 3 GitHub Secrets are set correctly
3. Confirm development theme exists in Shopify
4. Check Theme Access app is installed
5. Test Shopify CLI locally if needed: `shopify theme push --help`
