---
layout: page
title: A journey into the Reddit network
subtitle: Hyperlink formation
#image: /reddit_network/image.jpg
description: The pages meta description
hero_image: /reddit_network/img/network_hero.jpg
#hero_height: is-fullheight
toc: true
#toc_title: Table of Contents
menubar_toc: false
hero_darken: true
show_sidebar: false
---
## Abstract and objectives


> **The data**\
We use this [reddit dataset](https://snap.stanford.edu/data/soc-RedditHyperlinks.html), which contains the hyperlink network representing the connections between subreddits in a post, from January 2014 to April 2017. The nature of the relationship (positive/negative) were previously obtained by Kumar S. et al[^1] using crowd-sourcing and training a text based classifier. We use a [complementary dataset](https://snap.stanford.edu/data/web-RedditEmbeddings.html) to group the subreddits into clusters.

## At a global scale...

**Where are the negative edges located?**\
As a starting point, looking at the proportion of negative edges in subgroups might be helpful. More precisely, let's look at the proportion as a function of the **activity** of the subreddits.

<div style="text-align:center"><img src="/reddit_network/img/activityVSsign.png" /></div>

More active subreddits are more likely to produce negative posts towards other subreddits. Indeed we find for example

<div class="container">
  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
    <div style="width:50%;background-color:WhiteSmoke; margin: 0 auto; text-align:center;">
      <p><b>subredditdrama - shitredditsays - drama - conspiracy</b></p>
    </div>
  </div>
</div>

which are highly controversial subjects and have a lot of connections.


## At a local scale...

## An evolution through time



## Conclusion


## References
[^1]: S. Kumar, W.L. Hamilton, J. Leskovec, D. Jurafsky. [Community Interaction and Conflict on the Web](https://cs.stanford.edu/~srijan/pubs/conflict-paper-www18.pdf). World Wide Web Conference, 2018

[^2]: J. Leskovec, D. Huttenlocher, J. Kleinberg. [Signed networks in social media](https://cs.stanford.edu/people/jure/pubs/triads-chi10.pdf) Proceedings of the SIGCHI conference on human factors in computing systems. 2010.
