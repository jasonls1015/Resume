# Resume Project

> This repo is available as a GitHub Pages WebHelp project.

* [Senior Technical Writer](https://jasonls1015.github.io/Resume/technical-writer/technical-writer-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/twr-resume.pdf)

* [Documentation Engineer](https://jasonls1015.github.io/Resume/documentation-engineer/documentation-engineer-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/der-resume.pdf)

* [Custom Tailored](https://jasonls1015.github.io/Resume/custom-tailored/custom-tailored-resume.html) · [PDF](https://jasonls1015.github.io/Resume/downloads/ctr-resume.pdf)

* [Writing Samples](https://jasonls1015.github.io/Resume/docs/samples.html)

This repo is a demonstration of my Markdown resumes as a Writerside project with four single-sourced instances. Two resumes are targeted at different roles, and a third is built for per-application tailoring. A fourth instance presents a portfolio of writing samples and case studies.

Each resume's title, skills, and job experience bullets live as individual snippets in a shared library, [resume_snippets.md](Writerside/topics/resume_snippets.md). Each resume assembles the sections with `include` statements, selecting and ordering the content that best fits the role. A bullet edited once in the snippet library updates in every resume that includes it.

A GitHub Actions workflow builds the three resume instances as both WebHelp and PDF. The action also builds the Writing Samples instance as a static site only, and then deploys the combined output to GitHub Pages.

This project demonstrates the concepts of single-sourced authoring, snippet-based content reuse, and docs-as-code using GitHub and Writerside as an authoring platform with capabilities to blend structured XML and markdown.