---
geometry: margin=2cm
output: pdf_document
bibliography: references.bib
fig_caption: true
table_caption: true
csl: apa-6th-edition.csl
header-includes:
  - \usepackage{float}
  - \usepackage{caption}
  - \usepackage{subcaption}
  - \usepackage{graphicx}
  - \usepackage{svg}
  - \usepackage{hyperref}
---

# Report Template for MITx 6.419x
This template relies on Markdown, \TeX\ and Pandoc to bridge the gap. The initial goal of this template was to enable the authoring of reports for the 6.419x module in pure Markdown, with outputs that would render beautifully and generate identical PDFs. This approach has since been abandoned in favor of ensuring faithful PDF generation. As a result, the rendered Markdown may sometimes appear incorrect due to inline \TeX\. For best results, **do not** wait until your report is complete before attempting to render to PDF. I've been repeatedly burned by last-minute formatting issues that required troubleshooting, it's better to catch problems early rather than when you're trying to submit. If you're just here for the template, it's essentially the core of this document, with a brief version provided in Appendix A or in the directory `template.md`.

## Blind leading the blind

This document serves as a crib sheet for report authoring, based on lessons learned from many stumbles. It's intended as a starting point for quick snippets and should cover most needs in the context of 6.419x. For detailed documentation, refer to the Pandoc User's Guide[^1] and other online resources. No claims are made regarding the accuracy of citations or references -- this is simply an attempt to explore the functionality provided by Pandoc and \TeX\.

## Prerequisites  
Mileage may vary -- this document was developed and tested on macOS using a combination of `brew` and `tlmgr` CLI commands to set up the authoring environment. Start with `brew` or install dependencies directly, then add missing packages as needed using `tlmgr` (\TeX\ Live Manager - which is installed via `basictex` dependency). See documentation on [`brew`](https://brew.sh/), [`basictex`](https://formulae.brew.sh/cask/basictex#default), [`pandoc`](https://formulae.brew.sh/formula/pandoc#default), [`inkscape`](https://formulae.brew.sh/cask/inkscape#default).

### Homebrew  
```bash
brew install --cask basictex  
brew install pandoc  
brew install librsvg  # optional, may be required for SVG support
brew install inkscape # optional, mostlikely required for SVG support
```

### Version Info
```
pandoc --version
pandoc 3.6.3
Features: +server +lua
Scripting engine: Lua 5.4

tlmgr --version
tlmgr revision 74695 (2025-03-19 00:12:52 +0100)
tlmgr using installation: /usr/local/texlive/2025basic
TeX Live (https://tug.org/texlive) version 2025
```

### \TeX\
If you are attempting to use SVG based images, you will need to run one or more of the commands below

```bash
sudo tlmgr update --self
sudo tlmgr install subcaption
sudo tlmgr install graphicx
sudo tlmgr install svg  
sudo tlmgr install transparent  
sudo tlmgr install catchfile
```
## Generating a PDF
Most modern editors, including Jupyter Notebooks, support live Markdown preview, which is very helpful for ensuring that the syntax and rendering are correct. I typically work with a notebook for my code and have a separate Markdown file open for the report. I use the preview to catch any syntax issues early and confirm that any equations or complex markup render as expected.

Once I've completed a section, such as a question or figure, I render the document to a PDF to verify that everything is working as intended. To do so, use the following command:

```bash
pandoc template.md -f markdown -t pdf -s --pdf-engine=xelatex -o template.pdf --citeproc
```

If you encounter any issues with the rendering, add the `--verbose` flag to get more detailed error messages, which can help with troubleshooting:

```bash
pandoc template.md -f markdown -t pdf -s --pdf-engine=xelatex -o template.pdf --citeproc --verbose
```

### SVG Support
Markdown supports SVG images, while \TeX\ requires additional dependencies. For simplicity, SVGs images in this document are commented out. If you need support for them, uncomment the relevant sections and first attempt to use Markdown images. If the do not meet your needs ensure you have the required \TeX\ dependencies to render them successfully. Note: rendering SVG images using \TeX\ is a two-step process. Follow the commands below:

```bash
pandoc template.md -f markdown -t latex -s -o template.tex --citeproc
```

If everything is successful you should have a `template.tex` file, along with a `svg-inkscape` directory containing any converted SVG images. You can now proceed to rendering the final PDF.

```bash
xelatex --shell-escape template.tex
```

## Images

### Markdown image
Markdown images are pretty well supported, you can even leverage alt text for caption generation and reference the figure (although the reference will not appear correctly in rendered Markdown, when converted to PDF you will see the correct reference to Figure \ref{fig:graph_1}).

```markdown
![Example caption for graph](./graph-7128364_1280.png){ width=50% #fig:graph_1 }
```
**Example - Figure \ref{fig:graph_1}**

![Example caption for graph[^3]](./graph-7128364_1280.png){ width=50% #fig:graph_1 }


### \TeX\ Graphics and Subfigures
In most cases a Markdown image will do, when you want more customization, from a size and layout perspective, you can leverage \TeX\ directly. Sadly, SVG support requires installing additional dependencies, and a slightly different set of commands.

```latex
\begin{figure}[H]
\centering
\includegraphics[width=.5\textwidth]{./graph-7128362_1280.png}
\caption{Another example caption for graph\protect\footnotemark}
\label{fig:graph_2}
\end{figure}
\footnotetext{Image downloaded from Pixabay all credit to (krzysztof-m, n.d.)}
```

**Example - Figure \ref{fig:graph_2}**

\begin{figure}[H]
\centering
\includegraphics[width=.5\textwidth]{./graph-7128362_1280.png}
\caption{Another example caption for graph\protect\footnotemark}
\label{fig:graph_2}
\end{figure}
\footnotetext{Image downloaded from Pixabay all credit to (krzysztof-m, n.d.)}

**Subfigures**

There will be times when you generate graphs or other visualizations independently but want them to appear together in the same context. Subfigures are perfect for this use case. They allow you to create a single figure environment containing multiple subfigures, each with its own caption and alignment.


```latex
\begin{figure}[H]
\centering
\caption{Examle usage of Subfigures}
\begin{subfigure}{0.45\textwidth}
  \centering
  \includegraphics[width=\textwidth]{./graph-7128364_1280.png}
  \caption{Yet another example caption for graph A}
  \label{fig:graph_3_a}
\end{subfigure}%
\hspace{0.05\textwidth} % Adjust the space between the figures
\begin{subfigure}{0.45\textwidth}
  \centering
  \includegraphics[width=\textwidth]{./graph-7128362_1280.png}
  \caption{Yet another example caption for graph B}
  \label{fig:graph_3_b}
\end{subfigure}
\end{figure}
```

**Example - Figures \ref{fig:graph_3_a} and \ref{fig:graph_3_b}**

\begin{figure}[H]
\centering
\caption{Examle usage of Subfigures}
\begin{subfigure}{0.45\textwidth}
  \centering
  \includegraphics[width=\textwidth]{./graph-7128364_1280.png}
  \caption{Yet another example caption for graph A}
  \label{fig:graph_3_a}
\end{subfigure}%
\hspace{0.05\textwidth} % Adjust the space between the figures
\begin{subfigure}{0.45\textwidth}
  \centering
  \includegraphics[width=\textwidth]{./graph-7128362_1280.png}
  \caption{Yet another example caption for graph B}
  \label{fig:graph_3_b}
\end{subfigure}
\end{figure}

### SVG
As mentioned above, SVG support is native to Markdown, and requires additional dependencies for \TeX\. To ensure this template was functional out of the box, all SVG images were commented out, feel free to uncomment and experiment.

#### Markdown
```markdown
![Yet another example caption for graph (svg)](./graph-7128362.svg){ width=50% }
```

**Example**

(Uncomment to test SVG support)

<!-- 
![Yet another example caption for graph (svg)](./graph-7128362.svg){ width=50% }
 -->

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

#### \TeX\
*Note the distinction between using `\includesvg` versus `\includegraphics`.*
```latex
\begin{figure}[H]
  \centering
  \includesvg[width=.5\textwidth]{./graph-7128362.svg}
  \caption{Yet another example caption for graph (svg)}
\end{figure}
```

**Example**

(Uncomment to test SVG support)

<!-- 
\begin{figure}[H]
  \centering
  \includesvg[width=.5\textwidth]{./graph-7128362.svg}
  \caption{Yet another example caption for graph (svg)}
\end{figure}
-->


## Tables

### Markdown

Tables are rendered beautifully in \TeX\ using Markdown syntax, and it's easy to include a caption. Typically, the auto-increment functionality for tables requires using native \TeX\ table notation. However, you can use a workaround with the `\captionof{table}` command. **Note:** Table labels should be placed above the table; incorrect numbering will occur if placed below.

```markdown
\captionof{table}{A random table caption}
\label{tab:table_1}

| Node| Stat | $p$-value    |
|-----|------|--------------|
| A   | 1.0  | 0.0009765625 |
| B   | 3.0  | 0.0009765625 |
| C   | 0.5  | 0.0009765625 |
| D   | 0.0  | 0.0009765625 |
| E   | 9.0  | 0.0322265625 |
| F   | 17.0 | 0.322265625  |

```

**Example - Table \ref{tab:table_1}**

\captionof{table}{A random table caption}
\label{tab:table_1}

| Node| Stat | $p$-value    |
|-----|------|--------------|
| A   | 1.0  | 0.0009765625 |
| B   | 3.0  | 0.0009765625 |
| C   | 0.5  | 0.0009765625 |
| D   | 0.0  | 0.0009765625 |
| E   | 9.0  | 0.0322265625 |
| F   | 17.0 | 0.322265625  |


## Captions

Captions can be added to figures and tables, or in some cases, embedded directly within the figure block itself. They are automatically numbered by default. The only drawback is that when using the `\ref{label}` command, only the reference number is available.

## Footnotes, References, and Citations

References and citations are crucial for crediting authors for their intellectual contributions. Unfortunately, it's something I've struggled to prioritize, which was one of the driving factors behind creating this template. With each report, I've improved my formatting, except for proper citations. I typically use online tools like [Scribbr](https://www.scribbr.com/citation/generator) to generate citations.

My last report lacked proper citations, and I ran out of time trying to figure out [Zotero](https://www.zotero.org/). After spending some time on it, I managed to establish a faster workflow. I use Zotero's browser plugin to save references to the relevant collection. Once I'm ready to generate citations, I use the BibTeX export functionality provided by the Better BibTeX plugin for Zotero. This generates a `references.bib` file, and then the magic happens.

For convenience, I've included a CSL file. You can visit the Citation Style Language[^2] website to learn more about CSL and how to customize citation styles.

### Example Citations
1. In-text citation
   - Aynaud’s work on community detection has been widely used [@aynaudTaynaudPythonlouvain2025]
   - The complexity of matrix multiplication has been studied extensively [@ComputationalComplexityMatrix2025]

2. Parenthetical citation
   - Community detection is essential for network analysis (see [@CommunityDetectionNetworkXs])
   - Centrality measures provide insight into network structure (see [@disneySocialNetworkAnalysis2020])

3. Multiple citations
   - Several studies have explored community detection methods [@aynaudTaynaudPythonlouvain2025; @CommunityDetectionNetworkXs]

4. Direct reference:
   - According to Disney [@disneySocialNetworkAnalysis2020], centrality measures are crucial for network analysis

## Page Break

Although not generally required, there are instances where a graph or table would benefit from starting on it's own page to control page breaks you can simply use the \TeX\ command `\pagebreak` or wrap it in a `div` below to hide it from rendering in Markdown.


```markdown
<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>
```

## Source Code (Bonus - Word Counting)

Markdown offers good support for source code, so I won't go into too much detail. However, I wanted to share a small utility script I put together to help with word counting in a Jupyter notebook, it is based on [@262588213843476WordCountMarkdown] work. While it isn't the most efficient and doesn't account for math equations, it gets the job done. *Reminder:* For the word count to be accurate, make sure your latest changes are saved!

### Steps to Use the Word Counting Helper:

1. Add a `code` block at the top of your Jupyter notebook with the following:

   ```python
   __filename__ = 'scratch.ipynb'
   ```

2. Add the source code from **Appendix B** to a new `code` block.

3. Create a `markdown` block with the following comment. Ensure you use a unique identifier for the block (e.g., 'Problem 1 Part C'). Be cautious with symbols, as the regex was lightly tested:

   ```markdown
   [//]: # (count[Problem 1 Part C])
   This is a test of counting 
   ```

4. **Save the updates** to the Markdown block to ensure the word count reflects the changes.

5. Create a `code` block and enter the following code, replacing the contents of the `count_words` function with your unique identifier (e.g., 'Problem 1 Part C'):

   ```python
   count_words('Problem 1 Part C')
   ```

6. If successful, you should see the following output:

   > Problem 1 Part C: 6 words.

## The End

That is all that I've learned thus far, I hope to add more sections to this document as I improve on my authoring ability! Thank you for getting this far!

Cheers,


Akin Owolabi


<!-- Footnotes will be automatically numbered and placed -->
[^1]: https://pandoc.org/chunkedhtml-demo/index.html
[^2]: Citation Style Language - https://citationstyles.org/
[^3]: Image downloaded from Pixabay all credit to [@krzysztof-m]

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

# References
::: {#refs}
:::

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

# Appendix A - Report Template

```markdown
---
geometry: margin=2cm
output: pdf_document
bibliography: references.bib
fig_caption: true
table_caption: true
csl: apa-6th-edition.csl
header-includes:
  - \usepackage{float}
  - \usepackage{caption}
  - \usepackage{subcaption}
  - \usepackage{graphicx}
  - \usepackage{svg}
  - \usepackage{hyperref}
---

# Written Report - 6.419x Module X

<span style="visibility: hidden">\hfill</span><b>Name:</b> authorname

## Problem X: Template for assignments
Part (x): *(**X points**) This is a bare bones template for generating a complete report for MITx6.419x[^1]. (~100 word limit.)*

**Solution:** Open the Markdown version of the template and copy this into a new file to get started! Note: you'll need `references.bib` and `apa-6th-edition.csl` as well!

### Example - Figure \ref{fig:graph_1}

![Example caption for graph[^2]](./graph-7128364_1280.png){ width=50% #fig:graph_1 }

1. **In-text citation:**
   - Aynaud’s work on community detection has been widely used [@aynaudTaynaudPythonlouvain2025].
   - The complexity of matrix multiplication has been studied extensively [@ComputationalComplexityMatrix2025].


<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>


### Example - Table \ref{tab:table_1}

\captionof{table}{A random table caption}
\label{tab:table_1}

| Node| Stat | $p$-value    |
|-----|------|--------------|
| A   | 1.0  | 0.0009765625 |
| B   | 3.0  | 0.0009765625 |
| C   | 0.5  | 0.0009765625 |
| D   | 0.0  | 0.0009765625 |
| E   | 9.0  | 0.0322265625 |
| F   | 17.0 | 0.322265625  |


### Example - \TeX\ Math

You can use \TeX\ based math notation for both inline equations $x=1+2$ as well as multi-line blocks (see below). I generally have a cheatsheet open all the time[^3]. Be careful, sometimes Pandoc will fail to render because it doesn't like something in one of your math blocks.

\begin{align}
x &= \begin{bmatrix}
   a & b \\
   c & d
\end{bmatrix} \\
&= \begin{bmatrix}
   1 & 0 \\
   0 & 1
\end{bmatrix}
\end{align}

<!-- Footnotes will be automatically numbered and placed -->
[^1]: https://www.edx.org/learn/data-analysis/massachusetts-institute-of-technology-data-analysis-statistical-modeling-and-computation-in-applications
[^2]: Image downloaded from Pixabay all credit to [@krzysztof-m]
[^3]: https://quickref.me/latex

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

# References
::: {#refs}
:::

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

# Appendix A

This is additional information
```

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

# Appendix B - Word Count Source

```python
    ## https://gist.github.com/agounaris/5da16c233ce480e75ab95980831f459e
    import re
    import io
    import nbformat
    from IPython.display import display, HTML, Markdown
    
    COUNT_DIRECTIVE_REGEX = r"\[//\]: # $count\[(?P<label>.*?)\]$"
    COUNT_DIRECTIVE = re.compile(COUNT_DIRECTIVE_REGEX, re.IGNORECASE | re.MULTILINE)
    
    def process_count_directive(source):
        match = COUNT_DIRECTIVE.match(source)
        if not match:
            return (None, None)
        return(match.group('label'), source[match.end():])
    
    def markdown_word_count(source):
        return len(source.replace('#', '').strip().split(' '))
    
    
    def _is_initialized():
        if __filename__:
            return True
    
        display(HTML("""
          <p><code>__filename__</code> not initalized.</p>
        """))
    
    def iter_markdown(cell, label):
        if cell.cell_type != "markdown":
            return None
    
        found_label, source = process_count_directive(cell['source'])
        if label != found_label:
            return None
        return source
    
    
    def count_words(label):
        if not _is_initialized():
            return
    
        with io.open(__filename__, 'r', encoding='utf-8') as f:
            nb = nbformat.read(f, nbformat.NO_CONVERT)
            for cell in nb.cells:
                source = iter_markdown(cell, label)
                if source:
                    count = markdown_word_count(source)
                    display(HTML(f"""
                    <p>{label}: {count} word{"s" if count == 0 or count > 1 else '' }.</p>
                    """))
                    return
            display(HTML(f"""
            <p>{label}: Not found, check label spelling.</p>
            """))
   ```
   