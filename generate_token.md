
### 1️⃣ Create a GitHub token

Go to
👉 [https://github.com/settings/tokens](https://github.com/settings/tokens) → “**Fine-grained personal access tokens**” or “**Classic token**”.

Then:

* Choose **“Fine-grained token”** if possible (recommended).
* Scope permissions:

  * ✅ `contents: read and write`
  * ✅ `metadata: read`
  * ✅ `pull requests: read` (optional)

Copy the token after creating it.

---

### 2️⃣ Export it into your shell (or CI)

Run this in your terminal **before** calling `semantic-release`:

```bash
export GH_TOKEN=ghp_your_generated_token_here
```

or, if your workflow uses GitHub Actions:

```yaml
env:
  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

### 3️⃣ Verify your repository URL

In your `.git/config`, make sure the remote URL matches the repo name PSR expects:

```
[remote "origin"]
    url = https://github.com/mdop297/uv-monorepo.git
```

You can check it via:

```bash
git remote -v
```

If it’s wrong (e.g., using SSH but PSR expects HTTPS), fix it with:

```bash
git remote set-url origin https://github.com/mdop297/uv-monorepo.git
```

---

## ✅ Optional: disable GitHub publishing (if testing locally)

If you just want to test the version bump locally, you can tell PSR **not** to publish to GitHub yet.

Add this to `packages/core/pyproject.toml`:

```toml
[tool.semantic_release.publish]
upload_to_pypi = false
upload_to_release = false
```

or run:

```bash
semantic-release version --skip-publish
```

That way, it only bumps version + changelog but skips the GitHub API call.

---


✅ **Once you export `GH_TOKEN`, re-run:**

```bash
semantic-release version
```

You’ll see:

```
✅ Created GitHub release v0.0.1
📦 Uploaded distributions successfully!
```

---

