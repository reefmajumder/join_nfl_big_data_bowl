
<img src="extras/bdb.jpg" align="center" />

Welcome to the data homepage for the NFL's Big Data Bowl.

For those interested in trying NFL tracking data via [Next Gen Stats](https://nextgenstats.nfl.com/), there is a style guide with references to each data set and each variable, a list of FAQs related to player tracking data and this contest, and a tutorial on how to visualize and animate the player tracking data using the , and one game of tracking information.

Some questions that I would try to find answers to:

* What are coverage schemes (man, zone, etc) that the defense employs? What coverage options tend to be better performing?
* Which players are the best at closely tracking receivers as they try to get open?
* Which players are the best at closing on receivers when the ball is in the air?
* Which players are the best at defending pass plays when the ball arrives?
* Is there any way to use player tracking data to predict whether or not certain penalties – for example, defensive pass interference – will be called?
* Who are the NFL’s best players against the pass?
* How does a defense react to certain types of offensive plays?
* Is there anything about a player – for example, their height, weight, experience, speed, or position – that can be used to predict their performance on defense?

![](assets/pet-records.gif)

setup

```
git clone https://github.com/reefmajumder/join_nfl_big_data_bowl.git
cd pet-days/
jupyter nfl-big-data-bowl-notebook
```

How does Next Gen Stats work ?

* Every NFL player on the field (both on offense and defense) during active plays is tracked in terms of the capturing of their real-time location data, speed and acceleration
Sensors throughout the stadium track radio frequency identification (RFID) tags placed within the players' shoulder-pads, charting movements in a highly accurate manner
* The actual football is also tracked as an independent entity, opening up intriguing possibilities for analysis...
* All real-time data is processed entirely on an advanced machine learning based Amazon Web Services (AWS) infrastructure. Data is streamed towards the NFL stats page. FYI, Data scientists call this a pipeline.
* RFID tags: Designed by Zebra Technologies (official on-field player-tracking technology partner of the NFL), they consist of an integrated circuit (IC) attached to an antenna typically a small coil of wires plus some protective packaging as determined by the application requirements. They are active in this case and not passive.
* Follow NextGenStats Twitter, you will never look at NFL football the same again
* These real-time stats on players create a deeper fan experience and the NFL should be complimented on their choice to push the boundaries of analytics


Official rules
--------------

A complete set of official rules for the Big Data Bowl can be found [here](http://ops.nfl.com/big-data-bowl).

What player tracking data looks like
------------------------------------


### Reading/cleaning of the data

Firstly, the data needs to be cleaned. Missing values were handled. Wind speed contained inconsistent data which was fixed by writing a custom function.

``` 
def windspeed(x):
    x=str(x)
    if x.isdigit():
        return int(x)
    elif (x.isalpha()):
        return 0
    elif (x.isalnum()):
        return int(x.upper().split('M')[0])                             #return 12 incase of 12mp or 12 MPH
    elif '-' in x:
        return int((int(x.split('-')[0])+int(x.split('-')[1]))/2)   # return average windspeed incase of 11 - 20 etc..
    else:
        return 0
```

### Using keras and k-fold cross validation

``` 
from sklearn.model_selection import KFold
kfold=KFold(n_splits=3,shuffle=True)

for train_ind,val in kfold.split(X,y):
    
    x_train,xval = X.iloc[train_ind],X.iloc[val]
    y_train,yval= y.iloc[train_ind],y.iloc[val]
    
    y_train=transform_y(x_train,y_train)
    y_val=transform_y(xval,yval)
    
    model=None
    model=create_model()
    
    history=model.fit(x_train,y_train,epochs=20,validation_data=[xval,y_val],verbose=1)
    print('validation accuracy : {}'.format(np.mean(history.history['val_accuracy'])))

```

![](man/figures/README-unnamed-chunk-3-1.gif)
