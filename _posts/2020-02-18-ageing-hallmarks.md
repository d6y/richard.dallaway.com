---
title: |
    Ageing hallmarks
author: Richard Dallaway
layout: post
---

I've been learning about biological ageing over the last few months for a project. There's so much to take in just on the basics, let alone the new research coming out. To help myself make sense of it, I've decided to write up my notes on various papers, starting with _The Hallmarks of Aging_.

<!-- break -->

# Sources

The primary sources for this post are:

- _[The Hallmarks of Aging][hoa]_ by López-Otín, Carlos et al, published in _Cell_ in 2013.

- Week 7 of _[Epigenetic Control of Gene Expression][coursera]_, a course I've mentioned in a [post on learning genetics and epigenetics][lage].

There are full references and other links at the end of this post.

_I'm a software/AI person trying to understand the language and ideas in ageing. If you spot places where I've misunderstood, I lack a clue, or you have suggestions, do get in touch. [Twitter] or [LinkedIn] are probably the easiest ways to grab me._

# What is ageing?

Ageing is a progressive decline in how we function. There are other aspects of ageing in society, but I'm thinking only of the biological ageing here. (If you're interested in a broader treatment, take a look at the [Sep/Oct 2019 issue of MIT Technology Review][mtr], which includes articles on product design and attitudes to ageing.)

Where does this decline come from? In general, it is:

- the accumulation of damage; and

- over-activity of some processes leading to disease.

## Why does it matter?

This age-based deterioration is the leading risk factor for many diseases, including diabetes, cardiovascular, and neurodegenerative problems. Some of these conditions have no long term treatment, let alone cure. 

And yet some people can live without these problems. They have both a long healthspan and lifespan. We now know genetics and biochemistry control ageing (more on that later). So the question is: can _anyone_ potentially live without having to experience long periods with one or more of these diseases? If so, what kinds of lifestyles and treatments might give us that?

## Terminology

There are a few common words and phrases that are worth knowing before diving in any deeper:

<ul class="def-list">
  <li><strong>Ageing</strong> vs <strong>Aging</strong>: The UK vs US spelling.</li>
  <li><strong>Longevity</strong>: “increased longevity” means living for longer.</li>
  <li><strong>Healthspan</strong> is how long you live without disease.</li>
  <li><strong>Chronological age</strong> is… your age. How long you’ve been alive for.</li>
  <li><strong>Biological age</strong>: if you are ageing faster (or more slowly), your biological age would be above (or below) your chronological age. There’s no definitive way to measure this. Various models exist that combine multiple factors (lung capacity, cholesterol, skin elasticity, and so on) into a single value, but the ideal would be to find a biomarker for ageing.</li>
  <li>A <strong>biomarker for ageing</strong>. This would be a standard, validated, test from, say, blood or saliva, which would give a biological age. This is important because it would make it possible to measure the effects of interventions.</li>
  <li><strong>Senescence</strong>: the precise term for biological ageing.</li>
  <li><strong>Tissue</strong>, a collection of cells with similar function or structure. I might say “heart tissue”, “lung tissue”, and so on.</li>
  <li><strong>Signalling Pathways</strong>. I don’t have a grasp on the biological terms of “circuit” and “programs”, but I think I get “signalling pathways” now. These describe the influence of, say, proteins, and how they inhibit or stimulate other proteins. I’ll show one later in this post.</li>
</ul>

# Hallmarks of ageing

In 2013, _The Hallmarks of Aging_ proposed nine common features of ageing. To be included a hallmark had to:

- be a normal part of development (as opposed to the result of being hit on the head or something external);

- when increased, ageing speeds up; and

- ideally, when a hallmark is reduced or prevented, ageing is slowed.

I'll list out the nine hallmarks below, but I think it helps to know they fall into one of three groups:

1. causes of damage;

2. response to damage; and

3. causes of the phenotype (appearance) of ageing.

Let's look at the nine hallmarks by group.

## The causes of damage
Bear in mind that cause and correction aren't fully worked out. But the items listed below are hallmarks because they have shown increasing them speeds up ageing (and in some cases the reverse has been shown).

- Genetic: that is, DNA damage we accumulate, or specific premature ageing diseases. The genome has lots of repair mechanisms but isn't perfect. Replication mistake or external factors (such as smoking) lead to mutations. This is  a broad category and interacts with many of the other hallmarks. Cancer is the headline consequence.

- Epigenetic changes. Epigenetic marks are chemical additions to your genome which changes how your genes are expressed (if they are on, or off, in a particular tissue, for example). "Epigenetic drift" is the name for the stochastic errors that naturally occur over time and between individuals. The accumulation of these changes is associated with ageing. We'll see some a specific example later. There's also an interaction with the genome: some epigenetic changes may lead to genome instability, which may impact epigenetic marks, which may lead to more genome instability, and so on.

- Telomere shortening. Telomeres are compact regions of DNA at the start and end of the chromosomes. They prevent the DNA repair machinery from treating the ends as a break in a chromosome to be repaired. When a cell divides, and DNA is copied, but not to the end. The telomeres become shorter over time, and eventually, the cell shuts down. 

- Loss of proteostasis, which means not being able to generate and remove proteins at the right rate, or correctly fold proteins. Folding problems are associated with Parkinson's and Alzheimer's (as well as cataracts).

## Responses to damage 
This second group of hallmarks are beneficial in low doses (offering some kind of protection or repair), and bad when high (which happens during ageing).

- Deregulated nutrient sensing. The idea here is that you survive for longer if you have lower rates of cell growth and metabolism (giving lower rates of damage). In other words, decreased nutrient sensing increases longevity. Although this happens as we age, there's strong evidence (not just in mice) that explicit dietary restriction increases lifespan and healthspan.

- Mitochondrial dysfunction. The "powerhouse of the cells" lose efficiency over time. Terrible news, but surprisingly (to me) limited "mitochondrial poisoning"  increases efficiency. This is an example of how a small amount of stress can help as our bodies compensate (but only up to a point).

- Cell senescence is a situation where cells no longer divide and become inactive. It's a protective mechanism (against cancer) in the young, but an increasing problem over time.

## Causes of the appearance of ageing
The final two hallmarks are the result of the other seven we've seen:

- Stem cell exhaustion. Cuts heal when epidermal stem cells divide to create new skin cells. When exhausted, they don't divide as fast, meaning cuts take longer to heal. At an extreme, they don't heal at all. That's an example of stem cell exhaustion, but it applies to the immune system and other tissues. It is a "consequence of multiple types of aging-associated damages and likely constitutes one of the ultimate culprits of tissue and organismal aging." (_The Hallmarks of Aging_, p. 1206).

- Altered intercellular communication. In addition to changes at the cell level, there are changes in the interaction between cells. This appears to cover a wide area: reduce immune response, increased inflammation, diabetes, atherosclerosis, muscle weakness, and generally a whole lot of undesirable changes.

In summary:
> "The primary hallmarks could be the initiating triggers whose damaging consequences progressively accumulate with time. The antagonistic hallmarks, being in principle beneficial, become progressively negative in a process that is partly promoted or accelerated by the primary hallmarks. Finally, the integrative hallmarks arise when the accumulated damage caused by the primary and antagonistic hallmarks cannot be compensated by tissue homeostatic mechanisms. Because the hallmarks co-occur during aging and are interconnected, understanding their exact causal network is an exciting challenge for future work" (_The Hallmarks of Aging_, p. 1209).

# Suggestions for treatments

There's plenty of evidence to accompany the hallmarks, and that evidence suggests interventions to reduce ageing. Here are a couple of them.

## SIRT6

There are a family of proteins called sirtuins, one of which is SIRT6. SIRT6 is involved in laying down histone acetylation (a sort of epigenetic change), and this reduces with age. 

In mice, reduced SIRT6 leads to defective DNA repair, and accelerates ageing. Increased SIRT6 expression increases lifespan. It may also play a role in reducing inflammation.

You can see why there's a search for a drug that can have this effect, via this mechanism, without causing side effects.

## mTOR

Take a look at the signalling pathway relating ageing and dietary restriction (this is figure 4A from _The Hallmarks of Aging_, p. 1202):

<img src="/img/posts/2020/2020-02-18-ageing-hallmarks-fig4a.png" width="455" height="367">

The diagram is mapping out the nutrient sensing hallmark. We can see that mTOR, an enzyme, has a role in increasing ageing, while dietary restriction inhibits mTOR. This is one way in which to understand how nutrient sensing impacts ageing.

This suggests targeting mTOR may have benefits for ageing. Rapamycin, a compound that inhibits mTOR, is being investigated for this.


# Summary

We've seen:

- a little bit of background on ageing;

- the nine hallmarks of ageing, where a hallmark is a natural part of development, and increasing or decreasing the hallmark effects ageing; and

- a couple of examples of how the mechanisms and evidence play into those hallmarks to suggest treatment.

The paper is important because it gives a framework for thinking about ageing and how we might improve healthspan. Even if you don't read the paper, have a look at figure 7. It shows possible treatments (based on experience with mice).

<img src="/img/posts/2020/2020-02-18-ageing-hallmarks-fig7.jpg" width="769" height="501">

If you're interested in learning more, scroll down through the reading below. Don't miss the short video at the bottom.

[lage]: https://richard.dallaway.com/2019/12/31/genetics-epigenetics.html
[coursera]: https://www.coursera.org/learn/epigenetics/
[Twitter]: https://twitter.com/d6y
[LinkedIn]: https://www.linkedin.com/in/richarddallaway/

# Further reading

[hoa]: https://doi.org/10.1016/j.cell.2013.05.039
[facing]: https://www.nature.com/articles/s41586-018-0457-8

 <ul class="def-list">
  <li>
    López-Otín, Carlos et al (2013) The Hallmarks of Aging.
  Cell, Volume 153, Issue 6, 1194 - 1217, <a href="https://doi.org/10.1016/j.cell.2013.05.039">https://doi.org/10.1016/j.cell.2013.05.039</a>
  </li>
  <li>
    Partridge L, Deelen J, Slagboom PE. Facing up to the global challenges of ageing. Nature. 2018;561(7721):45–56. <a href="https://www.nature.com/articles/s41586-018-0457-8">doi:10.1038/s41586-018-0457-8</a>
  </li>
</ul> 

## Popular science and news

[mindscape]: https://www.preposterousuniverse.com/podcast/2018/10/01/episode-16-coleen-murphy-on-aging-biology-and-the-future/
[rev]: https://www.goodreads.com/book/show/12414734-the-epigenetics-revolution
[mtr]: https://www.technologyreview.com/magazine/2019/09/
[guardian]: https://www.theguardian.com/science/2019/dec/21/scientists-harness-ai-to-reverse-ageing-in-billion-dollar-industry
[economist]: https://www.economist.com/the-economist-explains/2016/08/15/how-medical-science-hopes-to-slow-down-ageing
[video]: https://www.youtube.com/watch?v=leI70P-rEAk

<ul class="def-list">
  <li>
    The Mindscape podcast from Oct 2018 features a conversation with <a href="https://www.preposterousuniverse.com/podcast/2018/10/01/episode-16-coleen-murphy-on-aging-biology-and-the-future/">Coleen Murphy on Aging, Biology, and the Future</a>. 1hr.
  </li>
  <li>
    Chapter 13 of <a href="https://www.goodreads.com/book/show/12414734-the-epigenetics-revolution">The Epigenetic Revolution</a> is on ageing.
  </li>
  <li>
    MIT Technology Review, <a href="https://www.technologyreview.com/magazine/2019/09/">Sep/Oct 2019 issue</a> is on longevity, covering biology, medicine, society’s perceptions of ageing, and product design.
  </li>
  <li>
    <a href="https://www.economist.com/the-economist-explains/2016/08/15/how-medical-science-hopes-to-slow-down-ageing">How medical science hopes to slow down ageing</a>, <em>The Economist</em>, 15 Aug 2016. This is a short explainer on the discoveries driving ageing research.
  </li>
  <li>
    <a href="https://www.theguardian.com/science/2019/dec/21/scientists-harness-ai-to-reverse-ageing-in-billion-dollar-industry">Scientists harness AI to reverse ageing in billion-dollar industry</a>, <em>The Guardian</em>, 21 Dec 2019. This item is on the recent focus and investment in the race “to find proven ways to help people live longer, healthier lives”.
  </li>
  <li>
    A fantastic 10-minute <a href="https://www.youtube.com/watch?v=leI70P-rEAk">video on <em>Extending Human Lifespan</em></a> by Kristen Fortney. It covers the ideas, history, and interventions relating to extending lifespan (thank you Suran for this link)
  </li>
</ul>