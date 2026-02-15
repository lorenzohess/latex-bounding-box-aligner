# PDF Bounding Box → LaTeX Macro Generator

## What This Tool Does

This tool is a single HTML + Javascript webpage which helps you overlay custom LaTeX content onto existing PDF pages. Common use cases include:

- **Homework answers**: fill in solutions on a PDF with specific answer boxes
- **Form filling**: add typed content to PDF forms
- **General annotations**: place text, equations, or diagrams at precise locations on any PDF

The tool does **not** modify your PDF. Instead, it generates LaTeX macros that position content at exact coordinates when you include the PDF in a LaTeX document which you compile yourself. You write the content and LaTeX will place it within that box.

---

## How to Use the Website

### Step 1: Open Your PDF

1. Open `index.html` in any modern browser.
2. Click the **Open PDF** button.
3. Select your PDF file.

All pages will render in a scrollable viewer. The zoom settings only affects the preview.

### Step 2: Draw Bounding Boxes

1. Click and drag on any page to draw a rectangular box
2. The box will appear with a colored border and a default name like `Box1`
3. Draw as many boxes as you need on any page

### Step 3: Name Your Boxes

1. In the **Bounding Boxes** panel on the right, click a box name to edit it
2. Enter a descriptive name like `AnswerQone`, `SolutionBox`, or `StudentName`
  - This will be the name of the LaTeX command for that box
  - Must contain only letters A-Z and a-z

### Step 4: Choose Output Format

Select your preferred LaTeX positioning package:

| Format      | Best For                                  |
|-------------|-------------------------------------------|
| **TikZ**    | Most users.                               |
| **textpos** | Lighter alternative if you don't use TikZ |

Both work with pdfLaTeX, XeLaTeX, and LuaLaTeX.

### Step 5: Copy the LaTeX Code Into Your Document

The right panel shows two output sections:

1. **Preamble** — copy this to your document preamble.

2. **Macros** — copy these after the preamble.

Click the **Copy** button next to each section to copy to clipboard.

---

## How to Use the LaTeX Output

### Basic Document Structure

```latex
\documentclass{article}

% === PASTE PREAMBLE HERE ===
\usepackage{tikz}
\usepackage{pdfpages}

\newcommand{\pdffilename}{exam.pdf}

\newcommand{\insertpdfpage}[2]{%
  \includepdf[pages=#1,pagecommand={#2}]{\pdffilename}%
}

% === PASTE MACROS HERE ===
\newcommand{\AnswerQone}[2][]{%
  \begin{tikzpicture}[remember picture, overlay]
    \ifx\relax#1\relax
      \node[anchor=north west, text width=200.00pt, inner sep=0pt]
        at ([xshift=72.00pt, yshift=-144.00pt]current page.north west) {#2};
    \else
      \node[anchor=center, text width=200.00pt, align=center, inner sep=0pt]
        at ([xshift=172.00pt, yshift=-194.00pt]current page.north west) {#2};
    \fi
  \end{tikzpicture}%
}

\begin{document}

% Include page 1 with content in AnswerQone box
\insertpdfpage{1}{%
  \AnswerQone{This is my answer to question 1.}%
}

% Include page 2 with multiple boxes
\insertpdfpage{2}{%
  \AnswerQtwo{Answer to Qtwo}%
  \ScoreBox[c]{\textbf{8/10}}%
}

\end{document}
```

### Setting the PDF Filename

Use this command to set your underlying PDF.

```latex
\renewcommand{\pdffilename}{path/to/your/file.pdf}
```

### Using Box Macros

Each box becomes a command that takes content as an argument:

```latex
% Normal (top-left aligned, text wraps to box width)
\AnswerQone{Your content here}

% Centered (horizontally and vertically within the box)
\AnswerQone[c]{Centered content}
```

### Multiple Boxes on One Page

Place multiple box commands inside `\insertpdfpage`:

```latex
\insertpdfpage{3}{%
  \StudentName{John Doe}%
  \StudentID{12345678}%
  \Answer[c]{The answer is $x = 42$.}%
}
```

### Add Pages With No Boxes

Use an empty second argument:

```latex
\insertpdfpage{4}{}
```

Or include a range of pages directly with `\includepdf`:

```latex
\includepdf[pages=5-10]{\pdffilename}
```

### Auto-Incrementing Pages

If you're including pages sequentially, you can add this to your preamble:

```latex
\newcounter{pdfpagenumber}
\setcounter{pdfpagenumber}{0}

\newcommand{\nextpage}{%
  \stepcounter{pdfpagenumber}%
  \includepdf[pages=\thepdfpagenumber]{\pdffilename}%
}
```

Then use:

```latex
\nextpage  % includes page 1
\nextpage  % includes page 2
\nextpage  % includes page 3
```

---

## Tips

### Content Overflow

If your content is longer than the box height, it will overflow downward. The box width constrains text wrapping, but height is not enforced. Keep content concise or adjust your box sizes.

### Math and Formatting

Box content is regular LaTeX, so you can use:

```latex
\AnswerQone{The solution is $\int_0^1 x^2\,dx = \frac{1}{3}$.}

\AnswerQtwo{%
  \begin{itemize}
    \item First point
    \item Second point
  \end{itemize}
}
```

### Adjusting Position After Generation

If a box is slightly off, you can manually tweak the `xshift` and `yshift` values in the generated macro. Positive `xshift` moves right; positive `yshift` (note the minus sign in the code) moves down.

### Debugging Positioning

To visualize box boundaries while developing, you can add a frame to TikZ nodes:

```latex
\node[anchor=north west, text width=200pt, draw=red, inner sep=0pt] ...
```
