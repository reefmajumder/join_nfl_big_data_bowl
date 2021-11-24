
<img src="extras/bdb.jpg" align="center" />

Welcome to the data homepage for the NFL's Big Data Bowl.

For those interested in trying NFL tracking data via [Next Gen Stats](https://nextgenstats.nfl.com/), there is a style guide with references to each data set and each variable, a list of FAQs related to player tracking data and this contest, and a tutorial on how to visualize and animate the player tracking data using the , and one game of tracking information.

Data Description
-------------------------------

1.  Player tracking data one 2017 game. See <>. Tracking data is stored as a unique .csv file: `tracking_gameId_[gameId].csv`, where `[gameId]` is a unique, 10-digit identifier for each game.

2.  Player, play, and game-level data that correspond to the tracking data. See <> for each of these .csv files.

3.  A Data schema, which contains information on each of the variables in the data set, as well as the *key* variables needed to link the data sets together. See <>.

4.  A list of Data FAQs. See <>.


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

### Animating the data

The following code animates each player that was on the field. As one note, the code is flexible, such that plays at different parts of the field could feature different boundaries. As a second, the x-axis and y-axis coordinates are flipped.

``` 

``` 


## General field boundaries



## Specific boundaries for a given play


## Ensure timing of play matches 10 frames-per-second


```

![](man/figures/README-unnamed-chunk-3-1.gif)
