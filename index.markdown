---
layout: page
title: A journey into the Reddit network
subtitle: Hyperlink formation
#image: /reddit_network/image.jpg
description: The pages meta description
hero_image: /reddit_network/img/network_hero.jpg
hero_height: is-fullheight
toc: true
#toc_title: Table of Contents
menubar_toc: false
hero_darken: true
show_sidebar: false
bokeh: true
output:
 html_document:
  css: _includes/style_button.css
---


## Abstract and objectives

Online social media has grown in popularity over the years, attracting more and more users to interact and share. As such, their complexity has also increased. Among them, Reddit's social news allows its users to interact and build communities, submitting various content and topics for discussion.

The goal of this project is to reveal what is the underlying structure of the network:
- How hyperlinks are created between subreddits ? 
- Can we characterise them using an existing social theory ? 
- Are the results the same at all scale (i.e. when a single specific community is considered) ? 
- Does the network exhibit structural changes through time ?

We present here some results on how these topics relate to each other, both on a large scale and on a smaller scale. Finally we explore the evolution of the network through time.


> **The data**\
We use this [reddit dataset](https://snap.stanford.edu/data/soc-RedditHyperlinks.html), which contains a network of the hyperlinks in a post between subreddits, from January 2014 to April 2017. The nature of the relationship (positive/negative) were previously obtained by Kumar S. et al[^1] using crowd-sourcing and training a text based classifier. We use a [complementary dataset](https://snap.stanford.edu/data/web-RedditEmbeddings.html) to group the subreddits into clusters.

## At a global scale...


<div style="text-align:center"><a name="activityVSsign"></a><h3 id="where-are-the-negative-links-located?">Where are the negative links located?</h3></div>
As a starting point, looking at the proportion of negative links in subgroups might be helpful. More precisely, let's look at the proportion as a function of the **activity** of the subreddits.

<div style="text-align:center"><img style="width:60%" src="/reddit_network/img/activityVSsign.svg" /></div>

The figure shows that more active subreddits are more likely to produce negative posts towards other subreddits than positive posts. Indeed we find for example

<div class="container" style="display:flex; position:relative;width: 80%; ">
  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
  <a href="https://www.reddit.com/r/SubredditDrama/" target="_blank" style="text-decoration: none; color: #505050;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>SubredditDrama</b></p>
    </div>
      </a>
  </div>
  
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
<a href="https://www.reddit.com/r/ShitRedditSays/" target="_blank" style="text-decoration: none; color: #505050;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>ShitRedditSays</b></p>
    </div>
     </a>
  </div>
 
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
<a href="https://www.reddit.com/r/Drama/" target="_blank" style="text-decoration: none; color: #505050;">
    <div style="border-radius: 25px;background-color: Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>Drama</b></p>
    </div>
      </a>
  </div>
<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
<a href="https://www.reddit.com/r/conspiracy/" target="_blank" style="text-decoration: none; color: #505050;">
    <div style="border-radius: 25px;background-color:Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>conspiracy</b></p>
    </div>
      </a>
  </div>

<div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
<a href="https://www.reddit.com/r/The_Donald/" target="_blank" style="text-decoration: none; color: #505050;">
    <div style="border-radius: 25px;background-color:Gainsboro; padding: 10px 20px; text-align:center;">
      <p><b>The_Donald</b></p>
    </div>
      </a>
  </div>

</div>

which are highly controversial subjects and have a lot of connections. Also this effect vanishes if we assume that the user randomly write positive/negative posts.

<div style="text-align:center"><h3 id="what-happens-if-we-consider-neighboring-relationships?">What happens if we consider neighboring relationships?</h3></div>

As a simple neighborhood, let's consider only relations between three subreddits with a connection between them. Hereafter we will call it a **triad**. 
We begin our analysis with the **balance** theory, introduced by Heider in the 1940s[^2].


<div class="container" style="display:flex;">
  <div class="row" style="width: 45%;margin: auto; margin-top: 1em; margin-bottom: 2em;">
<table>
<thead>
<tr>
<th align="center">Statement</th>
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
      <p>This theory claims that some types of triads appear more often than the others. In particular, the theory is based on the assumption that whenever there exists an interpersonal relationship between two people, communities, then there exists a harmony so that the ideas shared by both subjects coexist without any tension. This leads for example to an overrepresentation of the situation <I>the friend of my friend is my friend</I>.
</p>
    </div>
  </div>

</div>
>This theory can be extended to the **weak structural balance** theory, by weakening the assumption and stating that only *the friend of my friend is my enemy* relationship should be underrepresented, as introduced by Davis in the 1960s[^3].

To check if this relation holds in the Reddit network, we trained a linear classifier to guess the sign (positive/negative) of the closing edge of the triad using the characteristic of the other two relationships, that already exists when this hyperlink is created. As some subreddits have a preference for a certain type of relationships (e.g. controversial as we have seen [before](#activityVSsign)), we need to include a parameter to account for this. The table below shows if a statement is more likely to appear when compared to a randomized network. We observe that the Reddit network adheres to the weak balance structure.

<div style="width: 80%;margin: auto;">
<table>
	<thead>
	<tr>
	<th align="center">Statement</th>
	<th align="center">Prediction</th>
	<th align="center">Balance Theory</th>
	<th align="center">Weak Balance Theory</th>
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

The previous theory might oversimplify the situation, as other crucial mechanisms might have been ignored. Here, we apply the theory of **status** as described in Signed Network in Social Media paper[^4]. This theory introduces a new perspective to the creation of a relation between two subreddits: positive links are created from a subreddit with a lower status in the network to a subreddit with higher status. The status can be interpreted in this situation as respect.

However this model performs worse than the balance theory when trained on a linear classifier and the status theory does not match the measurements.

 
## At a local scale...
<div style="text-align:center"><a name="local_scale"></a></div>

We have seen that for the reddit network the weak balance theory seems to hold. But

<div style="text-align:center"><h3 id="is-it-still-the-case-at-a-local-scale?">Is it still the case at a local scale?</h3></div>

To address this question we must first group the subreddits into distinct communities.
To this end we use this [complementary dataset](https://snap.stanford.edu/data/web-RedditEmbeddings.html) providing information about what kind of users frequent which subreddits. Using this the subreddits are grouped into the communities shown in this interactive plot:
<body>
    <div align="center">
<iframe width="800" height="700" frameborder="0" scrolling="no" src="https://chart-studio.plotly.com/~youssef.saied/3.embed?link=false"></iframe>
    </div>
</body>


The communities labeled in the graph above were the ones that were identified as the most relevant for analysis. They contain a sufficient large amount of subreddits, which reasonably interact with each other (in our records). Other non-coherent communities were combined into an **Others** community. 

<div style="text-align:center"><img style="width:70%" src="/reddit_network/img/Cluster_graph.svg" /></div>

The communities were then classified into one of three classes based on how much the subreddits of the communities interact and how positive on average their interactions are

<div style="text-align:center"><h3 id="socially-Interacting"><i>Interacting Social</i> communities</h3></div>

<div style="width: 60%;margin: auto;">
<table>
    <thead>
    <tr>
    <th align="center">Features</th>
    <th align="center">Communities</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td style="text-align: center;
    vertical-align: middle;">
      <p>High connectivity</p>
      <p>High Interactivity</p>
      <p>Average negativity</p>
    </td>
    <td style="text-align: center;
    vertical-align: middle;">
    <div class="container" style="display:flex; position:relative;width: 100%; border: none">
    <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
        <button class="button" style="background-color:rgba(135,206,235,0.3)">Adult content</button>
    </div>
    <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
      <button class="button" style="background-color:rgba(135,206,235,0.3)">Gaming</button>
    </div>
    <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
    <button class="button" style="background-color:rgba(135,206,235,0.3)">Media</button>
    </div>
    </div>
    <div class="container" style="display:flex; position:relative;width: 60%; border: none">
     <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
    <button class="button" style="background-color:rgba(135,206,235,0.3)">Tech</button>
    </div>
    <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
    <button class="button" style="background-color:rgba(135,206,235,0.3)">Sports</button>
    </div>
    </div>
    </td>
    </tr>
    </tbody>
    </table>
</div>

These are communities where the subreddits both interact together at a reasonable rate. This is measured by number of hyperlinks created per subreddits and by the  _transitivity_ of the graph --- some graph theoretical property indicative of how connected a network is. Also they exhibit an average level of negative interactions. 

In general these communities seem to follow the weak theory of balance. The statement *friend of my friend is my friend* is overrepresented in all communities, which is a characteristic of the *Socially Interacting* communities. Nevertheless the community **Adult content** seems to be the only community that fully respects the _weak_ balance theory.


<div style="text-align:center"><h3 id="polemic-Interacting"><i>Polemic Interacting Social</i> communities</h3></div>
<div style="width: 60%;margin: auto;">
<table>
    <thead>
    <tr>
    <th align="center">Features</th>
    <th align="center">Communities</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td style="text-align: center;
    vertical-align: middle;">
      <p>High connectivity</p>
      <p>High Interactivity</p>
      <p>High negative hyperlink proportion</p>
    </td>
    <td style="text-align: center;
    vertical-align: middle;"><div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
         <button class="button" style="background-color:rgba(135,206,235,0.3)">Politics</button>
     </div>
     <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
       <button class="button" style="background-color:rgba(135,206,235,0.3)">Popular subjects</button>
    </div>
    </td>
    </tr>
    </tbody>
    </table>
</div>

This second class of communities differ from the first one by having a higher level of negative interactions. The theory of balance is not at all applicable here in general. 

All communities of this class have an underrepresentation of thr _the friend of my friend is my friend_ type of interaction and an overrepresentation of the counterintuitive _the enemy of my enemy is my enemy type of interaction_. This is probably due to the polemic nature of these categories and the possible complexity of the realtionships between the subreddits in such communities. The social theories presented here such as balance are mainly based on binary simplistic classification of relations as one of either friendship or animosity. Thus given the aforementioned complexity they should not be expected to hold for these communities, which is the result we find as neither status or balance hold for any of these communities. 


<div style="text-align:center"><h3 id="non-community"><i>Non-community</i> clusters</h3></div>
<div style="width: 60%;margin: auto;">
<table>
    <thead>
    <tr>
    <th align="center">Features</th>
    <th align="center">Communities</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td style="text-align: center;
    vertical-align: middle;">
      <p>Low connectivity</p>
      <p>Low Interactivity</p>
    </td>
    <td style="text-align: center;
    vertical-align: middle;"><div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
        <button class="button" style="background-color:rgba(135,206,235,0.3)">Porn</button>
    </div>
    <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 1em;">
      <button class="button" style="background-color:rgba(135,206,235,0.3)">Others</button>
    </div>
    </td>
    </tr>
    </tbody>
    </table>
</div>

This last class of communities are in fact not communities at all, with a low, almost non-existent level of interactivity (an order of magnitude lower than other communities) and low connectivity. They were grouped together because users often search for similar content.

These cluster do not lend themselves to any type of social analysis. One such cluster, **Porn** is an archetypical example of this class. The subreddits in this cluster do not seem to interact at all. Probably due to the inherent _non-interactive_ nature of this community: this is a **passive** cluster. All theme-related _interacting_ subreddits should be found in the **Adults content** community. Neither the balance theory nor the status theory were found to apply for this class.

## An evolution through time

Until now we have only considered the last state of the network. However the network evolves through time: some hyperlinks are created, the sentiment of the associated post might change. Let's examine the proportion of positive link created each month in the categories defined above.

{% capture html %}
{% include evolution.html %}
{% endcapture %}
{{ html | normalize_whitespace }} 

Using a simple regression analysis, the only communities that have a significant uptrend in the proportion of negative hyperlinks created per month are the **Gaming** and **Adult content** communities.
These observations might reveal some polarization of these two clusters, which could question the application of the _weak balance theory_ in a distant future. 

We also observed that during certain periods, some subreddits created significantly more negative hyperlinks than usual. We call these periods **conflicts**, as they might be caused by a coordinated attack of negative hyperlinks. We manage to confirm this hypothesis on the following three categories, for which conflicting periods systematically involved the creation of negative hyperlinks by popular subreddits:


<div class="container" style="display:flex; position:relative;width: 80%; border: none">
  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
      <button class="button" style="background-color:rgba(135,206,235,0.3)">Popular subjects</button>
  </div>

  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
    <button class="button" style="background-color:rgba(135,206,235,0.3)">Gaming</button>
  </div>

  <div class="row" style="margin: auto; margin-top: 1em; margin-bottom: 2em;">
  <button class="button" style="background-color:rgba(135,206,235,0.3)">Politics</button>
  </div>

</div>


One particular interesting example is the **Politics** category, for which we observed a massive increase in negative hyperlinks on November 2016, for the US elections. Also the participation to conflicts differ between the categories: **Politics** and **Popular subjects**  involve more subreddits (about 15-20%) than **Gaming** (only 5-6%). This means that conflits in **Politics** and **Popular subjects** tend to concern a larger proportion of the community. These results confirm the *polemic* nature of these categories, as it was noted in the previous [section](#local_scale).


## Conclusion

So, overall, what have we learned with this analysis of the Reddit network ? First, the network exhibit complex structures, as the usual balance and status theory do not seem to apply here, with negative hyperlinks that tend to be created by subreddits that participate more in the social network. Thus, accounting for the popularity of the subreddits creating the hyperlink is necessary. Doing so, we have found a prevalence for situations of the type _my friend of my friend is my friend_. 

Due to its complex structure and the diversity of subreddits, an analysis at a local scale reveals a classification of communities of subreddits into three categories: 
* the _interacting social communities_, such as **Gaming** and **Tech**, for which the weak balance theory holds
* the _polemic interacting social communities_, that includes **Politics** and **Popular subjects** communities, with a large presence of situations of the type _the enemy of my enemy is my enemy_.
* the non-communities clusters, that rarely interact, such as **Porn**.  

Lastly, we used a temporal analysis to detect periods of _conflicts_, characterised by a large increase in the number of newly created negative hyperlinks. In addition to having a uptrend for the proportion of such negative hyperlinks created per month, the **Gaming** category has conflicts caused by subreddits that tend to be more popular.


## References
[^1]: S. Kumar, W.L. Hamilton, J. Leskovec, D. Jurafsky. [Community Interaction and Conflict on the Web](https://cs.stanford.edu/~srijan/pubs/conflict-paper-www18.pdf). World Wide Web Conference, 2018

[^2]: F. Heider. The Psychology of Interpersonal Relations. John Wiley & Sons, 1958

[^3]: Davis, J. A. [Structural balance, mechanical solidarity, and interpersonal relations](http://snap.stanford.edu/class/cs224w-readings/davis63balance.pdf). American journal of sociology 68.4 (1963): 444-462.

[^4]: J. Leskovec, D. Huttenlocher, J. Kleinberg. [Signed networks in social media](https://cs.stanford.edu/people/jure/pubs/triads-chi10.pdf) Proceedings of the SIGCHI conference on human factors in computing systems. 2010.
