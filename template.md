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