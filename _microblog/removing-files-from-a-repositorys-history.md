---
layout: post
title: Removing files from a repository's history
subtitle: Completely remove a file from your local and remove repositories
date: 2021-04-16
tags: [git]
---

## Removing a file added in the most recent unpushed commit

If the file was added with your most recent commit, you can delete the file and amend the commit:

<pre class="command-line">
    <code class="language-bash">
git rm --cached giant_file
    </code>
</pre>

Then commit the change using `--amend -CHEAD`:

<pre class="command-line" data-output="1,2,3">
    <code class="language-bash">
# Amend the previous commit with your change
# Simply making a new commit won't work, as you need
# to remove the file from the unpushed history as well
git commit --amend -CHEAD
    </code>
</pre>

Push to your remote repository:

<pre class="command-line">
    <code class="language-bash">
git push
    </code>
</pre>

Or, if neccessary, force push:

<pre class="command-line">
    <code class="language-bash">
git push --force
    </code>
</pre>

