---
layout: post 
title:  "Metabolic Network Visualization in D3.js" 
date:  2015-12-28
comments: true
---

In my postdoc research I've been starting to do some flux balance analysis to help understand metabolic pathways in various organisms. One of the persistent problems in this research is quickly visualizing flux solutions, where material flows from one metabolite to another are shown using something similar to a sankey diagram.

Some previous solutions to this problem seem to exist, namely [escher.github.io](escher.github.io) and others. These packages never quite seemed to work for my application though, and required drawing the entire network structure from scratch. I was intrigued by D3.js's [force directed layouts](https://github.com/mbostock/d3/wiki/Force-Layout), and therefore decided to hack something together to allow small-to-medium sized networks to be quickly visualized.

<iframe sandbox="allow-scripts allow-forms allow-same-origin" src="http://bl.ocks.org/pstjohn/raw/8f4aea594d881378f6fe/7b15059aa1f101d6e6be7dc605d21cc876f70fdb/" marginwidth="0" marginheight="0" scrolling="no" width="600" height="400"></iframe>

One of the crucial parts of extending D3's force-directed-layout to work with metabolic networks was proper handling of the reaction objects. As reaction networks can have hyperedges (for instance, an edge in which two metabolites come together to form a single product), they do not conform to rigorous definitions of a directed graph. I therefore expand the graph to include hidden reaction 'nodes', and then draw bezier curves which respect the reactant-product reaction stoichiometry. This algorithm is implemented in `function calculate_path(d, force)`, which finds the appropriate path angle and control points.

The full code is available as a gist [http://bl.ocks.org/pstjohn/8f4aea594d881378f6fe](http://bl.ocks.org/pstjohn/8f4aea594d881378f6fe), or in a more bug-free form as part of my [fork](https://github.com/pstjohn/cobrapy) of the cobrapy package. 
