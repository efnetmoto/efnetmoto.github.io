## Summary

<!-- What does this change do, in one or two sentences? -->

## Checklist

- [ ] Previewed locally with `hugo server` and the change renders as expected
- [ ] Internal links use `{{< relref "..." >}}` and the build succeeds (relref
      errors at build time on a broken link)
- [ ] No broken `/files/<name>` references — the file exists in `static/files/`
      and is listed in `data/resources.toml`
- [ ] Did not edit `themes/hugo-book/` (it's a submodule — override via
      `layouts/` or `assets/` instead)
- [ ] Content follows the style rules in [AGENTS.md](AGENTS.md)
      (command forms, symptom-driven framing, pass-through/anti-blame)
- [ ] `markdownlint --config .markdownlint.yml '**/*.md'` is clean
