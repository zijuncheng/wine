# Wine Recommendation Based on Flavors

What's the best substitute for a particular bottle? This is perhaps one of the most frequently asked questions when people are shopping for wine. While it is true that a 2009 red-blend from Margaux could very likely satisfy you, but it certainly has a price to match. Many people might sought a more affordable alternative: perhaps a vintage wine from a later year in the same chateau (vineyard), a red-blend from the same year but a less renowned area such as Pays D'oc, or a bottle from a completely different region with a similar taste. This exploratory project provides a way to determine a substitute by considering multiple factors: appellations, grape varieties, production countries and most importantly, its tastes by reviews.

As an example, consider the following vineyard Morisoli from Napa Valley:

<img src="pics/red_origin.jpg" align="center" style="height: 200px"/> 

The closest substitute of their 2012 production --'Elyse 2012 Morisoli Vineyard Zinfandel (Rutherford)'.-- is the following according to the algorithm:

| <img src="pics/red1.jpg" align="center" style="height: 200px"/>        | <img src="pics/red2.jpg" align="center" style="height: 200px"/>            |<img src="pics/red3.jpg" align="center" style="height: 200px"/> |<img src="pics/red4.jpg" align="center" style="height: 200px"/> |
| --- |---| --- | --- |
| Stonestreet 2006 Christopher's Cabernet Sauvignon (Alexander Valley)* | Aquinas 2014 Cabernet Sauvignon (Napa County-Sonoma County-Lake County)* | Cakebread 2012 Benchland Kenwood 2010 Artist* |Series Cabernet Sauvignon (Sonoma County)* |

*All pictures are from https://www.wine-searcher.com/

This document include the following:

  - [Installation](#installation)
  - [Data Description](#data-description)
  - [Methodology](#methodology)
  - [Findings](#findings)
  - [Further Studies](#further-studies)
  
### Installation
----------------------------------
##### Source Data 
  - A self-made note based on the book [Wine Folly: The Essential Guide to Wine](wine_folly)
  - [Kaggle Wine Data](kaggle)
  
##### Download Intermediate Data to run the notebooks
- all the pickle files in the folder "pkl_data"

##### Install the Requirements
  Install the requirements using ```pip install -r requirements.txt```
  
  The following third-party packages are essential for this project:
   - [Instruction on installing glove-python](glove_python)
  (Technical reference on the GloVe model can be found [here](GloVe))
   

Please refer to the links above if you run into any problems.

### Data Description
----------------------------------

##### Notes from Wine Folly (NWF)
  The following factors are summarized in a spreadsheet based on the book Wine Folly: The Essential Guide to Wine:
  - Wine varieties
  - Places where the wine or the grape is produced/grown today
  - Properties (e.g., levels of dryness)
  - Possible Flavor Profiles 
   
##### Wikipedia(Wiki)
   For each of the possible flavors in the notes above, details are added to the learning process based on their Wikipedia page
   
#####  Online Review (OR)
   This data is from Kaggle and is originally a scrapped source from [WineEnthusiast](winewag).
   The columns used are "title","variety", "country" and "description". The last column "description" is a short paragraph of a review provided by a certified sommelier.
   

### Methodology
----------------------------------
##### Files
- Wine_Note_Wiki_Learning.ipynb
- Wine_OnlineReview_Learning

##### GloVe Model
- The project relies heavily on the GloVe Model where a vector is assigned to a word based on its context (or 'corpus')
- 3 layers of information are added to the corpus to help its understanding of a particular word involving flavor: NWF, Wiki and OR (details have been mentioned above). 
  - NWF sets the foundation of the model's wine knowledge so that it acknowledges the boundaries of different varieties: for example, Chardonnay and Merlot can never be similar due to their distinct flavors and inherent difference of the grape varieties. However, NWF also sets forward some possible linkage between some categories of wines: for instance, Cabernet Sauvignon and Bordeaux Blend, despite of their different wine varieties, could be similar due to their widely overlapping flavors. 
  - Wikipedia page on the flavors helps to group similar tastes closer in the vector space. For example, after learning from Wiki, peach, apricot and nectarine became close.
  
  Here's some results (mainly for flavors) after learning from NWF and Wiki:
  
  <img src="pics/wine_flavor1.png" align="center" style="height: 250px"/>
  
  <img src="pics/wine_flavor2.png" align="center" style="height: 250px"/>
  
  <img src="pics/wine_flavor3.png" align="center" style="height: 250px"/>
  
  -OR provides specific information on a particular bottle. This is the main subject to learn from. NWF and Wiki set foundations to this third stage of unsupervised learning.

##### Distance Functions
- Both Euclidean and Cosine Distance are used. They both provide reasonable results. However, it is worth noting that when less information are presented, cosine delivers a more consistent answer.

### Findings
----------------------------------

The algorithm works quite well in general. For the examples tested, the algorithm is always able to find a substitute of either the same kind of the wine or a possible different kind that can have similar flavors based on the review (e.g, Zinfindel and Cabernet Sauvignon in the beginning example of this document). The substitute is often from the same region and usually from the same year. The flavors based on review are also similar in that there are many overlapping key words, as expected.


### Limitations
----------------------------------
- A better criteria to judge the effectiveness of  the algorithm (maybe an A/B test)
- A more recent dataset by running the scrapper
- The details of different vineyard from Wikipedia might improve the learning process.
- The word learning could be improved. After enough iterations of learning, the algorithm still tries to put words together just because their similarities in their "forms" instead of their actual meanings: white gravel and white peach in the first word graph, for example.

[//]: # 


   [wine_folly]: https://www.amazon.com/Wine-Folly-Essential-Guide/dp/1592408990/ref=sr_1_2?crid=4KTQHE67IH5R&keywords=wine+folly&qid=1574797120&sprefix=wine-folly%2Caps%2C125&sr=8-2
   [glove_python]: https://github.com/maciejkula/glove-python/wiki/Installation-on-Windows
   [GloVe]: https://nlp.stanford.edu/projects/glove/
   [kaggle]: https://www.kaggle.com/sudhirnl7/wine-recommender
   [winewag]: https://www.winemag.com/?s=&drink_type=wine
