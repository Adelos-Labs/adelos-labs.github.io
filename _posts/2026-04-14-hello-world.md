---
title: Hello, World
---

This is the first post. New posts go in `_posts/` and are named `YYYY-MM-DD-slug.md`.

## Writing posts

Each post is a markdown file with a small frontmatter block at the top:

```yaml
---
title: My Post Title
---
```

The date comes from the filename; the layout is applied automatically via `_config.yml`.

## Code blocks

```python
def greet(name: str) -> str:
    return f"hello, {name}"
```

That's it — push to `main` and GitHub Pages will build and publish.
