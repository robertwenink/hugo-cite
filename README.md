# Hugo Cite

Easily manage your bibliography and in-text citations with [Hugo](https://gohugo.io), the popular static-site generator. 📝

⚠️ **Important note: APA is the only style currently available, and you must be aware that it does not match the entire APA spec.**  
More styles may be added eventually (contributions welcome!), but given that they are extremely verbose to implement, this is unlikely to happen in a near future.

## Install

1. Copy the partials to your `layouts/partials` directory. Make sure to mirror the directory structure (`layouts/partials/bibliography/`) in your own project.
2. Copy the shortcodes to your shortcodes directory (`layouts/shortcodes/`).

```
# Your Hugo project directory
├── layouts
|   ├── partials
|       ├── bibliography
|           ├── apa-style.html
|           └── bibliography-list.html
|   └── shortcodes
|       ├── bibliography.html
|       └── cite.html
```

## Usage

You must first provide a **[CSL-JSON](https://citeproc-js.readthedocs.io/en/latest/csl-json/markup.html) bibliography file**.
(Other formats, such as BiBTeX, are _not_ supported.)
In Zotero for instance, this can be accomplished by selecting the CSL JSON format when exporting a collection.
Just include `bib` in the filename (such as `bibliography.json`,`oh-my-bib.json`, or simply `bib.json`) and save it inside your Hugo project directory.

Here is an example:

```
# Your Hugo project directory
├── content
|   ├── article1
|       ├── index.md
|       └── bib.json
|   ├── article2
|       ├── index.md
|       ├── image.jpg
|       └── bib.json
```

### Shortcodes

There are two shortcodes you can use in your content:

- **`{{< bibliography >}}`**: display a list of works.
- **`{{< cite >}}`**: render an in-text citation.

### Display a bibliography

#### Basic Example

By default, the `{{< bibliography >}}` shortcode will render all entries from a `bib.json` included in a [leaf bundle](https://gohugo.io/content-management/page-bundles/#leaf-bundles). 

```markdown
{{< bibliography >}}
```

#### Specify a Path to the JSON File

You can also specify another JSON file located *inside* the Hugo project directory:

```markdown
{{< bibliography "path/to/bib.json" >}}
```

#### Cited Works

You can restrict the list only to works cited on the page (with the use of in-text citations, see below):

```markdown
{{< bibliography cited >}}
```

#### Combine Options

You can also **combine both options** (the path to the JSON file must come first however):

```markdown
{{< bibliography "path/to/bib.json" cited >}}
```

#### Named Params

You can chose to use **named params** for clarity (the order does not matter here):

```markdown
{{< bibliography src="path/to/bib.json" cited="true" >}}
```

#### Front-Matter

Instead of specifying the custom path inside your shortcode, you can specify it in the content page’s **front-matter** (this is especially uesful when you want to restrict to cited entries):

```markdown
---
title: My Article
bibFile: oh/my/bib.json # path relative to project root
---

## Bibliography

<!-- The bibliography will display works from oh/my/bib.json -->
{{< bibliography >}}
```

#### File From a URL

Thanks to Hugo’s `getJSON` function, you can also provide a **URL**.  
*Note however that this method may have some drawbacks if you are [reloading often](https://gohugo.io/templates/data-templates/#livereload-with-data-files), see the Hugo docs regarding potential issues.*

```markdown
{{< bibliography "https://example.com/my/bib.json" >}}
```

### Render in-text citations

Use the `{{< cite >}}` shortcode to render rich in-text citations.
The citation key must match the `id` field of a reference in your CSL-JSON file.
You can make it look like an author-date format, or anything else.

Here’s an excerpt of a CSL-JSON file:

```json
[
    {
        "id": "Lessig 2002",
        "author": [
            {
                "family": "Lessig",
                "given": "Lawrence"
        ]
    }
]
```

Using the citation key defined in the CSL-JSON, you can use it in content files:

```markdown
Our generation has a philosopher.
He is not an artist, or a professional writer.
He is a programmer. {{< cite "Lessig 2002" >}}

## Cited Works

<!-- Include the list of cited works on the page -->
{{< bibliography cited >}}
```

## Licence

[WTFPL](LICENSE)