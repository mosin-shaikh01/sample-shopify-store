# Shopify Deployment System

## Overview
This project uses GitHub Actions to deploy changes to a **development theme only** on Shopify. Deployments are **manual and explicitly triggered** — no automatic deployments occur on push or pull requests.

## Prerequisites Setup

### 1. GitHub Secrets Configuration
Add these secrets to your GitHub repository (Settings → Secrets and variables → Actions):

| Secret Name | Value | How to Find |
|------------|-------|-----------|
| `SHOPIFY_STORE_URL` | Your store URL (e.g., `my-store.myshopify.com`) | Shopify Admin → Settings |
| `SHOPIFY_THEME_ID` | Development theme ID | Shopify Admin → Online Store → Themes → Development → Copy theme ID from URL or settings |
| `SHOPIFY_CLI_THEME_TOKEN` | Admin API access token | Shopify Partner Dashboard → Create custom app → Admin API scopes: `write_themes`, `read_themes` |

### 2. Shopify Theme Setup
- **Development Theme Required**: Must be an unpublished (development) theme, never the live theme
- Create a new theme in Shopify Admin: Online Store → Themes → Add theme
- Keep this theme for development/preview only
- Copy the theme ID to the GitHub Secret `SHOPIFY_THEME_ID`

### 3. GitHub Repository Configuration
- Ensure Actions are enabled: Settings → Actions → General → Workflow permissions
- Grant write permissions if needed for deployment logs

## Deployment Process

### How to Deploy
When you want to deploy changes from GitHub main to the development theme on Shopify:

1. Go to GitHub repository
2. Click **Actions** tab
3. Select **"Deploy to Shopify Dev Theme"** workflow
4. Click **"Run workflow"** button
5. In the dialog, enter `DEPLOY` in the confirmation field
6. Click **"Run workflow"**

**The workflow will:**
- Deploy all changes from the current `main` branch
- Exclude `config/settings_data.json` (preserves Theme Editor changes)
- Use `--nodelete` flag (prevents accidental file deletions)
- Target the development theme only
- Display preview URL on completion

### Preview Your Changes
After deployment succeeds:
- Development theme preview: `https://your-store.myshopify.com/cdn/shop/t/{THEME_ID}/preview`
- Theme Editor: Shopify Admin → Online Store → Themes → Development theme → Customize

## Safety Guarantees

✅ **Manual Only**: No automatic deployment on push or PR  
✅ **Dev Theme Only**: Never targets the live/published theme  
✅ **Reversible**: Changes are never destructive (--nodelete flag)  
✅ **Safe Merge**: Theme Editor changes preserved (settings_data.json ignored)  
✅ **Explicit Control**: You decide when to deploy  
✅ **Audit Trail**: All deployments logged in GitHub Actions

## Rollback
If deployment causes issues:
1. Revert changes in GitHub or theme files
2. Run the workflow again with corrected code
3. Or manually revert in Shopify Admin Theme Editor

## Troubleshooting

**"Authentication failed"**
- Verify GitHub Secrets are correctly set
- Check API token has `write_themes`, `read_themes` scopes

**"Theme not found"**
- Verify `SHOPIFY_THEME_ID` is correct and development theme exists
- Check theme ID in Shopify Admin URL when viewing theme

**"settings_data.json conflicts"**
- This file is intentionally ignored to preserve Theme Editor changes
- Update it only through Shopify Admin, not GitHub

## Files Involved
- `.github/workflows/deploy-to-shopify.yml` - GitHub Actions workflow
- `shopify.app.json` - Shopify CLI configuration
- `.env.example` - Environment variable documentation
