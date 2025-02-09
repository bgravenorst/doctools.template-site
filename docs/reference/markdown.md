---
title: MkDocs And Custom Markdown Guide
description: How to write MarkDown for the Doctools system
---

# MkDocs And Custom Markdown Guide

ConsenSys documentation is written using [Markdown](https://daringfireball.net/projects/markdown/) syntax.

However, we use two flavours of this syntax:

* One for pages inside the `/docs` directory that will be rendered by [MkDocs] as described below
  in the [Installed Markdown Extensions](#installed-markdown-extensions) section.
* Another using the [Github syntax](https://guides.github.com/features/mastering-markdown/)
  for content outside of this `/docs` directory. These are mainly files to support our
  open source community like `README.md`.

## Documentation Website

### /docs Directory

The [/docs] directory in the ConsenSys doc repository contains all the documentation that
is generated into a static HTML website using [MkDocs] and the [Mkdocs Material].

## Formatting Markdown for doc site

Final documentation rendering is essential, but the format of the Markdown files is also important.

Formatting the Markdown code helps reviewers and writers to navigate in the code and review your changes.

A few basic rules:

* Each file must contain a header composed of [meta-data](https://squidfunk.github.io/mkdocs-material/extensions/metadata/)
  and limited by 3 dashes `---`.


!!! example

    ```markdown
      ---
      title: Installation overview
      description: Overview and requirements to install the software
      ---
    ```

* [As for other code](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit),
  each line of Markdown code should (not an error but just a warning) be limited to 100 columns long to be readable on any editor.
  Lines have to be wrapped without cutting the line in the middle of a word. A line break displays as a space.
* No HTML markup MUST be used inside a Markdown document.
  We provide [a lot of extensions](#installed-markdown-extensions) that are able to do the same thing
  without HTML. If you think one is missing, just discuss it with the team on [Discord] and we'll
  decide together if it's worth adding an extension.
  Only one specific case where HTML is tolerated: Tables that are too complex and already exist. If possible, use another way to display your content and keep the table simple without any other elements than TABLE, TR and TD.
* Only one first level title can be present on a page.
* If using Markdown tables, format them so they are also readable in the source code.
  You can quickly achieve this by using a tool like http://markdowntable.com/ or create tables from scratch with https://www.tablesgenerator.com/markdown_tables
* Code blocks require the language type.
* Code examples can be copied and pasted and work directly without modifying it.

## Installed Markdown extensions

>**Important**
> Extensions are only available for the docs under [/docs] directory.

As markdown can be a bit limited when it comes to some specific rendering of code, TOCs, and other documentation
elements, we configured some extensions for these items.
Extensions enable you to use simple Markdown syntax to achieve some complex rendering.

>**Important**
> Never use HTML tags directly in the Markdown files to try to render content.
For consistency reasons we only allow the specific renderings available in the extensions.

Here is a list of the available extensions:

### TOC

This extension automatically displays a table of content of the current page on the right side of the page.
It displays titles to the third level (`###`). After the third level, titles won't be displayed in the TOC.

This extension also displays a link on the right of any title called "permalink".
This link can be used to point directly to the title from another website.

Toc and navigation can also be hidden using the following meta in the page header

```yml
hide:
  - navigation
  - toc
```

### Include and exclude

#### Include a file in another one

If you have content to be repeated on multiple pages, you can create it in a common page in and include
it in all required pages.

Example:
To include the content of the `examples/excluded_file.txt` in another page, use:

=== "Result"
    {!examples/excluded_file.md!}

=== "Syntax"
    ![Syntax](include.png){: width="300em"}

#### Exclude a file

An [exclude plugin](https://github.com/apenwarr/mkdocs-exclude) is installed
(see `mkdocs.exclude.yml` file for the config of exclusions).
It excludes pages from the final rendered site as otherwise every .md file is rendered and copied.
Pages will still be in the source repository but they won't be copied in the final site and won't
appear in the search results even if you did not link them from the navigation. It's handy to
prevent include files to be reachable as standalone pages as they are intended to be included in
other pages.

### Admonition

The [Admonition extension](https://squidfunk.github.io/mkdocs-material/extensions/admonition/#admonition)
enables information, warning, and danger blocks.

!!!example

    ````markdown
    !!! note
        This is a multi line note
        in the EthSigner documentation.
    ````

    renders as

    !!! note
        This is a multi line note
        in the EthSigner documentation.

The 4 spaces indentation is required for the content to be part of the admonition.

We use the following [types](https://squidfunk.github.io/mkdocs-material/extensions/admonition/#types)
in our documentation:

* **Note** : Used to add information about a subject that doesn't directly needs to be taken in account
  to use this specific part.
  Example: "Available since v0.8.3"
* **Abstract** : Used at the beginning of a long article. Also known as "TL;DR", this can help users jump
  into the content knowing that they will find their answer somewhere in the page.
* **Info** : Used to provide detail about a specific part of the documentation.
  Ex: "The miner coinbase account is one of the accounts defined in the genesis file."
* **Tip** : Used to provide information that could help improve the use of the tool, make it faster.
  Example: "To restart the private network in the future, start from 4. Restart First Node as Bootnode."
* **Warning** : Used to warn the users about something important.
  Ex: "This will be deprecated in next version."
* **Danger** : Used to alert the user about a potential dangerous effect such as a risk of destroying
  something or losing assets.
  Ex: "Never use the development private keys for production use".
* **Example** : used to display an example. We usually use it with a single or tabbed code-block:

!!! exemple

    ````markdown
    !!! example
        This example shows how to use the `net_listening` RPC method.

        === "curl HTTP request"

            ```bash
            curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":53}' <JSON-RPC-http-endpoint:port>
            ```

        === "wscat WS request"

            ```json
            {"jsonrpc":"2.0","method":"net_listening","params":[],"id":53}
            ```

        === "JSON result"

            ```json
            {
              "jsonrpc" : "2.0",
              "id" : 53,
              "result" : true
            }
            ```
    ````

    renders as

    !!! example
        This example shows how to use the `net_listening` RPC method.

        === "curl HTTP request"

            ```bash
            curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":53}' <JSON-RPC-http-endpoint:port>
            ```

        === "wscat WS request"

            ```json
            {"jsonrpc":"2.0","method":"net_listening","params":[],"id":53}
            ```

        === "JSON result"

            ```json
            {
              "jsonrpc" : "2.0",
              "id" : 53,
              "result" : true
            }
            ```

### Footnotes

The [Footnotes extension](https://squidfunk.github.io/mkdocs-material/extensions/footnotes/) enables
adding footnotes.

Footnotes display content at the end of the page and a numbered link inside the text to go to this content.
The extension also adds a link at the end of the footnote back to the text.

!!! example

    ```markdown
    Here is a footnote[^1] that will take you to the bottom of the page if you click on it.

    [^1]:
        This is the footnote, click on the arrow to go back to the text.
    ```

    renders as

    Here is a footnote[^1] that will take you to the bottom of the page if you click on it.

    [^1]:
        This is the footnote, click on the arrow to go back to the text.

### Definitions List

The [def_list extension](https://python-markdown.github.io/extensions/definition_lists/) enables listing definitions
directly in the Markdown.

### Extra data variables

Lets you use values from the `mkdocs.yml` `extra` section as variables in the markdown code.

See [MkDocs Markdown extra data plugin](https://pypi.org/project/mkdocs-markdownextradata-plugin/)

If you have for example the following in `mkdocs.yml`

```yml
extra:
  support:
    email: quorum@consensys.net
```
You can display the email in any pages with the following code:

```markdown
{% raw %}
{{support.email}}
{% endraw %}
```

### Abbreviations

Avoid the use of abbreviations but some like "PoW" for proof-of-work or "dApp" for
decentralized application are now part of the Ethereum jargon. The [Abbreviation extension](https://python-markdown.github.io/extensions/abbreviations/)
enables a tool tip hint to be provided for abbreviations.

Preferably place abbreviations at the end of the Markdown file just before the end of file blank line to have them all in one place.

!!! example

    ```markdown
    This page explains PoA networks.
    *[PoA]: Proof of Work
    ```

    will render the abbreviation as

    This page explains PoA networks.
    *[PoA]: Proof of Work

### Math

[Arithmatex extension](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/#arithmatex-mathjax )
uses [MathJax](https://www.mathjax.org/) to enables math formulas in the documentation using [the provided syntax](https://www.mathjax.org/#demo).

!!! example

    ```markdown
    $\sigma=\displaystyle\prod_{k=1}^t\sigma_{i_k}^{L_{i_k}(0)}$

    Constructing the threshold signature $\sigma$ from $t$ individual
    signatures $\sigma_{i_k}$, $k=1,\dots,t$ and the Lagrange polynomials
    $L_{i_1}, \dots,L_{i_t}$ associated to the set $I=\{i_1,\dots,i_t\}$ of signers.
    ```

    will render as the following math formulas:

    $\sigma=\displaystyle\prod_{k=1}^t\sigma_{i_k}^{L_{i_k}(0)}$

    Constructing the threshold signature $\sigma$ from $t$ individual
    signatures $\sigma_{i_k}$, $k=1,\dots,t$ and the Lagrange polynomials
    $L_{i_1}, \dots,L_{i_t}$ associated to the set $I=\{i_1,\dots,i_t\}$ of signers.

### Plant UML

You can add diagrams using [Plant UML](https://plantuml.com/) syntax.

!!! example

    ````markdown
    ```plantuml format="svg" alt="Plantum diagram example" title="My super diagram"
    Actor1 ->  Actor2: calls
    Actor1 <-- Actor2: responds
    ```
    ````

    renders as:

    ```plantuml format="svg" alt="Plantum diagram example" title="My super diagram"
    Actor1 ->  Actor2: calls
    Actor1 <-- Actor2: responds
    ```

### Better Emphasis

The [Betterem extension](https://facelessuser.github.io/pymdown-extensions/extensions/betterem/) is
automatically applied to your bold and italic content.

### Keyboard Shortcuts

The [Keys syntax](https://facelessuser.github.io/pymdown-extensions/extensions/keys/) extension enables
displaying keyboard shortcuts.

!!! example

    ```markdown
    ++ctrl+c++
    ```

    renders ++ctrl+c++

### Folding Details Blocks

You can use a folding block to hide content. The block can be closed by default or open.
This pattern helps reduce the content length and enables a faster overview of the whole page.

!!! example

    ```markdown
    ???+ note "Folding details"
        This is the detail of my content.
        The plus sign makes it unfolded by default.
        Remove the plus sign and it will be folded by default.
    ```

    renders:

    ???+ note "Folding details"
        This is the detail of my content.
        The plus sign makes it unfolded by default.
        Remove the plus sign and it will be folded by default.

### Emojis

Emojis are fun, but they can also be useful to draw the reader's attention.
Try to use only neutral emojis like :warning:

Refer to a [supported full list of available emojis](https://www.webfx.com/tools/emoji-cheat-sheet/)
to find the suitable code.

!!! example
    `:warning:` displays :warning:

### Magic Links

If you want a URL to be displayed as a link, simply write it and this extension automatically displays it as
a link. You don't need to surround it with Markdown links syntax.

### Highlight Content

The [Mark extension](https://facelessuser.github.io/pymdown-extensions/extensions/mark/) enables highlighting of
content.

Text surrounded by double equals is highlighted in yellow.

!!! example

    ```markdown
    ==This is highlighted text==
    ```

    renders ==This is highlighted text==

### Strike Through

The [tilde syntax](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/#tilde) extensions enables text
strike through to be displayed.

!!! example

    ```markdown
    ~~This is the wrong way to do~~
    ```

    renders ~~This is the wrong way to do~~

### Symbols

The [Smart symbols syntax](https://facelessuser.github.io/pymdown-extensions/extensions/smartsymbols/) enables
the inclusion of symbols.

!!! example
    `-->` will draw a nice right arrow: -->.

### Task lists

The [Task list syntax](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/#tasklist) extension
enables displaying a list as a checklist.

### Code Samples And Examples With [SuperFences]

#### Format The Code

For writing code examples inside the documentation, refer to the developer style guides:

* Java : refer to EthSigner [coding convention](https://github.com/PegaSysEng/ethsigner/blob/master/CODING-CONVENTIONS.md).
* JSON : use https://jsonformatter.curiousconcept.com/ to format your JSON code.
* TOML : we follow version 0.5.0 language definition.
* JavaScript : see [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html).

#### Including Code in the Documentation

We use code-blocks provided by the [SuperFences] extension to present code samples and examples in the
documentation.

A basic code-block uses triple back ticks and the language name to enable syntax highlighting.

!!!example
    a JSON result is written as:

    ````text
    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": true
    }
    ```
    ````

    and renders as

    ```json
    {
      "jsonrpc": "2.0",
      "id": 1,
      "result": true
    }
    ```

Codehilite extension enables automatic syntax highlighting of code blocks. Define the language after
the code block delimiter to ensure correct highlighting.
If you don't provide the language name, the extension attempts to automatically discover it
but this can lead to errors.

Pygment is the implementation for this extension, refer to [Pygment website](https://pygments.org/)
for a list of the supported languages.

#### Tabbed Code Blocks

[SuperFences] enables additional functionality such as the tabbed code-block.

For example, to group the usage syntax and a usage example in the same block with tabs:

!!! example

    ````markdown
    === "Syntax"

        ```bash
        ethsigner --chain-id=<chainId>
        ```

    === "Example"

        ```bash
        ethsigner --chain-id=1337
        ```
    ````

    renders as

    === "Syntax"

        ```bash
        ethsigner --chain-id=<chainId>
        ```

    === "Example"

        ```bash
        ethsigner --chain-id=1337
        ```

Be careful about line increment and always surround code blocks with blank lines. Code blocks also always have to be defined with the language type. Here `bash`. See [Code Syntax Highlight](#code-syntax-highlight)

#### Line Numbers On Long Code Samples

[SuperFences] can also add [line numbers](https://facelessuser.github.io/pymdown-extensions/extensions/superfences/#showing-line-numbers)
to the code sample which makes it easier when discussing the code the sample.

The line numbers will only appear on the code block that uses the `linenums="1"` parameter.

!!! example

    ````markdown
        ```javascript linenums="1"
        window.MathJax = {
          tex: {
            inlineMath: [["\\(", "\\)"]],
            displayMath: [["\\[", "\\]"]],
            processEscapes: true,
            processEnvironments: true
          },
          options: {
            ignoreHtmlClass: ".*|",
            processHtmlClass: "arithmatex"
          }
        };

        document$.subscribe(() => {

          MathJax.typesetPromise()
        })
        ```
    ````

    renders as

    ```javascript linenums="1"
    window.MathJax = {
      tex: {
        inlineMath: [["\\(", "\\)"]],
        displayMath: [["\\[", "\\]"]],
        processEscapes: true,
        processEnvironments: true
      },
      options: {
        ignoreHtmlClass: ".*|",
        processHtmlClass: "arithmatex"
      }
    };

    document$.subscribe(() => {

      MathJax.typesetPromise()
    })
    ```

[MkDocs]: https://www.mkdocs.org/
[readthedocs.com]: https://readthedocs.com/
[Mkdocs Material]: https://squidfunk.github.io/mkdocs-material/
[Discord]: https://discord.gg/WRumvkY
[SuperFences]: https://squidfunk.github.io/mkdocs-material/extensions/pymdown/#superfences
