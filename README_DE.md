# Template_Bachelorthesis_eng

This template was written by: Andrea Horvath, Hochschule Campus Wien

created on: 10 Oct 2025
last change: 10 Oct 2025

If you have suggestions for changes or other remarks pleas contact:
andrea.horvath@hcw.ac.at

Modular LaTeX template (KOMA-Script `scrreprt`) for a **Bachelor thesis**.  
A central setup file (`.sty`) holds packages, layout, fonts, and macros; each front-matter item and chapter lives in its own file under `parts/`.


## 1) Project structure

```
Template_Bachelorthesis_eng/
├─ main.tex                     % Entry point – assembles everything
├─ hcw_bachelor_setup.sty       % Setup: packages, layout, fonts, title macro
├─ references.bib               % BibLaTeX database
├─ fonts/                       % (optional) Arial font files for Overleaf
└─ parts/
   ├─ titlepage.tex
   ├─ acknowledgments.tex
   ├─ abstract_de.tex
   ├─ abstract.tex
   ├─ abbrevations.tex
   ├─ table_of_contents.tex    % generated automatically
   ├─ list_of_figures.tex  % list of all figures (generated automatically)
   ├─ list_of_tables.tex   % list of all tables (generated automatically)
   ├─ introduction.tex
   ├─ chapter1.tex
   ├─ chapter2.tex
   ├─ ovierview_ai.tex         % overview of used AI technologies
   ├─ bibliography.tex  % generated automatically, based on references in references.bib
   └─ appendix.tex
```

## 2) Requirements

### TeX distribution (pick one)
- **Windows**: [MiKTeX] or **TeX Live**  
- **macOS**: **MacTeX** (TeX Live for macOS)  
- **Linux**: **TeX Live** via your package manager (e.g., `sudo apt install texlive-full`, or minimally `texlive-base texlive-latex-extra biber` …)

### Recommended engine & backend
- **Engine:** **XeLaTeX** (or LuaLaTeX) — so system fonts like Arial can be used.  
- **Bibliography:** **BibLaTeX + Biber** (do *not* use classic BibTeX with this template).

### Packages/components (if you installed a minimal TeX)
- `koma-script`, `fontspec` (Xe/Lua only), `babel` (ngerman), `csquotes`, `biblatex`, `biber`  
- Layout/tables/figures: `geometry`, `graphicx`, `booktabs`, `tabularx`, `longtable`, `caption`, `eso-pic`  
- Hyperlinks: `hyperref`  
- Optional: `chngcntr`, `wallpaper`, `changepage`, `scrextend`

### Fonts (Arial)
- **Overleaf:** upload Arial files into a `fonts/` folder (see **Fonts** below).  
- **Local:** Arial is typically available on Windows/macOS. On Linux, install via **msttcorefonts** or use substitutes like **TeX Gyre Heros** / **Liberation Sans**.

## 3) Quick start

1. Open `hcw_bachelor_setup.sty` and fill in your details:
   ```tex
   \newcommand{\thesistitleDE}{Your German title}
   \newcommand{\authorname}{Your Name}
   \newcommand{\university}{Your University}
   \newcommand{\faculty}{Your Faculty}
   % ...
   ```

2. Write content in the files under `parts/` (e.g., `einleitung.tex`, `kapitel1.tex`, …).

3. **Compile** (Overleaf runs the sequence automatically):
   ```bash
   xelatex main
   biber   main
   xelatex main
   xelatex main
   ```

The PDF is `main.pdf`.

## 3a) Compiling in other editors

> Goal: **XeLaTeX → Biber → XeLaTeX → XeLaTeX** (order matters)

### TeXworks (Win/macOS/Linux)
1. Set **Typeset** to **XeLaTeX** and run once.  
2. Choose **Biber** (Typeset menu/tool list) and run.  
3. Run **XeLaTeX** twice.  
*Tip:* If Biber isn’t listed, add a tool entry for program `biber` in **Preferences → Typesetting**.

### TeXstudio
- **Tools** menu in order: **XeLaTeX** → **Biber** → **XeLaTeX** → **XeLaTeX**.  
- Or define a **User Command** (Options → Build → User Commands):  
  `txs:///xelatex | txs:///biber | txs:///xelatex | txs:///xelatex`

### Texmaker
- **Tools** sequence: **XeLaTeX** → **Biber** → **XeLaTeX** → **XeLaTeX**.  
- In **Options → Quick Build**, configure a custom sequence.

### Visual Studio Code (LaTeX Workshop)
Add this to `.vscode/settings.json` or set it via the extension’s settings:
```json
{
  "latex-workshop.latex.tools": [
    {"name":"xelatex","command":"xelatex","args":["-interaction=nonstopmode","-synctex=1","%DOC%"]},
    {"name":"biber","command":"biber","args":["%DOCFILE%"]}
  ],
  "latex-workshop.latex.recipes": [
    {"name":"xe+biber","tools":["xelatex","biber","xelatex","xelatex"]}
  ],
  "latex-workshop.latex.recipe.default": "xe+biber"
}
```

### Sublime Text (LaTeXTools)
Configure the builder/recipe to use **XeLaTeX** and **Biber** (`LaTeXTools.sublime-settings`).

### Command line
```bash
xelatex main
biber   main
xelatex main
xelatex main
```

**Troubleshooting**
- Biber missing? → TeX Live Manager (`tlmgr install biber`) / MiKTeX Console (install “biber”).  
- “Undefined citations”? → Biber didn’t run; run the full sequence again.

## 4) Fonts (Arial on Overleaf)

Overleaf doesn’t ship proprietary Arial. To use true Arial:

1. Create a folder `fonts/` and upload:
   - `Arial.ttf`, `Arial Italic.ttf`, `Arial Bold.ttf`, `Arial Bold Italic.ttf`
2. In the XeLaTeX/LuaLaTeX branch of `hcw_bachelor_setup.sty`:
   ```tex
   \RequirePackage{fontspec}
   \setmainfont{Arial}[
     Path=fonts/,
     Extension=.ttf,
     UprightFont    = * ,
     ItalicFont     = * Italic ,
     BoldFont       = * Bold ,
     BoldItalicFont = * Bold Italic
   ]
   \setsansfont{Arial}[Path=fonts/,Extension=.ttf,
     UprightFont=*,ItalicFont=* Italic,BoldFont=* Bold,BoldItalicFont=* Bold Italic]
   ```
3. Do **not** load `helvet`, `tgheros`, or `uarial` when using `fontspec`.

*(If you can’t upload Arial, use TeX Gyre Heros instead of Arial in a pdfLaTeX-compatible setup.)*

## 5) Title page (background, positioning, bottom margin)

**Background on the title page only**:
```tex
\usepackage{graphicx,eso-pic}
\newcommand{\TitleBackground}[1]{%
  \AddToShipoutPictureBG*{%
    \AtPageLowerLeft{\includegraphics[width=\paperwidth,height=\paperheight]{#1}}%
  }%
}
```
**Title page macro (excerpt):**
```tex
\newcommand{\HCWTitlePage}{%
  \begingroup
  \newgeometry{top=2.5cm,bottom=1.0cm,left=2.5cm,right=2.5cm}
  \begin{titlepage}\thispagestyle{empty}
    \TitleBackground{figures/background.jpg}
    \vspace*{2cm}\hspace*{1.5cm}\begin{minipage}{0.78\textwidth}
      \raggedright
      % ... your title block ...
    \end{minipage}
    \vfill
  \end{titlepage}
  \restoregeometry
  \endgroup
}
```

## 6) Add the table of contents itself to the ToC

In `parts/inhaltsverzeichnis.tex`:
```tex
\cleardoublepage
\phantomsection
\addcontentsline{toc}{chapter}{Inhaltsverzeichnis}
\tableofcontents
\cleardoublepage
```
(If you switch to English, rename it to “Table of contents” accordingly.)

## 7) Figures & tables (and their lists)

**Figure** (appears in the list of figures via `\caption`):
```tex
\begin{figure}[ht]
  \centering
  \includegraphics[width=0.6\textwidth]{figures/image.pdf}
  \caption{Example figure}
  \label{fig:ex}
\end{figure}
```
**Table** (appears in the list of tables via `\caption`):
```tex
\begin{table}[ht]
  \centering
  \caption{Example table}
  \label{tab:ex}
  \begin{tabular}{lcr}
    \toprule
    Name & Value & Unit \\ \midrule
    A & 1.23 & m/s \\
    \bottomrule
  \end{tabular}
\end{table}
```

## 8) Labels “Abb. 1” / “Tab. 1” or “Fig. 1” / “Table 1”

To remove the chapter prefix and use short labels:
```tex
\usepackage{chngcntr}
\counterwithout{figure}{chapter}
\counterwithout{table}{chapter}
\renewcommand{\thefigure}{\arabic{figure}}
\renewcommand{\thetable}{\arabic{table}}

\usepackage{caption}
% German short labels
\captionsetup[figure]{labelformat=simple,labelsep=space,name=Abb.}
\captionsetup[table]{labelformat=simple,labelsep=space,name=Tab.}
```
For English, change to:
```tex
\captionsetup[figure]{labelformat=simple,labelsep=space,name=Fig.}
\captionsetup[table]{labelformat=simple,labelsep=space,name=Table}
```
If your first “real” figure/table starts at 2, reset counters after front matter:
```tex
\setcounter{figure}{0}
\setcounter{table}{0}
```

## 9) Bibliography (BibLaTeX + Biber)

- In the preamble: `\addbibresource{references.bib}`  
- Print the bibliography (no double heading):
```tex
\phantomsection
\chapter*{Bibliography}
\addcontentsline{toc}{chapter}{Bibliography}
\printbibliography[heading=none]
\cleardoublepage
```
Or let BibLaTeX make the heading & ToC entry:
```tex
\printbibliography[heading=bibintoc,title={Bibliography}]
```

## 10) Chapters & subheadings

Use standard sectioning:
```tex
\chapter{Chapter 1}
\section{Section}
\subsection{Subsection}
\subsubsection{Level 3}
```
Control numbering & ToC depth:
```tex
\setcounter{secnumdepth}{3} % number down to subsubsection
\setcounter{tocdepth}{2}    % list down to subsection
```

## 11) Abbreviations / Glossary

The template uses a simple `longtable` in `abkuerzungen.tex`.  
For a full glossary/acronyms with automatic lists, integrate the `glossaries` package.

## 12) “Overview of AI technologies used” after the bibliography (unnumbered)

With KOMA-Script:
```tex
\addchap{Overview of AI Technologies Used}
```
Standard:
```tex
\chapter*{Overview of AI Technologies Used}
\addcontentsline{toc}{chapter}{Overview of AI Technologies Used}
```

## 13) Adjust margins for the main matter only

After the title page:
```tex
\cleardoublepage
\pagenumbering{arabic}
\newgeometry{top=2.0cm,bottom=2.0cm,left=2.5cm,right=2.5cm,headsep=10mm}
% \restoregeometry
```

## 14) Troubleshooting

- **Undefined citations** → Biber didn’t run; run XeLaTeX → Biber → XeLaTeX → XeLaTeX.  
- **Duplicate bibliography headings** → Use `\printbibliography[heading=none]` *or* remove your own `\chapter*` heading.  
- **Extra page before title page** → Put background command **inside** the `titlepage` and avoid `\clearpage` before it.  
- **Arial looks wrong on Overleaf** → Upload fonts to `fonts/`, set compiler to XeLaTeX, fix exact filenames/case.  
- **First figure/table is “2”** → Reset counters after front matter.


