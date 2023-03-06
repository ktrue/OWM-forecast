# OpenWeatherMap.org International forecast formatting script - multilingual

This script is based on the DarkSky DS-forecast.php script and uses the API kindly provided by [https://openweathermap.org](https://openweathermap.org).  A companion script **OWM-forecast-lang.php** contains English->language lookups for use by **OWM-forecast.php**.
The current languages supported are:

Afrikaans | български език | český jazyk | Català | Dansk | Nederlands | English | Suomi | Français | Deutsch | Ελληνικά | Magyar | Italiano | עִבְרִית | Norsk | Polski | Português | limba română | Español | Svenska | Slovenščina | Slovenčina | Srpski

In order to use this script you need to:

1.  Register for and acquire a free OpenWeatherMap.org API key.

    1.  Browse to [https://openweathermap.org](https://openweathermap.org) and sign up/in to acquire an API key.
    2.  Using the OWM site, subscribe the API key to your account to activate it for API usage.
        You'll see a message like the following if you have not subscribed the API feed in your OpenWeatherMap account:
        ![OWM-401-error](https://user-images.githubusercontent.com/17507343/223267899-1a1b0415-3d46-4785-b80f-bcb6d09ad161.jpg)
    3.  insert the API key in **$OWMAPIkey** in the OWM-forecast.php script or as **$SITE['OWMAPIkey']** in _Settings.php_ for Saratoga template users.
    4.  Customize the **$OWMforecasts** array (or **$SITE['OWMforecasts']** in _Settings.php_) with the location names, latitude/longitude for your forecasts. The first entry will be the default one for forecasts.
2.  Use this script ONLY on your personal, non-commercial weather station website.
3.  Leave attribution (and hotlink) to OpenWeatherMap.org as the source of the data in the output of the script.

Adhere to these three requirements, and you should have fair use of this data from openweathermap.org.

## Settings in the OWM-forecast.php script

```
// Settings ---------------------------------------------------------------
// REQUIRED: a openweathermap.org API KEY.. sign up at https://www.openweathermap.org/api
$OWMAPIkey = 'specify-for-standalone-use-here'; // use this only for standalone / non-template use
// NOTE: if using the Saratoga template, add to Settings.php a line with:
//    $SITE['OWMAPIkey'] = 'your-api-key-here';
// and that will enable the script to operate correctly in your template
//
$iconDir ='./forecast/images/';	// directory for carterlake icons './forecast/images/'
$iconType = '.jpg';				// default type='.jpg' 
//                        use '.gif' for animated icons from http://www.meteotreviglio.com/
//
// The forecast(s) .. make sure the first entry is the default forecast location.
// The contents will be replaced by $SITE['OWMforecasts'] if specified in your Settings.php

$OWMforecasts = array(
 // Location|lat,long  (separated by | characters)
'Saratoga, CA, USA|37.27465,-122.02295',
'Auckland, NZ|-36.910,174.771', // Awhitu, Waiuku New Zealand
'Assen, NL|53.02277,6.59037',
'Blankenburg, DE|51.8089941,10.9080649',
'Cheyenne, WY, USA|41.144259,-104.83497',
'Carcassonne, FR|43.2077801,2.2790407',
'Braniewo, PL|54.3793635,19.7853585',
'Omaha, NE, USA|41.19043,-96.13114',
'Johanngeorgenstadt, DE|50.439339,12.706085',
'Athens, GR|37.97830,23.715363',
'Haifa, IL|32.7996029,34.9467358',
); 

//
$maxWidth = '640px';                      // max width of tables (could be '100%')
$maxIcons = 10;                           // max number of icons to display
$maxForecasts = 14;                       // max number of Text forecast periods to display
$maxForecastLegendWords = 4;              // more words in forecast legend than this number will use our forecast words 
$numIconsInFoldedRow = 8;                 // if words cause overflow of $maxWidth pixels, then put this num of icons in rows
$autoSetTemplate = true;                  // =true set icons based on wide/narrow template design
$cacheFileDir = './';                     // default cache file directory
$cacheName = "OWM-forecast-json.txt";      // locally cached page from OWM
$refetchSeconds = 3600;                   // cache lifetime (3600sec = 60 minutes)
//
// Units: Temp,Baro,Wind,Rain,Snow,Distance
// 'si' = C,hPa,m/s,mm,mm,km
// 'ca' = C,hPa,km/h,mm,mm,km
// 'uk' = C,mb,mph,mm,mm,km
// 'us' = F,inHg,mph,in,in,km
// 
$showUnitsAs  = 'ca'; //

$charsetOutput = 'ISO-8859-1';        // default character encoding of output
//$charsetOutput = 'UTF-8';            // for standalone use if desired
$lang = 'en';	// default language
$foldIconRow = false;  // =true to display icons in rows of 5 if long texts are found
$timeFormat = 'Y-m-d H:i T';  // default time display format

$showConditions = true; // set to true to show current conditions box

// ---- end of settings ---------------------------------------------------
```

For Saratoga template users, you normally do not have to customize the script itself as the most common configurable settings are maintained in your _Settings.php_ file. This allows you to just replace the _OWM-forecast.php_ on your site when new versions are released.  
You DO have to add a **$SITE['OWMAPIkey'] = '_your-key-here_';** and a **$SITE['OWMforecasts] = array( ...);** entries to your _Settings.php_ file to support this and future releases of the script.

<dl>

<dt>$OWMAPIkey = 'specify-for-standalone-use-here';</dt>

<dd>This setting is for **standalone** use (do not change this for Saratoga templates).  
Register for a openweathermap API Key at https://openweathermap.org and replace _specify-for-standalone-use-here_ with the registered API key. The script will nag you if this has not been done.  

**For Saratoga template users**, do the registration at the openweathermap API site above, then put your API key in your _Settings.php_ as:  

$SITE['OWMAPIkey'] = '_your-key-here_';  

to allow easy future updates of the OWM-forecast.php script by simple replacement.</dd>

<dt>$iconDir</dt>

<dd>This setting controls whether to display the NOAA-styled icons on the forecast display.  
Set $iconDir to the relative file path to the Saratoga Icon set (same set as used with the WXSIM plaintext-parser.php script).  
Be sure to include the trailing slash in the directory specification as shown in the example above.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['fcsticonsdir']** to specify this value.</dd>

<dt>$iconType<dt>

<dd>This setting controls the extension (type) for the icon to be displayed.  
**='.jpg';** for the default Saratoga JPG icon set.  
**='.gif';** for the Meteotriviglio animated GIF icon set.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['fcsticonstype']** to specify this value.</dd>

<dt>$OWMforecasts = array(  <br>
// Location|forecast-URL (separated by | characters) <br> 
'Saratoga|37.27465,-122.02295', <br> 
'Auckland|-36.910,174.771', // Awhitu, Waiuku New Zealand <br> 
<br>
... <br> 
);</dt>

<dd>This setting is the primary method of specifying the locations for forecasts. It allows the viewer to choose between forecasts for different areas based on a drop-down list box selection.  
**Saratoga template users**: Use the _Settings.php_ entry for **$SITE['OWMforecasts'] = array(...);** to specify the list of sites and URLs.</dd>

<dt>$maxWidth</dt>

<dd>This variable controls the maximum width of the tables for the icons and text display. It may be in pixels (as shown), or '100%'. The Saratoga/NOAA icons are 55px wide and there are up to 8 icons, so beware setting this width too small as the display may be quite strange.</dd>

<dt>$maxIcons<dt>

<dd>This variable specifies the maximum number of icons to display in the graphical part of the forecast. Some forecast locations may have up to 8 days of forecast (8 icons) so be careful how wide the forecast may become on the page.</dd>

<dt>$cacheFileDir</dt>

<dd>This setting specifies the directory to store the cache files. The default is the same directory in which the script is located.  
Include the trailing slash in the directory specification.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['cacheFileDir']** to specify this value.</dd>

<dt>$cacheName</dt>

<dd>This variable specifies the name of the cache file for the OWM forecast page.</dd>

<dt>$refetchSeconds</dt>

<dd>This variable specifies the cache lifetime, or how long to use the cache before reloading a copy from openweathermap. The default is 3600 seconds (60 minutes). Forecasts don't change very often, so please don't reduce it below 60 minutes to minimize your API access count and keep it to the free Developer API range.</dd>

<dt>$showUnitsAs</dt>

<dd>This setting controls the units of measure for the forecasts. <br> 
 Units: Temp,Baro,Wind,Rain,Snow,Distance<br>
 'si' = C,hPa,m/s,mm,mm,km<br>
 'ca' = C,hPa,km/h,mm,mm,km<br>
 'uk' = C,mb,mph,mm,mm,km<br>
 'us' = F,inHg,mph,in,in,km<br>
<br>
**Saratoga template users:** This setting will be overridden by the **$SITE['OWMshowUnitsAs']** specified in your _Settings.php_.  
</dd>

<dt>$foldIconRow</dt>

<dd>This setting controls 'folding' of the icons into two rows if the aggregate width of characters exceeds the $maxSize dimension in pixels.  
**= true;** is the default (fold the row)  
**= false;** to select not to fold the row.  
**Saratoga template users:** Use the _Settings.php_ entry for **$SITE['foldIconRow']** to specify this value.</dd>

</dl>

More documentation is contained in the script itself about variable names/arrays made available, and the contents. The samples below serve to illustrate some of the possible usages on your weather website.

## Usage samples

```
<?php  
$doIncludeOWM = true;  
include("OWM-forecast.php"); ?>
```

You can also include it 'silently' and print just a few (or all) the contents where you'd like it on the page

```
<?php  
$doPrintOWM = false;  
require("OWM-forecast.php"); ?>  
```

then on your page, the following code would display just the current and next time period forecast:

```
<table>  
<tr align="center" valign="top">  
<?php print "<td>$OWMforecasticons[0]</td><td>$OWMforecasticons[1]</td>\n"; ?>  
</tr>  
<tr align="center" valign="top">  
<?php print "<td>$OWMforecasttemp[0]</td><td>$OWMforecasttemp[1]</td>\n"; ?>  
</tr>  
</table>
```

Or if you'd like to include the immediate forecast with text for the next two cycles:

```
<table>  
<tr valign="top">  
<?php print "<td align=\"center\">$OWMforecasticons[0]<br />$OWMforecasttemp[0]</td>\n"; ?>  
<?php print "<td align=\"left\" valign=\"middle\">$OWMforecasttext[0]</td>\n"; ?>  
</tr>  
<tr valign="top">  
<?php print "<td align=\"center\">$OWMforecasticons[1]<br />$OWMforecasttemp[1]</td>\n"; ?>  
<?php print "<td align=\"left\" valign=\"middle\">$OWMforecasttext[1]</td>\n"; ?>  
</tr>  
</table>
```

If you'd like to style the output, you can easily do so by setting a CSS for class **OWMforecast** either in your CSS file or on the page including the OWM-forecast.php (in include mode):


```
<style type="text/css">    
.OWMforecast {    
    font-family: Verdana, Arial, Helvetica, sans-serif;    
    font-size: 9pt;    
}    
</style>
```

## Installation of OWM-forecast.php

Download **OWM-forecast.php** and **OWM-forecast-lang.php** .

Download the [ **Icon Set** ](https://saratoga-weather.org/saratoga-icons2.zip) , and upload to ./forecast/images directory. This icon set is also used by advforecast2.php, WXSIM plaintext-parser.php, AW-forecast.php, DS-forecast.php, and WC-forecast.php scripts -- if you'd used any of them, you likely have the correct icon set installed already.

Change settings in **OWM-forecast.php** for the **$OWMforecast** address(s) and the address of the icons if necessary and upload the modified OWM-forecast.php to your website.

Ensure the permission on "OWM-forecast-json-{n}-{units}.txt" cache file(s) are at least 666 or 766 so the file is writable by the OWM-forecst.php script

**Demo:**[ **OWM-forecast-demo.php** ](https://saratoga-weather.org/OWM-forecast-demo.php)(note: uses UTF-8 only mode)  

**Download**:[ **Icon Set** ](https://saratoga-weather.org/saratoga-icons2.zip)(upload to your website in the **/forecast/images** directory)  

# Sample output
## English
![English Sample Output](https://user-images.githubusercontent.com/17507343/216797785-88f522e2-7100-4026-8a6c-2256d5082abd.jpg)
## English (Hourly)
![English Sample Output](https://user-images.githubusercontent.com/17507343/216797786-cadff259-e41d-438a-98a2-ba45333748fc.jpg)
## Ελληνικά (Greek)
![Greek Sample Output](https://user-images.githubusercontent.com/17507343/216797787-6947f3c2-5fcc-4745-bbb2-17dd3d55653d.jpg)
## Ελληνικά (Greek) Hourly
![Greek Sample Output](https://user-images.githubusercontent.com/17507343/216797788-fd75bd5b-3ffb-4780-a818-2acd3b7f6fb9.jpg)
