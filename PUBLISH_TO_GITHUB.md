# Publish to GitHub

## Recommended
Create a new private repository first, review once more, then decide whether to make it public.

Suggested repo names:
- `citadel-developer-tour`
- `citadel-governed-intelligence-tour`
- `citadel-collaborator-worlds`

## Minimal git flow

```bash
cd /Users/citadel/.openclaw/workspace/projects/Citadel/developer-tour-pack-public
git init
git checkout -b main
git add .
git commit -m "Initial curated Citadel developer tour pack"
git remote add origin <YOUR_GITHUB_REPO_URL>
git push -u origin main
```

## Before making public
Check for:
- names you do not want exposed
- internal service labels
- external vendor references you want generalized
- mission details that are more specific than intended
