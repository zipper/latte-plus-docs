# Latte+ documentation

User documentation and **public issue tracker** for **Latte+**, the comprehensive Latte
template language plugin for PhpStorm.

📖 **Read the docs:** <https://zipper.github.io/latte-plus-docs/>

## Found a bug or have an idea?

This repository doubles as the public issue tracker for Latte+. Please use the
templates when opening an issue:

- 🐞 [Report a bug](https://github.com/zipper/latte-plus-docs/issues/new?template=bug_report.yml)
- 💡 [Request a feature](https://github.com/zipper/latte-plus-docs/issues/new?template=feature_request.yml)

> The plugin's source code lives in a separate private repository. Only documentation
> and issues are public here.

## Building the docs locally (optional)

The site uses [just-the-docs](https://just-the-docs.com/) and is built natively by
GitHub Pages — no CI step is required. To preview locally:

```bash
bundle install
bundle exec jekyll serve
```

Then open <http://localhost:4000/latte-plus-docs/>.
