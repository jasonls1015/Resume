# Resume Project

> This repo is available as a GitHub Pages WebHelp project.

* [Standard Resume](https://jasonls1015.github.io/Resume/webHelpSR2-all/jls-resume.html)

* [Condensed Resume](https://jasonls1015.github.io/Resume/webHelpCR2-all/jls-resume.html)

This repo is a demonstration of my Markdown resume as a Writerside project with two single-sourced WebHelp instances.

The WebHelp is deployed to GitHub Pages with the `pages build and deployment` action, where each resume is presented according to the conditional settings for the two help instances in Writerside.

Both instances contain `include` statements that pull in the first three sections for *Authoring*, *Collaboration*, and *Deliverables*. These sections are from a snippet library named [skills.md](Writerside/topics).

Then the sections are conditionally tagged with `<if>` statements to target the separate help instances. The standard resume includes the snippets, while the condensed resume does not.

This project demonstrates the concepts of single-sourced authoring, conditional builds, and docs-as-code using GitHub and Writerside as an authoring platform.


