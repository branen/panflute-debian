---
title: panfl(1)
date: 03 Oct 2019
lang: en-US
---

# NAME

panfl - Pandoctools-compatible runner for Panflute filters

# SYNOPSIS

**panfl** **-t** *writer*  [**-d** *directory*]... [**\-\-data-dir**] [**\-\-no-sys-path**] [*filter*]...

**panfl** [*writer*]

**panfl** **\-\-help**

# DESCRIPTION

**panfl** runs filters written for Panflute, a Pythonic API for transforming documents with Pandoc.

When called with the **-t** argument, it can be used in Pandoctools shell scripts.

When called with one argument, it can be used with **pandoc \-\-filter**.
In this case, pandoc passes the sole argument, and search paths and filter names are read from the document metadata.

# OPTIONS

**-w**, **-t**, **-\-write**, **-\-to** *writer*

: Pandoc writer option (passed to filters).

[**-d** *directory*]...

: Search for filters in specified directories.

[**\-\-data-dir**]

: Search for filters in "*pandoc_user_dir*/filters",
where *pandoc_user_dir* is the default user data directory as reported by **pandoc \-\-version**.

 [**\-\-no-sys-path**]

: Don't search for filters in Python's **sys.path**.

[*filter*]...

: The filters to find and run.
They may be relative or absolute paths or Python module names.
If this option is omitted, filters will be taken from the **panflute-filters** key of the document's metadata.

# SEARCH PATH

The search for filters proceeds as follows:

- If one or more search paths were specified with **-d**, these are searched in order.

- If no search paths were specified with **-d** and the document metadata contains a **panflute-path** key, the corresponding value must either be a list or a string.

    * In the former case, each element of the list is tilde-- and environment-variable--expanded and searched in order.

    * In the latter case, the string is expanded and searched as above, as though it the single element of a list.

  In addition to directory paths, the **panflute-path** list may also contain the values **\-\-data-dir** or **\-\-no-sys-path**; these change later search behavior.

- If **\-\-data-dir** was specified, the corresponding directory is searched.  (See OPTIONS, above.)

- Unless **\-\-no-sys-path** was specified, each element of Python's **sys.path** is searched in order, except for '' and '.', which are never searched.

Here are some helpful tips:

- If **-d** is specified, **panflute-path** is completely ignored.

- If you want to load filters from document-level directories (instead of from user- or system-level directories) using document metadata, use relative paths with **-d** or **panflute-path** or see **panflute(1)**.

- If you want to write filters that don't rely on an external runner, see the **run_filter** and **run_filters** methods in the **panflute** Python module.

# EXAMPLES

```bash
cat some_markdown.md | \
    pandoc -f markdown -t json | \
    panfl -t html some_filter | \
    pandoc -f json -t html >some_html.html
```

```bash
cat some_markdown.md | \
    pandoc -f markdown -t html --filter panfl >some_html.html
```

# SEE ALSO

pandoc(1), panflute(1)

Panflute library documentation

: /usr/share/doc/python3-panflute/html/index.html
