---
authors:
- chris-macrae
images:
- /uploads/2017/12/31/hugovsjekyll.png
date: 2017-07-21 09:22:55 +0000
excerpt: We put Hugo and Jekyll to the test, find out which won.
title: "Hugo vs Jekyll: Benchmarked"
draft: true
categories:
- hugo
- jekyll
---
In order to help you decide which static site generator is best for your development team, we put Hugo and Jekyll toe-to-toe to see which is faster and more efficient.

## Why Did We Do This?

[StackOverflow](https://insights.stackoverflow.com/survey/2016#developers-who-code-are-happy-developers) reported that developers who can check-in code more often, more rapidly are the happiest. As developers at Forestry.io, we're in total agreement and recognize that need for developers to work more efficiently.

This need for a more efficient workflow is what has brought about the [static site](https://forestry.io/docs/getting-started/what-is-a-static-site/) movement. Managing the upgrades, performance, and security of traditional server-dependant software like Wordpress and Drupal is just too much overhead.

Static sites eliminate the need for a server, have blazing fast performance, and tight security. 

Hugo and Jekyll are the leading static site generators in the world right now according to [StaticGen.com](staticgen.com), and are supported by Forestry.io, so we wanted to find out how they measured up in performance.

## Preparing The Test

In order to fairly compare Hugo and Jekyll, we needed to ensure they were on an equal playing field.

To do this, we came up with a list of requirements for the test:

#### Equal Content

We wanted to ensure the content was as similar as possible, to ensure that we’re testing the performance of building the same site.

For each test, we created a basic YAML content model and then generated 10, 100, 1000, and 10000 posts using this model for both Jekyll and Hugo.

#### Equal template structure

In most benchmarks, boilerplate themes are used. This isn’t a fair comparison, as the template structure can affect build time.

So we put together *archive, single, and taxonomy* templates for each test with the minimum amount of HTML markup needed, no CSS or JS to process or compile, and used the same data for both Hugo and Jekyll.

#### 1:1 feature comparison

We didn’t want to benchmark against features that the two static site generators didn’t share, as this wouldn’t be a fair comparison. So we created two tests:

- Raw build performance, without any plugins or similar features.

- Build performance using common plugins/features, like sitemap and feed generation.

#### Running the Benchmark

We ran the benchmarks using a simple bash script. Each benchmark runs the build process 13 times recording the `real` time output from the Unix `time` command. We took the average of these 13 builds to get our final result.

**Hugo**

```
#!/bin/bash
# Runs Unix Time Benchmark

for ((i=1;i&amp;amp;amp;lt;=13;i++));
do
  { time hugo --quiet &amp;amp;amp;gt;/dev/null; } 2&amp;amp;amp;gt;&amp;amp;amp;amp;1 | grep real 
done

echo Benchmark Complete

```

**Jekyll**

```
#!/bin/bash
# Runs Unix Time Benchmark

for ((i=1;i&amp;amp;amp;lt;=13;i++));
do
  { time bundle exec jekyll build --quiet &amp;amp;amp;gt;/dev/null; } 2&amp;amp;amp;gt;&amp;amp;amp;amp;1 | grep real 
done

echo Benchmark Complete

```

## The Basic Benchmark

For the first benchmark, we generated content with a title, a body, and a date in the templates. The content looked like this:

**Hugo**

```
---
title: Test 1
date: 2017-07-17T13:48:56-03:00
---
# Page Headline

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce eget urna nisl. Donec rhoncus libero eget tristique dapibus. Nullam ultricies ullamcorper ipsum sed lacinia.

```

**Jekyll**

```
---
title: Test 1
layout: post
---
# Page Headline

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce eget urna nisl. Donec rhoncus libero eget tristique dapibus. Nullam ultricies ullamcorper ipsum sed lacinia.

```

The was the most basic benchmark possible, testing only how fast Hugo and Jekyll build the same number of pages.

As you can see below, Hugo is a clear winner here. The results for Hugo are so small, they can’t even be seen on the graph!

<img src="/uploads/2017/12/31/basic-stats.jpg" draggable="true" data-bukket-ext-bukket-draggable="true">

<table>
<thead>
<tr>
<th>Generator</th>
<th>10</th>
<th>100</th>
<th>1000</th>
<th>10000</th>
</tr>
</thead>
<tbody>
<tr>
<td>Hugo</td>
<td>0.05s</td>
<td>0.08s</td>
<td>0.34s</td>
<td>2.95s</td>
</tr>
<tr>
<td>Jekyll</td>
<td>1.47s</td>
<td>3.3</td>
<td>14.38</td>
<td>187.15</td>
</tr>
</tbody>
</table>

## The Second Benchmark

For the next benchmark, we generated content with a title, body, date, categories, and tags. We enabled the generation of category and tag archives, and also generated an XML sitemap and RSS/Atom Feed.

These are some of the core features a basic blog or documentation site would need, so we felt this was a fair and representative benchmark.

In this case, our content looked like this:

**Hugo**

```
---
title: Test 1
categories:
  - red
  - orange
  - yellow
  - green
  - blue
  - indigo
  - violet
tags:
  - red
  - orange
  - yellow
  - green
  - blue
  - indigo
  - violet
date: 2017-07-17T14:07:58-03:00
---
# Page Headline

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce eget urna nisl. Donec rhoncus libero eget tristique dapibus. Nullam ultricies ullamcorper ipsum sed lacinia.

```

**Jekyll**

```
---
title: test_1
tags: red orange yellow green blue indigo violet
categories: red orange yellow green blue indigo violet
layout: post
---
# Page Headline

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce eget urna nisl. Donec rhoncus libero eget tristique dapibus. Nullam ultricies ullamcorper ipsum sed lacinia.

```

Looking at the results below, you can see that Hugo is again the winner. It’s clear that as you add complexity to Jekyll the decrease in performance is *much* more noticeable.

<img src="/uploads/2017/12/31/advanced-stats.jpg" draggable="true" data-bukket-ext-bukket-draggable="true">

<table>
<thead>
<tr>
<th>Generator</th>
<th>10</th>
<th>100</th>
<th>1000</th>
<th>10000</th>
</tr>
</thead>
<tbody>
<tr>
<td>Hugo</td>
<td>0.06s</td>
<td>0.12s</td>
<td>0.65s</td>
<td>7.46s</td>
</tr>
<tr>
<td>Jekyll</td>
<td>1.38s</td>
<td>3.8</td>
<td>18.42</td>
<td>218.61</td>
</tr>
</tbody>
</table>

## Other Thoughts

It’s important to note that these tests were run in an environment where all of the necessary components were downloaded and installed beforehand.

Given that Jekyll has plugins where Hugo doesn’t, in cloud or server environments where Gems need to be downloaded, they will add additional time to builds (e.g, Forestry’s preview feature).

#### Incremental Builds
Both Hugo and Jekyll have a built-in server that provides *incremental building*. This means that from the moment you spin up the server, they will watch for changes and only rebuild the pages that have changed.

This greatly speeds up build time in a development environment for both generators.

## A Clear Winner

Given that most sites aren’t going to be anywhere close to 10000 pages, we’ll make the final verdict based on average build time over 10, 100, and 1000 pages…

<img src="/uploads/2017/12/31/hugo-vs-jekyll-totals.jpg" draggable="true" data-bukket-ext-bukket-draggable="true">

So as we can see, when it comes to speed and build performance, Hugo is a *clear* winner.

Don't just take our word for it. Hugo user @darinpope managed to get Hugo to generate 600k pages in under 5 minutes, and Dan Hersam illustrates the speed of Hugo on video.

Depending on the size and scale of your website, the performance gains offered by Hugo may offer a serious advantage.

**Next week,** we’ll compare the usability and features of Hugo and Jekyll to help you decide which static site generator is right for you.