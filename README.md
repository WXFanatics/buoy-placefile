## This is a fork of [https://github.com/ktrue/buoy-placefile](https://github.com/ktrue/buoy-placefile)

I will most likely create and/or modify the placefiles with my personal touches.

Modified Placefiles:

[Buoys.php](https://github.com/ktrue/buoy-placefile/blob/main/buoys.php) - Added the ability to adjust distance using the ?maxDistance= parameter for programs like Supercell Wx. Added the max distance currently set to the title of the placefile.  Removed lat/long indention. Capitalized the g in gust

![supercell-wx_4WwU0iCaKn](https://github.com/WXFanatics/buoy-placefile/assets/96398274/d88a0449-0fcc-455b-879a-51bf5f068a91)

# buoy-placefile
## GRLevelX placefile generator for NOAA NDBC buoy data

## Purpose:

This script set gets buoy data from www.ndbc.noaa.gov and formats a placefile for GRLevelX software
to display icons for buoys, and
mouse-over popups with text for the current observation from the buoy.

One script is to run via cron to gather the data routinely (*get-buoy-data.php*).  

The *buoys.php* script is to be accessed by including the website URL in the GRLevelX placefile manager window.

## Scripts:

### *get-buoy-data.php*

This script reads the two files from www.ndbc.noaa.gov and creates 
- *buoy-info-inc.php* file which contains the parsed and formatted metadata for each buoy and
- *buoy-data-inc.php* file which contains the parsed and formatted observation data for each buoy.

It should be run by cron every 5 minutes to keep the data current.  Keep in mind that
many buoys report only once per hour so loading more often won't result in 'new' data.

### *buoys.php*

This script generates a GRLevelX placefile from the *buoy-info-inc.php* and *buoy-data-inc.php*
files on demand by a GRLevel3 instance.  It will return buoy icons with popup info
for each buoy within 250 miles of the current radar selected in GRLevel3.
It requires the following files:

-   *buoy-info-inc.php* (produced by *get-buoy-data.php* for the current buoy metadata)
-   *buoy-data-inc.php* (produced by *get-buoy-data.php* for the current buoy observation data)
   

The script uses 2 icon files:  *buoy-icons.png*, *windbarbs-kt-white.png*

If you run the script for debugging in a browser, add `?version=1.5&dpi=96&lat={latitude}&lon={longitude}` to
the *buoys.php* URL so it knows what to select for display.

Additional documentation is in each script for further modification convenience.

## Installation

Upload the following files in a directory under the document root of your website.  (We used 'placefiles' in the examples below)

- *get-buoy-data.php*
- *buoys.php*
- *windbarbs-kt-white.png*
- *buoy-icons.png*
  
Set up cron to run *get-buoy-data.php* like:
```
*/5 * 1 * * * cd $HOME/public_html/buoy-placefile;php -q get-buoy-data.php > buoy-status.txt 2>&1
```
be sure to change the public_html/buoy-placefile to the directory where you installed the scripts.

Run the script once (to generate data) by `https://your.website.com/buoy-placefile/get-buoy-data.php`

That will show

```
get-buoy-data.php V1.00 - 24-Sep-2023 - webmaster@saratoga-weather.org
.. Loaded https://www.ndbc.noaa.gov/activestations.xml which has 264383 bytes.
.. saved XML to activestations.xml
.. activestations.xml created='2023-09-24T14:50:01UTC' buoy count='1323'
.. buoy counts by type
   372	buoy
    67	dart
   688	fixed
    55	oilrig
    89	other
    51	tao
     1	usv
.. saved buoy-info-inc.php with 1325 entries (includes 2 legend entries).
.. Loaded https://www.ndbc.noaa.gov/data/latest_obs/latest_obs.txt which has 105434 bytes.
.. saved raw TXT data to latest_obs.txt.
.. saved buoy-data-inc.php with 884 buoy entries.
.. Done
```

Then you can test the metar-placefile script by using your browser to go to
`https://your.website.com/buoy-placefile/buoys.php?version=1.5&dpi=96&lat=37.0&lon=-122.0`

If that returns a placefile, then add your placefile URL into the GRLevelX placefile
manager window.

Sample *buoys.php* output:
```
; placefile with conditions generated by buoys.php V1.00 - 24-Sep-2023 - webmaster@saratoga-weather.org
; Generated on Sun, 24 Sep 2023 14:53:05 +0000
;
Title: NDBC Buoy Observations - Sun, 24 Sep 2023 14:53:05 +0000 - Saratoga-Weather.org 
Refresh: 5
Color: 255 255 255
Font: 1, 12, 1, Arial
IconFile: 1, 19, 43, 2, 43, windbarbs-kt-white.png
IconFile: 2, 17, 17, 8, 8, buoy-icons.png
Threshold: 999

; generate 41013 Frying Pan Shoals, NC at 33.441,-77.764 at 248 miles S 
Object: 33.441,-77.764
Threshold: 999
Icon: 0,0,210,1,2
Text: -17, 13, 1, 78
Color: 0 148 255
Text: -17, -13, 1, 79
Color: 255 255 255
Icon: 0,0,000,2,2,"BUOY 41013 Frying Pan Shoals, NC\n   (33.441,-77.764)\n----------------------------------------------------------\nTime:  Sunday, Sep 24 (14:10Z)\nTair:  77.9F (25.5C)\nTdew:  69.4F (20.8C)\nTwtr:  78.8F (26.0C)\nWind:  SSW 9 mph (14 km/h, 8 kt)\n       gust 11 mph (18 km/h, 10 kt)\nPres:  29.96 inHg (1014.6 hPa)\nAPD:   5.5 sec\nWvDir: ESE\n\nPgm:   NDBC Meteorological/Ocean\nOwner: NDBC\n----------------------------------------------------------"
End:

; generate 41024 Sunset Nearshore, NC (SUN2) at 33.837,-78.477 at 233 miles SSW 
Object: 33.837,-78.477
Threshold: 999
Icon: 0,0,70,1,1
Text: -17, 13, 1, 73
Color: 0 148 255
Text: -17, -13, 1, 76
Color: 255 255 255
Icon: 0,0,000,2,2,"BUOY 41024 Sunset Nearshore, NC (SUN2)\n   (33.837,-78.477)\n----------------------------------------------------------\nTime:  Sunday, Sep 24 (13:08Z)\nTair:  72.5F (22.5C)\nTdew:  68.2F (20.1C)\nTwtr:  75.6F (24.2C)\nWind:  ENE 7 mph (11 km/h, 6 kt)\n       gust 9 mph (14 km/h, 8 kt)\nPres:  29.94 inHg (1013.9 hPa)\n\nPgm:   IOOS Partners\nOwner: CORMP\n----------------------------------------------------------"
End:

```

## Acknowledgements

Special thanks to Mike Davis, W1ARN of the National Weather Service, Nashville TN office
for his testing/feedback during development.   

## Sample output

![buoy-placefile](https://github.com/ktrue/buoy-placefile/assets/17507343/614de01c-b4bb-437c-92fd-99fb548ecef5)
