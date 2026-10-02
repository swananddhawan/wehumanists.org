# We Humanists

> Humans for Humanism and Progress

Source for [wehumanists.org](https://wehumanists.org), a static website built with [Hugo](https://gohugo.io) and the [Paper](https://github.com/nanxiaobei/hugo-paper) theme.

Anyone is welcome to contribute a post. You only need to know basic Markdown and Git.

---

## Prerequisites

| Tool | Version | Notes |
|---|---|---|
| [Git](https://git-scm.com/downloads) | any recent | |
| [Hugo **extended**](https://gohugo.io/installation/) | `0.158.0` or newer | CI uses `0.167.0`. Older versions fail to build. |
| [Dart Sass](https://sass-lang.com/install/) | optional | Installed in CI; not needed for local preview |

No Node.js / npm required.

### Installing Hugo

Official install guide for all platforms: <https://gohugo.io/installation/>

#### Linux

Pick **one** of these. Full Linux guide: <https://gohugo.io/installation/linux/>

**Option A: Snap** (easiest, auto-updates, extended edition):

```sh
sudo snap install hugo
```

**Option B: `.deb` from GitHub releases** (Debian / Ubuntu; same method CI uses):

```sh
HUGO_VERSION=0.167.0
wget -O /tmp/hugo.deb "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb"
sudo dpkg -i /tmp/hugo.deb
```

Use `linux-arm64.deb` instead of `linux-amd64.deb` on ARM machines.

**Option C: Prebuilt binary** (any distro):

```sh
HUGO_VERSION=0.167.0
wget -O /tmp/hugo.tar.gz "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
tar -xzf /tmp/hugo.tar.gz -C /tmp hugo
sudo mv /tmp/hugo /usr/local/bin/hugo
```

> [!WARNING]
> Avoid `sudo apt install hugo` and other distro package managers unless you've checked their version. They often ship an old or non-extended Hugo, and this site needs **extended 0.158.0 or newer**.

#### macOS

```sh
brew install hugo
```

Guide: <https://gohugo.io/installation/macos/>

#### Windows

```powershell
winget install Hugo.Hugo.Extended
```

Guide: <https://gohugo.io/installation/windows/>

#### BSD and others

See <https://gohugo.io/installation/bsd/> or the [releases page](https://github.com/gohugoio/hugo/releases).

### Verify

Check your install:

```sh
hugo version   # should mention "extended" and be v0.158.0 or newer
```

> Older Hugo fails with errors like `can't evaluate field Locale`. Upgrade Hugo (`brew upgrade hugo`).

## Setup

The theme is a **git submodule**, so clone with `--recurse-submodules`:

```sh
git clone --recurse-submodules git@github.com:swananddhawan/wehumanists.org.git
cd wehumanists.org
```

Already cloned without submodules? Run:

```sh
git submodule update --init --recursive
```

> If `themes/paper/` is empty, the site will build blank or with "layout not found" warnings. The command above fixes it.

## Commands

| Command | What it does |
|---|---|
| `hugo server -D` | Local preview at <http://localhost:1313>, **including drafts**. Live-reloads on save. |
| `hugo server` | Local preview of published posts only (what the live site shows). |
| `hugo new content posts/my-post.md` | Create a new post from the archetype. |
| `hugo --gc --minify` | Production build into `public/` (CI does this for you). |
| `git submodule update --remote themes/paper` | Update the theme to latest upstream (maintainers only). Then refresh the `layouts/_default/` overrides: see [Project structure](#project-structure). |

## Writing a new post

### 1. Create the file

```sh
hugo new content posts/my-post-slug.md
```

- Use an **English, lowercase, kebab-case** slug (e.g. `reason-and-compassion.md`).
- The slug becomes the URL: `https://wehumanists.org/posts/my-post-slug/`.

### 2. Edit the front matter

The new file starts with a TOML block between `+++` lines:

```toml
+++
title = 'आओ मानवतावाद इन्सानियत की राह पर चले'
date = 2025-01-01T17:49:10+05:30
draft = true
+++
```

| Field | Required | Description |
|---|---|---|
| `title` | yes | Post title. Hindi or any language is fine. |
| `date` | yes | Publish date. Auto-filled by `hugo new`. |
| `draft` | yes | `true` hides the post on the live site. **Set to `false` to publish.** |
| `tags` | no | e.g. `tags = ['humanism', 'science']` |
| `author` | no | e.g. `author = 'Your Name'` |
| `math` | no | `true` to enable KaTeX math rendering. |
| `mermaid` | no | `true` to enable Mermaid diagrams. |
| `hideReadingTime` | no | `true` to hide the "N min read" shown next to the date. |

> [!IMPORTANT]
> **Check the `date`.** `hugo new` fills in the current date and time. Hugo hides posts dated in the future, both locally and on the live site, until that moment arrives. If your post is missing from `hugo server -D`, check its `date` first. To preview a future-dated post on purpose, use `hugo server -D -F`.
>
> If the author wrote the post on an earlier day, set that date, e.g. `date = 2026-09-25T00:00:00+05:30`.

### 3. Write the content (Markdown)

Below the front matter, write in normal Markdown:

```markdown
## Section heading

Regular paragraph with **bold** and *italic* text.

> A quote from someone wise.

A claim that needs a source.[^1]

[^1]: Source or footnote text.
```

#### Pasting text from a document or email

Plain text that looks fine in an editor can render differently as Markdown. Before previewing, check these:

| In the pasted text | Renders as | Fix |
|---|---|---|
| Paragraphs separated by a single line break | One merged paragraph | Put a **blank line** between paragraphs. |
| A line starting with `- ` (e.g. a sign-off `- Name`) | A bullet list | Escape the dash: `\- Name`. |
| A line starting with `1) ` or `1. ` | A numbered list | Fine for lists. Keep list items on consecutive lines; a blank line between items is OK too. |
| A line containing only `+++` | Literal `+++` text | Use `---` for a horizontal rule. |
| A line of text directly above a `---` line | A heading | Put a blank line between the text and `---`. |

Change only formatting, never the author's words.

Collapsible section (theme shortcode; `summary` is required):

```markdown
{{< collapse summary="Click to expand" content="Hidden **markdown** text" >}}
```

### 4. Adding images

Use a **page bundle**: a folder with `index.md` plus images next to it.

```
content/posts/my-post-slug/
├── index.md
└── photo.jpg
```

```sh
hugo new content posts/my-post-slug/index.md
```

Then reference the image relatively:

```markdown
![Description of photo](photo.jpg)
```

### 5. Preview

```sh
hugo server -D
```

Open <http://localhost:1313> and check your post.

## Contributing (fork + pull request)

1. **Fork** this repository on GitHub.
2. **Clone** your fork (with submodules):
   ```sh
   git clone --recurse-submodules git@github.com:<your-username>/wehumanists.org.git
   cd wehumanists.org
   ```
3. **Create a branch**:
   ```sh
   git checkout -b post/my-post-slug
   ```
4. **Add your post** (see [Writing a new post](#writing-a-new-post)).
5. **Preview** with `hugo server -D`.
6. **Commit and push**:
   ```sh
   git add content/posts/
   git commit -m "Add post: my post title"
   git push origin post/my-post-slug
   ```
7. **Open a pull request** against `main` on GitHub.

A maintainer will review and merge. Once merged, the site deploys automatically.

### Pull request checklist

- [ ] `draft = false` in front matter
- [ ] Filename / folder is an English kebab-case slug
- [ ] Previewed locally with `hugo server -D`
- [ ] Images (if any) are in the post's page bundle
- [ ] No changes inside `themes/paper/`

## Project structure

```
.
├── archetypes/default.md       # Template used by `hugo new`
├── content/
│   ├── about.md                # About page
│   └── posts/                  # Blog posts go here
├── layouts/                    # Site-level overrides of theme templates
├── themes/paper/               # Theme (git submodule — do not edit)
├── hugo.toml                   # Site configuration (title, menu, params)
└── .github/workflows/hugo.yaml # Build + deploy pipeline
```

To change something the theme renders, copy the file from `themes/paper/layouts/...` into the same path under `layouts/` and edit it there.

Current overrides:

- `layouts/_default/baseof.html`: theme copy, with `site.LanguageCode` (deprecated) replaced by `site.Language.Locale`.
- `layouts/_default/single.html`: theme copy, with a "N min read" item added to the post byline after the author (skipped when `hideReadingTime = true`).

> [!NOTE]
> **Updating the theme?** Hugo overrides whole files, so each file above hides any upstream changes to the theme's version. After running `git submodule update --remote themes/paper`, refresh both overrides:
>
> ```sh
> cp themes/paper/layouts/_default/baseof.html layouts/_default/baseof.html
> cp themes/paper/layouts/_default/single.html layouts/_default/single.html
> ```
>
> Then re-apply the custom changes:
>
> - In `baseof.html`, change `site.LanguageCode` to `site.Language.Locale`.
> - In `single.html`, after the author `{{- end -}}` in the byline, add:
>
>   ```go-html-template
>   {{- if not .Params.hideReadingTime -}}
>   <span class="mx-1">&middot;</span>
>   <span>{{- .ReadingTime }} min read</span>
>   {{- end -}}
>   ```
>
> Run `git diff layouts/` to compare against the previous version, then `hugo server -D` and check that posts show the read time and no deprecation warnings appear.

## Deployment

- Every push to `main` triggers [`.github/workflows/hugo.yaml`](.github/workflows/hugo.yaml).
- It builds with Hugo extended `0.167.0` and deploys to **GitHub Pages**.
- Can also be triggered manually from the repository's **Actions** tab (`workflow_dispatch`).
- `public/` is git-ignored; never commit build output.

## License

Everything in this repository (posts, templates, configuration) is licensed under [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/). The full text is in [LICENSE](LICENSE).

In short, you may share and adapt the material if you:

- **Attribute**: credit We Humanists and link to the license.
- **NonCommercial**: don't use it for commercial purposes.
- **ShareAlike**: release your adaptations under the same license.

By submitting a pull request, you agree that your contribution is licensed under CC BY-NC-SA 4.0.

The Paper theme in `themes/paper/` is a separate project with its own license: see [`themes/paper/LICENSE`](themes/paper/LICENSE).

The site footer shows the license notice. It comes from the `copyright` setting in `hugo.toml`.
