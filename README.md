# R for Data Science Solution Set

This is a solution guide to the exercises in 
[R for Data Science](http://r4ds.had.co.nz/index.html) by Hadley Wickham and 
Garret Grolemund.

This guide is a work in progress, and all are welcome to contribute solutions 
to missing sections or edits and improvements to existing work. 

## Contributing

### Workflow

To make a contribution to this guide, use the following workflow:

Clone this repository to your local machine.

```
git clone https://github.com/UI-Research/r4ds-exercises.git
```

Checkout your own branch of the repository.

```
git checkout -b <new-branch-name>
```

Add solutions or edits and improvements to exisiting work.

```
git add -u
```

Add solutions to new work.

```
git add <new-file-name>
```

Commit your changes to your local branch.

```
git commit -m <meaningful-commit-message>
```

Push your local branch up to the github repository.

```
git push -u origin <new-branch-name>
```

Issue a pull request from your branch into the master branch.

If you are new to git and github, or have any questions on any step, don't 
hesitate to contact Research Programming for assistance.

### Formatting 

This guide was written in `R Markdown` and compiled using the `bookdown` 
package. Please use the following formatting to ensure that `bookdown` compiles 
correctly.

* The exercises for each chapter are in their own `.Rmd` file in the format 
`{chapter-number}-{chapter-name}`, i.e. `03-data-visualization.Rmd`. The chapters are based on the [online copy](http://r4ds.had.co.nz/) of the book. 

    - The first line should include a top-level markdown heading (`#`) with the 
    chapter number and name, i.e. `# Chapter 3 - Data Visualisation {-}`.
    
    - Each section should include a second-level markdown heading (`##`) with 
    the section number and name, i.e. `## 3.2 - First Steps {-}`.
    
    - Each problem should include a third-level markdown heading (`###`) with 
    the problem number, i.e. `### Problem 1 {-}`.
    
    - Please note the `{-}` after each heading, this keeps the numbering 
    consistent as not each chapter in R4DS has exercises to complete.
    
* When adding a solution to an exercise, be sure to 
    
    - Copy down the question first.
    
    - Name each code chunk using the format `{chapter}-{section}-{problem}`,
    i.e. `3-2-1`. If a problem requires more than one code chunk, delineate them 
    as `3-2-1a`, `3-2-1b`, etc.
    
    - Try your best to ensure that your solution uses methods covered up to 
    that section of the book, so that others can follow along more easily.
    