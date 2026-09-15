# mdi-companion

Companion website for the Apress book on **MDI**, a Java framework for
scientific visualization, simulation, and related technical work.

Live site: https://heddle.github.io/mdi-companion/

## What's here

- `index.html`, `about.html`, `documentation.html`, `errata.html` — the site pages
- `projects/` — one page per companion repository (`mdi`, `mdi-3D`, `mdi-radar`, `mdi-nbody`, `mdi-ml-classifier`)
- `javadocs/` — a copy of the generated `mdi` API docs
- `javadocs-3d/` — a copy of the generated `mdi-3D` API docs
- `css/style.css` — the one stylesheet the whole site shares
- `.nojekyll` — tells GitHub Pages to serve the files as-is, with no Jekyll processing

No build step. It's plain HTML/CSS; edit a file, commit, push, done.

## Enabling GitHub Pages (one-time setup)

1. Push this repository to GitHub (already done if you're reading this from the repo).
2. On GitHub, go to the repo's **Settings** tab, then **Pages** in the left sidebar.
3. Under **Build and deployment** &rarr; **Source**, choose **Deploy from a branch**.
4. Under **Branch**, choose `main` and folder `/ (root)`, then **Save**.
5. GitHub builds and publishes the site at `https://heddle.github.io/mdi-companion/`
   — the first publish can take a minute or two. Later pushes to `main` republish
   automatically.

## Updating the site

Edit the relevant `.html` file (or `css/style.css` for site-wide style changes),
then:

```bash
git add -A
git commit -m "Update ..."
git push
```

GitHub Pages picks up the new commit automatically within a minute or so.

## Regenerating the release Javadocs

Generate the documentation from the exact release tags used by the book, copy it into this
repository, and then return each source repository to `develop`:

```bash
cd ~/mdi
git switch --detach v1.2.3
mvn -Prelease clean javadoc:javadoc
rsync -a --delete target/site/apidocs/ ~/mdi-companion/javadocs/
git switch develop

cd ~/mdi-3D
git switch --detach v1.0.2
mvn -Prelease clean javadoc:javadoc
rsync -a --delete target/site/apidocs/ ~/mdi-companion/javadocs-3d/
git switch develop

cd ~/mdi-companion
git status --short
```

`-Prelease` selects the pinned Javadoc plugin and its doclint configuration. Because this command
runs only `clean` followed by the direct `javadoc:javadoc` goal, it does not reach the `verify`
phase and therefore does not run GPG signing. `rsync --delete` also removes pages for APIs that no
longer exist, so review `git status` before committing.
