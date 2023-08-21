# Data Science Assessment

## Problem 1

In this question, we are seeing the death tolls of each year's deadliest natural disaster in the 20th and 21st century. Natural disasters include earthquakes, floods, cyclones, etc. Let's first take a quick look at some of the events in these years

| Year | Death toll  | Event                                | Countries affected | Type              | Date         |
|-----:|:------------|:-------------------------------------|:-------------------|:------------------|:-------------|
| 1900 | 6,000–8,000 | 1900 Galveston hurricane             | United States      | Tropical cyclone  | September 9  |
| 1901 | 9,500       | 1901 eastern United States heat wave | United States      | Heat wave         | June–July    |
| 1902 | 29,000      | 1902 eruption of Mount Pelée         | Martinique         | Volcanic eruption | April–August |
| 1903 | 3,500       | 1903 Manzikert earthquake            | Turkey             | Earthquake        | April 28     |
| 1904 | 400         | 1904 Sichuan earthquake              | China              | Earthquake        | August 30    |

| Year | Death toll | Event                                    | Countries affected                                       | Type                | Date          |
|-----:|:-----------|:-----------------------------------------|:---------------------------------------------------------|:--------------------|:--------------|
| 2001 | 20,005     | 2001 Gujarat earthquake                  | India                                                    | Earthquake          | January 26    |
| 2002 | 1,030      | 2002 Indian heat wave                    | India                                                    | Heat wave           | May           |
| 2003 | 72,000     | 2003 European heat wave                  | Europe                                                   | Heat wave           | July – August |
| 2004 | 227,898    | 2004 Indian Ocean earthquake and tsunami | Indonesia, Sri Lanka, India, Thailand, Maldives, Somalia | Earthquake, Tsunami | December 26   |
| 2005 | 87,351     | 2005 Kashmir earthquake                  | India, Pakistan                                          | Earthquake          | October 8     |

Firstly we have a bar plot of death toll over the years. The different colors represent different types of natural disasters. The outstanding feature of this plot is the 1931 China floods that cause over 2 million deaths. And since this number is overwhelmingly high, the other bars are compressed. So in the next plot, we are plotting the same graph without the 1931 disaster to get a picture of other years' situations

![](Figs/unnamed-chunk-5-1.png)<!-- -->

In the following plot, we can see the top death tolls are 1976 Tangshan earthquake with over 400,000 deaths, 1970 Bhola cyclone with 300,000 deaths, 1920 Haiyuan earthquakes with over 200,000 deaths, 1975 Typhoon Nina with over 200,000 deaths.

The graphs give us a clear image that earthquake causes much higher damage than other disasters, followed by cyclone/typhoon in general. They both happen the most frequently, and cases higher death tolls. Removing the 1931 disasters, the next most severe disasters are floods and heat waves, shown in green color in the plot. Volcanic eruption, avalanche, landslide, and limnic eruption are less frequent and less severe comparatively.

![](Figs/unnamed-chunk-7-1.png)<!-- -->
