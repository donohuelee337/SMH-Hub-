# SMH-collab-hub

**Sister-companies collaboration hub** — bidding and scheduling across St. Matthews Home, St. Matthews Electric, St. Matthews Plumbing, and Huckleberry Handyman (GC: HH / Doug & Bryan). Standalone from the SMH client/PM Apps Script project.

## Docs

- **Design spec:** [`docs/superpowers/specs/2026-04-11-sister-companies-collab-hub-design.md`](docs/superpowers/specs/2026-04-11-sister-companies-collab-hub-design.md)
- **Plans:** `docs/superpowers/plans/` (implementation checklists go here)

## Related repo

The **SMH-app** monorepo holds the existing Project Manager + client portal. It also has an in-tree workspace at `collab-hub/` and a copy of this spec under `SMH-app/docs/superpowers/specs/`. When requirements change, update **both** copies or pick one canonical doc and link the other.

## New GitHub remote

After `git init` (already done locally if you cloned from a machine that ran setup):

```bash
cd SMH-collab-hub
git remote add origin https://github.com/donohuelee337/SMH-collab-hub.git
git push -u origin main
```

Create the empty repo on GitHub first if the URL does not exist yet.
