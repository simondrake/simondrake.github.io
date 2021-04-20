---
layout: page
title: Git
subtitle: Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency
permalink: /example-commands/git/
---

# Git

#### Checkout single file from another branch

<pre class="command-line"><code class="language-bash">
git checkout branch_name -- file.go
</code></pre>

#### Squash commits

<pre class="command-line"><code class="language-bash">
git rebase -i HEAD~[NUMBER OF COMMITS]
</code></pre>

#### Reset local from remote

<pre class="command-line" data-output="2"><code class="language-bash" >
git reset --hard origin/master

git reset --hard origin/{branch_name}
</code></pre>

#### Find a deleted file in commit history

If you do not know the exact path you may use

<pre class="command-line"><code class="language-bash">
git log --all --full-history -- "**/thefile*"
</code></pre>

If you know the path the file was at, you can do this:

<pre class="command-line"><code class="language-bash">
git log --all --full-history -- {path-to-file}
</code></pre>

This should show a list of commits in all branches which touched that file. Then, you can find the version of the file you want, and display it with...

<pre class="command-line"><code class="language-bash">
git show {SHA} -- {path-to-file}
</code></pre>

Or restore it into your working copy with:

<pre class="command-line"><code class="language-bash">
git checkout {SHA}^ -- {path-to-file}
</code></pre>

Note the caret symbol (`^`), which gets the checkout prior to the one identified, because at the moment of `<SHA>` commit the file is deleted, we need to look at the previous commit to get the dleted file's contents


#### Remove file that was previous tracked and is now in `.gitignore`

Single file:
<pre class="command-line"><code class="language-bash">
git rm --cached {file}
</code></pre>

Whole folder:
<pre class="command-line"><code class="language-bash">
git rm -r --cached {folder}
</code></pre>

<br>

#### `git diff` a single file between commits

<pre class="command-line"><code class="language-bash">
git diff 6c485e874de3:file.go origin/master:file.go
</code></pre>

#### Diff ranges

![it diff range help](/assets/images/pages/exampleCommands/git/git-diff-help.png)

In other words, `git diff foo..bar` is exactly the same as `git diff foo bar`; both will show you the difference between the tips of the two branches `foo` and `bar`. On the other hand, `git diff foo...bar` will show you the difference between the "merge base" of the two branches and the tip of `bar`. The "merge base" is usually the last commit in common between those two branches, so this command will show you the changes that your work on `bar` has introduced, while ignoring everything that has been done on `foo` in the mean time.
e

