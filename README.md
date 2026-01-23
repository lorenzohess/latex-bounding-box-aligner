# Latex Bounding Box Aligner

## Usage

The website contains all the LaTeX you need. To avoid copy-pasting the static preamble each time we provide `bounding-box-aligner-preamble.tex` which you can import into all your LaTeX documents.

### Patterns

#### Importing `bounding-box-aligner-preamble.tex`

1. Determine the file path of `bounding-box-aligner-preamble.tex`.
2. Import the file with `\input{/path/to/bounding-box-aligner-preamble.tex}`

#### Inserting the PDF's Next Page

Add `\nextpage`.

## Features

1. Well-defined bounding box overflow: if you add too much text in a bounding box area, it'll just overflow, with no weird behavior.
2. Font size changes work within bounding box: this means you can make the font smaller with `\tiny` if you need lots more room.
3. Portable: a single, serverless webpage with HTML, CSS, and Javascript.

## Demo

For a demo, load demo.html. Open demo-base.pdf to see how the bounding boxes look when added. Then, open demo-annotated.tex and demo-annotated.pdf to see how the LaTeX commands are used and to see the resulting PDF.
