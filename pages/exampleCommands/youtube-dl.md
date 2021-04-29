---
layout: page
title: youtube-dl
subtitle: youtube-dl is an open-source download manager for video and audio from YouTube and over 1000 other video hosting websites
permalink: /example-commands/youtube-dl/
---

Note: If you get a `/usr/bin/env: 'python': No such file or directory` error when running these commands, run `which python3` and add the command before `youtube-dl`. For example - `/usr/bin/python3 youtube-dl`.

### Download

<pre class="command-line"><code class="language-bash">
youtube-dl [video url]
</code></pre>


### Download with custom title

<pre class="command-line"><code class="language-bash">
youtube-dl -o 'Custom Title.mp4' [video url]
</code></pre>

### List formats

<pre class="command-line"><code class="language-bash">
/usr/local/bin/youtube-dl --list-formats [video url]
</code></pre>

### Download best audio/video

<pre class="command-line"><code class="language-bash">
youtube-dl -f bestvideo+bestaudio [video url]
</code></pre>

### Download a specific format

<pre class="command-line"><code class="language-bash">
youtube-dl -f m4a [video url]
</code></pre>

