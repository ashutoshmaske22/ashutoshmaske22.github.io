---
layout: ../../layouts/Post.astro
title: How this website is built and deployed
description: A static site, one GitHub repo, a CI gate and free hosting. Total running cost is zero.
date: 2026-10-30
tag: Platform
---

I wanted this site to follow the same rules I use at work: everything in Git, every change checked by a pipeline, and a rollback that takes seconds.

## The setup

<figure class="diagram">
<svg viewBox="0 0 640 150" role="img" aria-label="Laptop pushes to GitHub, GitHub Actions builds and audits, GitHub Pages serves the site to visitors">
<g font-family="IBM Plex Mono, monospace" font-size="13" text-anchor="middle">
<rect x="10" y="40" width="120" height="60" rx="6" fill="#141a21" stroke="#232c36"/><text x="70" y="66" fill="#e3e8ee">laptop</text><text x="70" y="86" fill="#8b97a5" font-size="11">git push</text>
<rect x="175" y="40" width="120" height="60" rx="6" fill="#141a21" stroke="#232c36"/><text x="235" y="66" fill="#e3e8ee">GitHub</text><text x="235" y="86" fill="#8b97a5" font-size="11">repo + PR</text>
<rect x="340" y="40" width="120" height="60" rx="6" fill="#141a21" stroke="#f0a742"/><text x="400" y="66" fill="#f0a742">Actions</text><text x="400" y="86" fill="#8b97a5" font-size="11">build · audit</text>
<rect x="505" y="40" width="125" height="60" rx="6" fill="#141a21" stroke="#5fd0a4"/><text x="567" y="66" fill="#5fd0a4">Pages</text><text x="567" y="86" fill="#8b97a5" font-size="11">CDN · HTTPS</text>
</g>
<g stroke="#8b97a5" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round">
<path d="M132 70h38"/><path d="M164 64l6 6-6 6"/>
<path d="M297 70h38"/><path d="M329 64l6 6-6 6"/>
<path d="M462 70h38"/><path d="M494 64l6 6-6 6"/>
</g>
<text x="400" y="128" text-anchor="middle" font-family="IBM Plex Mono, monospace" font-size="11" fill="#ef6f5e">a failed check stops here</text>
</svg>
</figure>


- **Astro** builds the site into plain HTML files. There is no server to patch or scale.
- **GitHub** holds the code. Changes go through pull requests.
- **GitHub Actions** installs dependencies, runs a security audit and builds the site on every pull request. If any step fails, the change can't merge.
- **GitHub Pages** hosts the result for free once a change lands on `main`.
- **Dependabot** opens pull requests when a dependency needs an update.

## What a change looks like

1. Create a branch and edit a file.
2. Open a pull request. Actions runs the checks.
3. Merge. The deploy job publishes the new version in a minute or two.

## When something goes wrong

Every version lives in Git. If a release breaks something, I run `git revert` on the bad commit and push, and the pipeline republishes the last good version.

## What it costs

Nothing. The repository, the pipeline minutes and the hosting all fit inside GitHub's free tier for public repositories at this size.
