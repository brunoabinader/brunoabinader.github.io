---
title: "New kid on the block: Plasma Water Animation"
date: 2010-04-19
---

I'm glad to make available a fresh new Plasma animation: WaterAnimation. This animations uses the ripple effect to produce a liquefied behavior on the target widget. An example of the animation behavior can be seen below, enjoy! :smile:

<iframe width="600" height="400" style="display:block;margin-left:auto;margin-right:auto" src="https://www.youtube.com/embed/e7qsEpSgGZk" title="Plasma - KDE Water Animation | Crismon's Blog" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you're curious about how this works, the ripple effect uses a pair of matrices with the same size as the widget, providing the previous and current wave heigths. These wave heights are used to calculate the water reflection. If the widget gets resized, these matrices are resized as well, so the animations keeps on going in every situation.
