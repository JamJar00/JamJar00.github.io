---
layout: post
title: Introducing Japr - The Project Linter
tags: japr
---

Japr is a cross-language tool for rating and enforcing the overall quality of projects by looking at tool & language setup

Over summarized: It's a linter that makes sure you install linters (and some other stuff)

[GitHub](https://github.com/JamJar00/japr) | [PyPi](https://pypi.org/project/Japr/) | [Docker](https://hub.docker.com/r/jamoyjamie/japr)

![Screenshot of a report](/assets/blog/img/japr-screenshot.png)

## Japr
Japr is a linter to help you make sure your project is setup as best as it can be. It checks things like:
- Do you have a readme? Does it have a getting started section or some installation docs?
- Have you setup a linter for your language? Are you following the best practices for setting up those languages?
- Is git setup correctly? Using `main` instead of `master` and excluding the right files?

Obviously project setup depends highly on the project's purpose so Japr has four configurations available:
- Personal - A lightweight ruleset designed to tidy up personal projects
- Team - A balanced ruleset designed for projects that belong to a single team
- Inner Source - A comprehensive ruleset for projects that are accessible across an organisation and may be used by other teams (often referred to as 'inner source' projects
- Open Source - A comprehensive ruleset for open source projects that are for anyone and everyone that might stumble upon the code

Japr is a very strict linter, it's opinionated and hard to satisfy so don't be upset if your project doesn't pass straight away! I don't have a single project that meets all of Japr's demands (even, ironically, Japr itself). Make use of the auto fixes and don't be afraid to suppress things if you disagree!

## Rationale
Think about your favourite open source project. Chances are it's very easy to get started with because you just looked at the readme and followed the steps. Perhaps you've raised an issue and they'd setup issue templates to help you help them. Perhaps you committed some code, ran their linters and their CI/CD pipelines.

Now think about a time when the codebase hurt you, maybe that proof-of-concept code you inherited from a coworker full of `.DS_Store` files or that GitHub project that might have been perfect for you if you could actually build it.

A lot of the differences between those two projects probably seem like common sense. Create a readme, setup CI/CD, create a `.gitignore` but there's plenty more things we take for granted in the best projects.

We lint basic things like our code formatting which could also be fixed with common sense, why not lint our project setup?

## Getting Started
You can install Japr using pip and run as you expect:
```bash
pip install japr
japr <path> -t <project-type>
```
Or just run it in Docker:
```bash
docker run --rm -v $(pwd):/app jamoyjamie/japr:v1.0.1 -t <project-type>
```

Project type should be one of `personal`, `team`, `inner-source` or `open-source` depending on what best suits your project!

There's also experimental auto fixing for some issues which you can try by adding `--fix`!

## Future
I'll be using Japr on new projects and retrofitting old projects as I work on them again. I intend to add support for the other languages and tools I use as I go (e.g. terraform, helm) but I'll gladly accept advice, discussion and pull requests for other languages or general debate into the rulesets so that they represent a consensus.

Please give it a try, feedback and let me know what your highest project scores are!
