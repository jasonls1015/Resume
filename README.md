# Resume Project

> This repo is available as a GitHub Pages WebHelp project.

* [Senior Technical Writer](https://jasonls1015.github.io/Resume/technical-writer/technical-writer-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/twr-resume.pdf)

* [Content Engineer](https://jasonls1015.github.io/Resume/content-engineer/content-engineer-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/cer-resume.pdf)

* [Knowledge Manager](https://jasonls1015.github.io/Resume/knowledge-manager/knowledge-manager-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/kmr-resume.pdf)

* [Writing Samples](https://jasonls1015.github.io/Resume/docs/samples.html)

This repo is a demonstration of my Markdown resume as a Writerside project with three single-sourced instances, each targeted at a different role, plus a fourth instance presenting a portfolio of writing samples.

Every job's experience bullets live as individual snippets in a shared library, [resume_snippets.md](Writerside/topics/resume_snippets.md). Each resume assembles its Professional Experience section with `include` statements, selecting and ordering the bullets that best fit that role. A bullet edited once in the snippet library updates in every resume that includes it.

A GitHub Actions workflow builds the three resume instances as both WebHelp and PDF, builds the Writing Samples instance as WebHelp only, then deploys the combined output to GitHub Pages.

This project demonstrates the concepts of single-sourced authoring, snippet-based content reuse, and docs-as-code using GitHub and Writerside as an authoring platform.