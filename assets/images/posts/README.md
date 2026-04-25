# Post Images

Store blog post images in this folder.

Recommended structure:

```text
assets/images/posts/YYYY-MM-DD-post-slug/image-name.png
```

Use images in Markdown with `relative_url` so paths work on GitHub Pages:

```markdown
![Image description]({{ "/assets/images/posts/YYYY-MM-DD-post-slug/image-name.png" | relative_url }})
```

