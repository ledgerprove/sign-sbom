# Publishing `ledgerprove/sign-sbom` to the GitHub Marketplace

This is a one-time setup, then `git push --tags` after each release.

## Prerequisites

- The action lives at https://github.com/ledgerprove/sign-sbom (the `ledgerprove` GitHub org).
- You're an admin of that org and that repo.
- The repo's `dist/index.js` is committed (it's the bundled handler GitHub runs).

## Pre-flight checklist

Before you click "Publish", confirm:

- [ ] `action.yml` `name` is unique on the marketplace (search at https://github.com/marketplace?type=actions). "LedgerProve — Sign SBOM" should be fine.
- [ ] `action.yml` `description` is < 125 chars and explains what the action does.
- [ ] `action.yml` `branding.icon` and `branding.color` are valid Feather icons / GitHub palette values.
- [ ] `README.md` has a Quick start at the top (it does).
- [ ] `LICENSE` file exists at the repo root. Add MIT if missing — see template below.
- [ ] `dist/index.js` is committed and up to date with `src/`.
- [ ] No secrets, tokens, or `.env` files in the repo. Run `gitleaks detect` if unsure.

## Step-by-step publish

1. **Tag the first release** (do this from your terminal, in the action repo):
   ```bash
   cd /path/to/sign-sbom
   git tag v1.0.0 -m "v1.0.0 — initial Marketplace release"
   git tag v1 -m "v1"          # rolling tag — workflows that pin @v1 follow this
   git push origin v1.0.0 v1
   ```

2. **Open the repo on github.com** → **Releases** (right sidebar) → **Draft a new release**.

3. **Configure the release:**
   - Tag: pick `v1.0.0` from the dropdown.
   - Release title: `v1.0.0 — Initial release`
   - Description: paste the README's Quick start + a bullet list of features.
   - **Tick** ✅ "Publish this Action to the GitHub Marketplace".
   - Accept the developer agreement (first time only).
   - Pick a primary category: **Security** (and optionally a secondary like **Continuous integration**).

4. **Click "Publish release"**. The action shows up at:
   `https://github.com/marketplace/actions/ledgerprove-sign-sbom`

   GitHub takes a few minutes to render the page. Refresh until it appears.

## After publishing — keep `v1` rolling

When you ship a backwards-compatible bug fix (say `v1.0.1`):

```bash
git tag v1.0.1 -m "v1.0.1 — fix X"
git tag -f v1                  # move v1 to the new commit
git push origin v1.0.1
git push origin v1 --force     # update the rolling tag
```

Users pinning `@v1` automatically get the fix on their next CI run. Users pinning `@v1.0.0` stay on the exact version.

For a breaking change, cut a fresh major (`v2`, `v2.0.0`) — never repurpose `v1`.

## MIT License template (drop in as `LICENSE`)

```
MIT License

Copyright (c) 2026 LedgerProve Ltd

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

## Common review-rejection reasons (and how to avoid them)

- **Description too marketing-y.** Stick to "what it does" — no "best", "fastest", "ultimate".
- **No README example.** A copy-paste workflow snippet at the top of the README is mandatory.
- **Action name collides with an existing one.** Search the marketplace before tagging.
- **`dist/` out of date.** Build with `npm run build` before tagging; commit `dist/index.js`.
- **Logo/icon doesn't match brand.** Use `branding.icon: shield` for security tools — it's the most-recognised glyph in this category.
