### Models and Views
Rather than using widgets, it might be better to use a model view setup. This will strip data from representation and makes cross widget filtering easier. See a good tutorial [here](https://doc.qt.io/qtforpython/overviews/modelview.html)

*Note*: There are two approaches
1. Via QtsomeobjectWidgets -> containing data themself
1. Via QtsomeobjectViews + Models -> data is contained in the model and the view only is a visual representation of the data

#### Tree Models
What seemed a bit odd to me is that there are no standard Tree Model implementations. And one would always need to work of the `QtCore.QtAbstractItemModel` to implement [example1](https://gist.github.com/zhanglongqi/6994c2bc611bacb4c68f)



