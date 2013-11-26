The goal is to provide a framework for WCS transformations which is general and flexible enough that it is easy to extend by adding new transformations or coordinate classes.

The WCS class is represented as a directed graph whose nodes are coordinate systems and edges are transformations
between them. This allows one to specify multiple coordinate systems and the transformations between them. 
(The [prototype](https://github.com/nden/code-experiments/blob/master/generalized_wcs_api/prototype/wcs.py) uses [networkx](http://networkx.github.io).)

Further advantage of this approach is that this can be extended by the transformations in the celestial coordinates package `astropy.coordinates` which is also represented by a graph and thus providing access to its claasses and transformations.
