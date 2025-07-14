**DISCLAIMER:** This document is a work in progress, I'll polish it later. Also, I haven't fully figured out everything about the service, so some things may just be wrong here.

# Links
Migration:
1. [Temporary OverLeaf](https://www.overleaf.com/read/kcdyhfgwybvw#2ca12f)
2. [Temporary repo](https://github.com/HaykTarkhanyan/lecture_service_attempt)
3. [Google Sheet for status tracking](https://docs.google.com/spreadsheets/d/1CUaNZJt9qlAQZcsy_i1eFSv5n2B-eL_tfbIdLOAQUAM/edit?usp=sharing)

Useful links:
1. [Lecture Service Repo](https://github.com/slds-lmu/lecture_service)
2. [Lecture Service Wiki](https://github.com/slds-lmu/lecture_service/wiki/Slides)
3. [Lecture Debug](https://github.com/slds-lmu/lecture_debug)
4. [IML Repo](https://github.com/slds-lmu/lecture_iml/tree/master)
5. [SL Repo (for reference)](https://github.com/slds-lmu/lecture_sl/)
  
# Known ToDos
1. References sometimes don't display the text as expected 
2. Footer is now absent (will get fixed once we add title/subtitle to the first slide)
3. Actions fail (should try from a branch for the main repo)

# Importing `lecture_service` files into your repo
If I'm not mistaken [`lecture_service`](https://github.com/slds-lmu/lecture_service) assumes Linux. At least we need to run a bash file, so if you're using Windows, the easiest way I found was going to Microsoft Store and installing WSL (Windows Subsystem for Linux) with Ubuntu, [download link](https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=en-us&gl=AM&ocid=pdpshare).

Open the Ubuntu terminal
```bash
git clone https://github.com/slds-lmu/lecture_iml # or another repo
cd lecture_iml
```

After that inside the repo in which you want to integrate the service place the following file - [`update-service.sh`](https://github.com/slds-lmu/lecture_service/blob/main/service/scripts/update-service.sh) 

Then run
```bash
bash update-service.sh
```
After that a repo called `scripts` which contains the `update-service.sh` file will be created. You can delete the manually inserted file now.

Also, you will see a number of new files, especially in the `style` folder.

# (Optional) Overleaf (OL) integration
If you're working in a separate repo you will most likely want to sync it with OL. You are gonna need a paid version of OL and also you may get an error stating that maximum number of files (2000) has been exceeded. You can run `git ls-files | wc -l` to get the count of the files in a given directory. If you want to get distribution of file counts per directory you can run `find . -maxdepth 1 -mindepth 1 -type d | while read dir; do   printf "%-25.25s : " "$dir";   find "$dir" -type f | wc -l; done`.


# Steps

1. First line should be `\documentclass[11pt,compress,t,notes=noshow, xcolor=table]{beamer}`
2. Comment out `\usepackage{../../style/lmu-lecture}`
3. Add `\input{../../style/preamble}`
4. Rename `blackbox_flashlight_left.pdf` from the `style` folder to `logo.pdf`
5. Change title slide in this way:
```latex
\newcommand{\titlefigure}{figure/performance_vs_interpretability}
\newcommand{\learninggoals}{
\item Why interpretability?
\item Developments until now?
\item Use cases for interpretability}


\lecturechapter{Introduction, Motivation, and History}
\lecture{Interpretable Machine Learning}
```
becomes
```latex
\titlemeta{
Interpretable Machine Learning % commenting out \title from line 10, since here we don't have heading-subheading structure (helps avoid duplication of the title)
}{
Introduction, Motivation, and History
}{
figure/performance_vs_interpretability
}{
\item Why interpretability?
\item Developments until now?
\item Use cases for interpretability
}
```
6. Change `\begin{columns}` to `\splitVCC` or `\splitVTT` [more info](https://github.com/slds-lmu/lecture_service/wiki/Slides#splitv-column-layout-helpers)
```latex
\splitVCC[0.8]{ % 0.8 is the width of the first column
  \begin{itemize}
    \item Example itemize content for centered columns
    \item Second itemize item
  \end{itemize}
  }{
  Lorem ipsum dolor sit amet, consectetur adipiscing elit
  }
```
7. Note: when using `\splitVCC` and having content splited into two frames, for some reason the second column moves when the slide changes
8. For URL's (not papers) change `\citebutton{Some Text}{https:something}` to `\sourceref{https:something}`. The problem is that the text is hardcoded to `Click for source`. Don't know if this is problematic. Maybe a workaround is to pretend it's a paper and specify the text as an author.
9. When citing papers we need to create a `references.bib` file for each chunk folder. 
```bibtex
\citebutton{Goodman \& Flaxman (2017)}{https://doi.org/10.1609/aimag.v38i3.2741}
``` 
get's replaced with 
```latex
references.bib file
@article{Goodman_Flaxman,
  author={Goodman and Flaxman},
  year={2017},
  url={https://doi.org/10.1609/aimag.v38i3.2741}
}

and actual code 
\furtherreading{Goodman_Flaxman}
```
10. A small problem when we want the text to have multiple words - `\furtherreading` displays only the last name. Workaround is removing the author and setting a title

```bibtex
@article{European_Commission,
    title={European Commission},
    year={2019},
    url={https://doi.org/10.2759/346720}
}
```
11.  If you want to use `\color` you'll need to add `\usepackage{xcolor}`, it no longer comes with `common.tex`. You may also need to add
```latex
\definecolor{ggred}{rgb}{0.973, 0.463, 0.427}
\definecolor{ggblue}{rgb}{0, 0.749, 0.769}
\definecolor{fgcolor}{rgb}{0.345, 0.345, 0.345}
```
12. You may sometimes need to import staff from `latex-math` because it has been moved from `common.tex`

# Older notes:
## Adapting Tex files
If you try compiling any file you most likely will receive `LaTeX Error: Environment tikzpicture undefined.` error. It is no longer present in `style/common.tex`, add the line `\input{../../style/preamble.tex}` in your preamble to fix it.

## Preamble
Document class arguments have changed, notable the aspect ratio is not specified to 16:9. 
New -> `\documentclass[11pt,compress,t,notes=noshow, xcolor=table]{beamer}`

Please refer to [this](https://github.com/slds-lmu/lecture_service/wiki/Slides#preamble--title-slide) wiki page for more details.

Now most likely your file will compile, but it will be a mess, we need to change many commands now. 
## Title slide (learning goals slide)
`\learninggoals` seems to be outdated, now one should use `\titlemeta`. More details [here](https://github.com/slds-lmu/lecture_service/wiki/Slides#preamble--title-slide)
Example:
```latex
\titlemeta{Random Forest}{Bagging Ensembles}{
    figure_man/rf_majvot_averaging.png
}{
    \item The basic idea of bagging
    \item The second learning goals
}
```

## Citation
Details [here](https://github.com/slds-lmu/lecture_service/wiki/Slides#citations-sourceref-and-furtherreading)

Looks like we're going to need `references.bib` file in each chunk folder (I used [SL](https://github.com/slds-lmu/lecture_sl/blob/main/slides/boosting/references.bib) as reference)

Also replace the `\citebutton` with `\furtherreading`
Example: 
```latex
\furtherreading{Adadi and Berrada 2018}{https://doi.org/10.1109/ACCESS.2018.2870052}.
```
Got replaced with `\furtherreading{Adadi_Berrada}`. `Adabi_Berrada` is defined in the newly created `references.bib` file in the chunk folder 
```bibtex
@article{Adadi_Berrada,
  author={Adadi and Berrada},
  journal={IEEE Access}, 
  title={Peeking Inside the Black-Box: A Survey on Explainable Artificial Intelligence (XAI)}, 
  year={2018},
  volume={6},
  number={},
  pages={52138-52160},
  keywords={Conferences;Machine learning;Market research;Prediction algorithms;Machine learning algorithms;Biological system modeling;Explainable artificial intelligence;interpretable machine learning;black-box models},
  doi={10.1109/ACCESS.2018.2870052},
  url={https://doi.org/10.1109/ACCESS.2018.2870052}
}
```
I manually added he `url` field, not sure if it is needed, but looks like it's present elsewhere.

## Internal notes:
Repo location is `\\wsl.localhost\Ubuntu\root\lecture_service_attempt`

