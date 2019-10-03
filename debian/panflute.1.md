---
title: panflute(1)
date: 03 Oct 2019
lang: en-US
---

# NAME

panflute - metadata-driven runner for Panflute filters

# SYNOPSIS

**panflute** [*writer*]

# DESCRIPTION

**panflute** runs filters written for Panflute, a Pythonic API for transforming documents with Pandoc.
It takes its options from the document metadata.

## Arguments

*writer*

: Pandoc writer option (passed to filters).

## Document metadata

**panflute-path**

: A list of directories to search for filters.

**panflute-filters**

: A list of filters to run.
They may be specified as relative or absolute paths or as Python module names.

In either case, if a string is specified instead of a list, it will be treated as the single element of an implicit list.

See the Panflute library documentation for details.

# SEARCH PATH

The search for filters proceeds as follows:

- The current directory is searched.

- Each element of `panflute-path` is tilde-- and environment-variable--expanded and searched in order.

  In addition to directory paths, the **panflute-path** list may also contain the values **\-\-data-dir** or **\-\-no-sys-path**; these change later search behavior.

- If **\-\-data-dir** was specified, the "*pandoc_user_dir*/filters" directory is searched,
where *pandoc_user_dir* is the default user data directory as reported by **pandoc \-\-version**.

- Unless **\-\-no-sys-path** was specified, each element of Python's **sys.path** is searched in order.

# EXAMPLE

```bash
cat some_markdown.md | \
    pandoc -f markdown -t html --filter panfl >some_html.html
```

# SEE ALSO

pandoc(1), panfl(1)

Panflute library documentation

: /usr/share/doc/python3-panflute/html/index.html
