# Importing `lecture_service` files into your repo
If I'm not mistaken [`lecture_service`](https://github.com/slds-lmu/lecture_service) assumes Linux. At least we need to run a bash file, so if you'r using Windows, the easiest way I found was going to Microsoft Store and installing WSL (Windows Subsystem for Linux) with Ubuntu, [download link](https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=en-us&gl=AM&ocid=pdpshare).

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

# Adapting Tex files
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

## Footer (ToDo)
Behaves weirdly now, 


## Internal notes:
Repo location is `\\wsl.localhost\Ubuntu\root\lecture_service_attempt`