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


<div style="text-align:center"><a name="activityVSsign"></a><h3>Where are the negative edges located?</h3></div>
As a starting point, looking at the proportion of negative edges in subgroups might be helpful. More precisely, let's look at the proportion as a function of the **activity** of the subreddits.

<div style="text-align:center"><img style="width:50%" src="/reddit_network/img/activityVSsign.png" /></div>

The figure shows that more active subreddits are more likely to produce negative posts towards other subreddits than positive posts. Indeed we find for example

<div class="container" style="display:flex; position:relative;width: 80%; ">
  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>SubredditDrama</b></p>
    </div>
  </div>
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>ShitRedditSays</b></p>
    </div>
  </div>
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>Drama</b></p>
    </div>
  </div>
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="border-radius: 25px;background-color:Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>conspiracy</b></p>
    </div>
  </div>

<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="border-radius: 25px;background-color:Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>The_Donald</b></p>
    </div>
  </div>

</div>

which are highly controversial subjects and have a lot of connections. Also this effect vanishes if we assume that the user randomly write positive/negative posts.

<div style="text-align:center"><h3>What happens if we consider neighboring relationships?</h3></div>

As a simple neighborhood, let's consider only relations between three subreddits with a connection between them. Hereafter we will call it a **triad**. 
We begin our analysis with the **balance** theory, introduced by Heider in the 1940s[^2].


<div class="container" style="display:flex;">
  <div class="row" style="width: 45%;margin: auto; margin-top: 1em; margin-bottom: 2em;">
<table>
<thead>
<tr>
<th align="center">Sentence</th>
<th align="center">Occurrence</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center">The friend of my friend is my friend</td>
<td align="center">High</td>
</tr>
<tr>
<td align="center">The enemy of my friend is my enemy</td>
<td align="center">High</td>
</tr>
<tr>
<td align="center">The friend of my friend is my enemy</td>
<td align="center">Low</td>
</tr>
<tr>
<td align="center">The enemy of my enemy is my enemy</td>
<td align="center">Low</td>
</tr>
</tbody>
</table>
      </div>


<div class="row" style="width: 45%;margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <div style="padding: 10px 20px; text-align:justify;">
      <p>This theory claims that some type of triad to appear more often than the others. In particular, the theory is based on the assumption that whenever there exists an interpersonal relationships between two people, communities, then there exists a harmony so that the ideas shared by both subjects coexist without any tension and complication. This leads for example to an overrepresentation of the situation <I>the friend of my friend is my friend</I>.
</p>
    </div>
  </div>

</div>
>This theory can be extended to the **weak structural balance** theory, by weakening the assumption and stating that only *the friend of my friend is my enemy* relationship should be underrepresented, as introduced by Davis in the 1960s[^3].

To check if this relation holds in the Reddit network, we trained a linear classifier to guess the sign (positive/negative) of the closing edge of the triad using the characteristic of the other two relationships, that already exists when this hyperlink is created. As some subreddits have a preference for a certain type of relationships (e.g. controversial as we have seen [before](#activityVSsign), we need to include a parameter to account for this. We see that the Reddit network has a weakly balance structure.

<div style="width: 80%;margin: auto;">
<table>
	<thead>
	<tr>
	<th align="center">Sentence</th>
	<th align="center">Prediction</th>
	<th align="center">Balance Theory</th>
	<th align="center">Weakly Balance Theory</th>
	</tr>
	</thead>
	<tbody>
	<tr>
	<td align="center">The friend of my friend is my friend</td>
	<td align="center">Likely</td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	</tr>
	<tr>
	<td align="center">The enemy of my friend is my enemy</td>
	<td align="center">Likely</td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	</tr>
	<tr>
	<td align="center">The friend of my friend is my enemy</td>
	<td align="center">Unlikely</td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	</tr>
	<tr>
	<td align="center">The enemy of my enemy is my enemy</td>
	<td align="center">Likely</td>
	<td align="center"><img width=10px src="/reddit_network/img/cross.svg" /></td>
	<td align="center"><img width=18px src="/reddit_network/img/check.svg" /></td>
	</tr>
	</tbody>
	</table>
</div>
**So, what's next?**

The previous theory might oversimplify the situation, as other mechanisms might at stake. Here, we apply the theory of status as described in Signed Network in Social Media paper[^4]. This theory introduces a new perspective to the creation of a relation between two subreddits: positive relationships are created from a subreddit with a lower status in the graph to a subreddit with higher status. The status can be interpreted in this situation as respect.

However this model performs worse than the balance theory when trained on a linear classifier.

 
## At a local scale...

## An evolution through time



## Conclusion


## References
[^1]: S. Kumar, W.L. Hamilton, J. Leskovec, D. Jurafsky. [Community Interaction and Conflict on the Web](https://cs.stanford.edu/~srijan/pubs/conflict-paper-www18.pdf). World Wide Web Conference, 2018

[^2]: F. Heider. The Psychology of Interpersonal Relations. John Wiley & Sons, 1958

[^3]: Davis, J. A. [Structural balance, mechanical solidarity, and interpersonal relations](http://snap.stanford.edu/class/cs224w-readings/davis63balance.pdf). American journal of sociology 68.4 (1963): 444-462.

[^4]: J. Leskovec, D. Huttenlocher, J. Kleinberg. [Signed networks in social media](https://cs.stanford.edu/people/jure/pubs/triads-chi10.pdf) Proceedings of the SIGCHI conference on human factors in computing systems. 2010.
