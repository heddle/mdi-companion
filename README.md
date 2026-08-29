# mdi-companion

Companion website for the Apress book on **MDI**, a Java framework for
scientific visualization, simulation, and related technical work.

Live site: https://heddle.github.io/mdi-companion/ (once Pages is enabled — see below)

## What's here

- `index.html`, `about.html`, `documentation.html`, `errata.html` — the site pages
- `projects/` — one page per companion repository (`mdi`, `mdi-3D`, `mdi_radar`, `mdi-ml-classifier`)
- `javadocs/` — a copy of the generated `mdi` API docs
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

## Regenerating the mdi Javadoc

From the `mdi` project:

```bash
mvn -P '!release' javadoc:javadoc
cp -R target/site/apidocs/* ../mdi-companion/javadocs/
```

(`-P '!release'` avoids the GPG-signing profile used for Maven Central releases,
which otherwise hangs waiting for a passphrase.)
