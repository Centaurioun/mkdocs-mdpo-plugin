# Configuration

Always put `mdpo` plugin in `mkdocs.yml` file after other plugins which could
edit the content of your files:

```yaml
- plugins
  - search
  - include-markdown
  - mdpo
```

## Languages

<!-- mdpo-disable-next-line -->
### **`languages`** (*list*)\*

Languages to translate your files into. Commonly defined as
[ISO 639 codes][iso-369].

!!! note

    If you are using [mkdocs-material][mkdocs-material] theme, can also be
    defined in the `extra.alternate` configuration setting (see
    [Site language selector][mkdocs-material-site-language-selector]).

<!-- mdpo-disable-next-line -->
### **`default_language`** (*str*)

Original language of your files. If not defined, the first language found in
[`languages`](#languages-list) will be used.

!!! note

    If you are using [mkdocs-material][mkdocs-material] theme, can also be
    defined in the `theme.language` configuration setting (see
    [Site language][mkdocs-material-site-language]).

## Layout

<!-- mdpo-disable-next-line -->
### **`locale_dir`** (*str*)

Directory where the PO translation files will be placed. If not defined,
the root of your documentation (`docs_dir` setting) will be used,
which will not allow you to use the command `mkdocs serve`. The default
layout would be something like:

=== "Configuration"

    ```yaml
    plugins:
      - mdpo:
          languages:
            - en
            - es
            - fr
    ```

=== "Documentation directories tree"

    ```
    docs
    ├── es
    │   └── index.md.po
    ├── fr
    │   └── index.md.po
    └── index.md
    ```

The problem with this layout is that doesn't allow you to use the
command `mkdocs serve`.

The recommended practice is to organize your tipical `docs/` directory
with multiple subdirectories, one for documentation files, other for
translation files, other for theme overrides...

=== "Configuration"

    ```yaml
    docs_dir: docs/src

    plugins:
      - mdpo:
          languages:
            - en
            - es
            - fr
          locale_dir: ../locale
    ```

=== "Documentation directories tree"

    ```
    docs
    ├── locale
    │   ├── es
    │   │   └── index.md.po
    │   └── fr
    │       └── index.md.po
    └── src
        └── index.md
    ```

<!-- mdpo-disable-next-line -->
### **`lc_messages`** (*bool* or *str*)

In the world of program translation is common the creation of a `LC_MESSAGES/`
folder inside the language directory. If you need it you can set this setting
as `true`, but if you want another folder name, you can pass a string, or even
a relative path to create more than one folder between the language directory
and their content:

=== "true"

    === "Configuration"

        ```yaml
        plugins:
          - mdpo:
              languages:
                - en
                - es
                - fr
              locale_dir: locale
              lc_messages: true
        ```

    === "Documentation directories tree"

        ```
        docs
        ├── locale
        │   ├── es
        │   │   └── LC_MESSAGES
        |   │       └── index.md.po
        |   └── fr
        |       └── LC_MESSAGES
        │           └── index.md.po
        └── index.md
        ```

=== "Custom value"

    === "Configuration"

        ```yaml
        plugins:
          - mdpo:
              languages:
                - en
                - es
                - fr
              locale_dir: locales
              lc_messages: my-own/subdir
        ```

    === "Documentation directories tree"

        ```
        docs
        ├── locales
        │   ├── es
        │   │   └── my-own
        |   │       └── subdir
        |   |           └── index.md.po
        |   └── fr
        |       └── my-own
        │           └── subdir
        |               └── index.md.po
        └── index.md
        ```

<!-- mdpo-disable-next-line -->
### **`dest_filename_template`** (*str*)

Template for destination file name inside `site/` directory. This is a valid
[Jinja2 template][jinja2-template] string that will be used to define where
should be placed the generated translated file inside the `site/` directory.

The default value is `{{language}}/{{file.dest_path}}`, being `file` the
original documentation file and `language` the language of the translation.

The context for the template includes:

- `language`: Translation language for the file.
- `file`: The original Markdown file object inside your documentation directory.
- All the configuration settings of the plugin such as `languages`,
  `default_language`, `lc_messages`, `locale_dir`, `dest_filename_template`
  itself...

<!-- mdpo-disable-next-line -->
### **`ignore_extensions`** (*list*)

File extensions that are ignored from being added to site directory, defaults to
`['.po', '.pot', '.mo']`.

<!-- mdpo-disable-next-line -->
### **`ignore_msgids`** (*list*)

You can ignore certain messages from being dumped into PO files adding them to
this list.

[iso-369]: https://en.wikipedia.org/wiki/ISO_639
[mkdocs-material]: https://squidfunk.github.io/mkdocs-material
[mkdocs-material-site-language]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-language/#site-language
[mkdocs-material-site-language-selector]: https://squidfunk.github.io/mkdocs-material/setup/changing-the-language/#site-language-selector
[jinja2-template]: https://jinja2docs.readthedocs.io/en/stable/templates.html
[polib.POFile]: https://polib.readthedocs.io/en/latest/api.html#polib.POFile