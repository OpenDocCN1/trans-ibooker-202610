+   - The code to open a block goes on 1 line, like:
+     - `if expression; then`
+     - `for expression; do`
+   - List elements in `case..esac` are indented 4 spaces and closing `;;` are at that same indent level, blocks for each item are also indented 4 spaces
+     - One-line elements should be closed with `;;` on the same line
+     - Prefer lining up the `)` in each element, unless it gets cluttered or out of hand
+     - See the example code in *Chapter 8*

# Syntax

+   - Use `#!/bin/bash` or `#!/usr/bin/env bash` when writing bash code, not `#!/bin/sh`
+     - FIXME-TOOLS: there’s a Markdown/Asciidoctor rendering issue with the three # ! above
+   - Use `$@` unless you are *really* sure you need `$*`
+   - Use `==` instead of `=` for equality, to reduce confusion with assignment
+   - Use `$()` instead of `<code>``</code>` backticks/backquotes
+     - FIXME-TOOLS: there’s a Markdown/Asciidoctor rendering issue with the backticks
+   - Use `[ [` instead of `[`