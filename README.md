### Self-Organizing Map implementation on the classification task
#

This project shows my simple approach to the problem. I was asked to classify types of the shafts, based on their arrangement in the terrain and also to look for similarities of the shaft classes, check if there are any differences between them etc. This information was to be used as a guideline for historical origin of the shafts.

### Data description

Data contains measurement of the mining shafts in Poland. First three columns are related to the research field and also, particular measurement characteristics. Section column could be treated as approximated target, as research fields were chosen by looking at the visual qualities of the shaft forms.

### Data columns

![](https://github.com/agnieszkalysak/som_mining_shafts/blob/master/datset_head.png)

- **section** - ID of the following research fields
- **ID** - identifier of the particular measurement
- **section_area** - area of the particular section (research field) in hectares
- **object_area** - area of the form, including embankment and shaft in square meters
- **rel_height** - relative height of the embankment around the shaft in meters
- **object_distance** - distance between shafs (holes) in meters

### Illustrative map

Below is a schematic map, this is rather a graphic, showing mining shafts shape and arrangement, not actual map, on which measurements were taken.

![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F4027889%2Fc73e31ea9a350ea013a27342bc59e05d%2Fsom%20map.png?generation=1589667421440855&alt=media)

Sections are showed on the image as dashed contours.
- **Section 1** - one huge shaft form
- **Section 2** - objects with single shaft inlet
- **Section 3** - small objects with single shaft inlet
- **Section 4** - objects with double shaft inlet
- **Section 5** - small objects, spread out densely, with single and double shaft inlet
- **Section 6** - densely distributed shafts with embankments connected and single shaft inlet
- **Section 7** - very small shafts with single shaft inlet
- **Section 8** - densely distributed shafts and shaft aggregations, poorly preserved

### Solution

I used MiniSOM package from PyPI.

`pip install MiniSom`

The parameters weren't tuned much. The data set was standarized before training.

`som = MiniSom(x = 21, y = 21, input_len = 4, neighborhood_function = 'gaussian', sigma = 1.5, learning_rate = 0.5)`

Below is a distance map between neurons. Markers correspond to the section IDs. SOM clustered shafts rather according to their origin in the terrain, including some exeptions of course. Blank spaces between markers can be interpret as borders between clusters, where bright color means very strong border and darker means border, which is not so defined.
So, red point could be interpret as the outlier in the data, group of dark green squares and blue diamonds are also strongly different from the other data. Other clusters are mixed, so there are similarities between them. I checked this hypothesis in the som_mining_shaft.ipynb file.

![](https://github.com/agnieszkalysak/som_mining_shafts/blob/master/dist_map.png)

The same plot with inverted colors of the distance map (for beter readability) and added legend.

![](https://github.com/agnieszkalysak/som_mining_shafts/blob/master/dist_map_i.png)

Plot, the same, as previous, with addition of the following measurement IDs.

![](https://github.com/agnieszkalysak/som_mining_shafts/blob/master/dist_map_i_l.png)

### Summary

Self-Organizing Map clusters with strong borders are the most different chunks of the data. That means section 0, 2, 3 differ from each other and from the rest of the dataset the most. Section 0 is highest object with only one shaft, section 2 contains high, regular spread shafts and section 3 contains high objects, randomly distributed across the terrain.

Other clusters are more similar to each other. Section 4 is divided into three parts, as there are shaft with long, medium and short distance between them. But, whole section 4 is similar to the section 3 in its character.

Section 5 looks almost identical to the section 6 for our SOM. Outliers from section 7 and 8 also share one cluster in the distance map.

The conclusion is - last measurements with small objects near to each other are very similar, where first measurements differ strongly in their height, area and arrangement in the terrain.

That was all! I hope you enjoyed this simple approach to the task, I would be happy for any suggestions and thoughts.

### References
Thanks to:
1. https://github.com/JustGlowing/minisom
