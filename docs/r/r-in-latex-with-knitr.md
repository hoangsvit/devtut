---
metaTitle: "R in LaTeX with knitr"
description: "R in Latex with Knitr and Code Externalization, R in Latex with Knitr and Inline Code Chunks, R in LaTex with Knitr and Internal Code Chunks"
---

# R in LaTeX with knitr



## R in Latex with Knitr and Code Externalization


Knitr is an R package that allows us to intermingle R code with LaTeX code.  One way to achieve this is external code chunks.  External code chunks allow us to develop/test R Scripts in an R development environment and then include the results in a report.  It is a powerful organizational technique.  This approach is demonstrated below.

```r
# r-noweb-file.Rnw
\documentclass{article}
 
 <<echo=FALSE,cache=FALSE>>=
 knitr::opts_chunk$set(echo=FALSE,  cache=TRUE)
 knitr::read_chunk('r-file.R')
 @
 
\begin{document}
This is an Rnw file (R noweb).  It contains a combination of LateX and R.
 
One we have called the read\_chunk command above we can reference sections of code in the r-file.R script.

<<Chunk1>>=
@
\end{document}

```

When using this approach we keep our code in a separate R file as shown below.

```r
## r-file.R
## note the specific comment style of a single pound sign followed by four dashes

# ---- Chunk1 ----

print("This is R Code in an external file")

x <- seq(1:10)
y <- rev(seq(1:10))
plot(x,y)

```



## R in Latex with Knitr and Inline Code Chunks


Knitr is an R package that allows us to intermingle R code with LaTeX code. One way to achieve this is inline code chunks. This apporach is demonstrated below.

```r
# r-noweb-file.Rnw
\documentclass{article}     
\begin{document}
This is an Rnw file (R noweb).  It contains a combination of LateX and R.

<<my-label>>=
print("This is an R Code Chunk")
x <- seq(1:10)
@

Above is an internal code chunk.
We can access data created in any code chunk inline with our LaTeX code like this.
The length of array x is \Sexpr{length(x)}.

\end{document}

```



## R in LaTex with Knitr and Internal Code Chunks


Knitr is an R package that allows us to intermingle R code with LaTeX code.  One way to achieve this is internal code chunks.  This apporach is demonstrated below.

```r
# r-noweb-file.Rnw
\documentclass{article}    
\begin{document}
This is an Rnw file (R noweb).  It contains a combination of LateX and R.

<<code-chunk-label>>=
print("This is an R Code Chunk")
x <- seq(1:10)
y <- seq(1:10)
plot(x,y)  # Brownian motion
@

\end{document}

```



#### Syntax


<li><<internal-code-chunk-name, options...>>=<br />
# R Code Here <br />
@</li>
1. \Sexpr{ #R Code Here }
<li><< read-external-R-file >>=<br />
read_chunk('r-file.R')<br />
@<br />
<br />
<<external-code-chunk-name, options...>>=<br />
@</li>



#### Parameters


|Option|Details
|------
|echo|(TRUE/FALSE) - whether to include R source code in the output file
|message|(TRUE/FALSE) - whether to include messages from the R source execution in the output file
|warning|(TRUE/FALSE) - whether to include warnings from the R source execution in the output file
|error|(TRUE/FALSE) - whether to include errors from the R source execution in the output file
|cache|(TRUE/FALSE) - whether to cache the results of the R source execution
|fig.width|(numeric) - width of the plot generated by the R source execution
|fig.height|(numeric) - height of the plot generated by the R source execution



#### Remarks


Knitr is a tool that allows us to interweave natural language (in the form of LaTeX) and source code (in the form of R).  In general, the concept of interspersing natural language and source code is called [literate programming](https://en.wikipedia.org/wiki/Literate_programming).  Since knitr files contain a mixture of LaTeX (traditionally housed in .tex files) and R (traditionally housed in .R files) a new file extension called R noweb (.Rnw) is required.  .Rnw files contain a mixture of LaTeX and R code.

Knitr allows for the generation of statistical reports in PDF format and is a key tool for achieving [reproducable research](https://en.wikipedia.org/wiki/Reproducibility#Reproducible_research).

Compiling .Rnw files to a PDF is a two step process.  First, we need to know how to execute the R code and capture the output in a format that a LaTeX compiler can understand (a process called 'kniting').  We do this using the knitr package.  The command for this is shown below, assuming you have [installed the knitr package](https://en.wikipedia.org/wiki/Literate_programming):

```r
Rscript -e "library(knitr); knit('r-noweb-file.Rnw')

```

This will generate a normal .tex file (called r-noweb.tex in this example) which can then be turned into a PDF file using:

```r
pdflatex r-noweb-file.tex

```
