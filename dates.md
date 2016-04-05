# Dates

Lucee has a built in DateTime object.

##Creating a Date & Time Object

{% gist id="roryl/6a9839d7d9cbd52636afc35844e1aa3f",file="create_date_time.cfm" %}{% endgist %}

![](date.png)


Creating a Date only object using the createDate() function, simply creates a DateTime object, but the Time is 0. This is handy when time is not needed but only the date.

{% gist id="roryl/6a9839d7d9cbd52636afc35844e1aa3f",file="create_date_time.cfm" %}{% endgist %}

![](date.png)

##Working with DateTime objects
The Lucee DateTime object has a number of functons, refer to the [DateTime object reference](http://luceedocs.herokuapp.com/objects). We show an example here to get you started

###Comparing Dates

The following example shows comparing dates with each other. the dateTime.compare() function works on a date time. It returns -1 if the date is before another date, 1 if it is after, and 0 if they are the same.

{% gist id="roryl/6a9839d7d9cbd52636afc35844e1aa3f",file="date_compare.cfm" %}{% endgist %}

The echo's from this example return:

```
myDate should be before myDate2: -1
myDate2 should be after myDate: 1 
myDate should be equal to otherDate: 0
```

##Working with Timezones
A datetime object is independent of a specific timezone, it is only a offset in milliseconds from 1970-1-1 00.00:00 UTC (Coordinated Universal Time). When dumping a dateTime object, Lucee is actually formatting it for the timezone of the Lucee server, and that timezone is not in the datetime object itself.

To set a specific timezone for your application, use the `this.timezone' setting in the Application.cfc

{% gist id="roryl/6a9839d7d9cbd52636afc35844e1aa3f",file="Application.cfc" %}{% endgist %}

<noscript>
```
component {
	this.timezone = "UTC";
}
```
</noscript>

This means that the timezone only comes into play when you need specific information like hours in a day, minutes in a hour or which day it is since those calculations depend on the timezone. For these calculations, a timezone must be specified in order to translate the date object to something else. If you do not provide the timezone in the function call, it will default to the timezone specified in the Lucee Administrator (Settings/Regional), or the timezone specified for the current request using the function setTimezone. You can find a list of all available timezones in the Lucee administrator (Settings/Regional). Some examples of valid timezones:

- AGT (for time in Argentina)
- Europe/Zurich (for time in Zurich/Switzerland)
- HST (Hawaiian Standard Time in the USA)

###Always store UTC
When persisting dates to a database, you should always store dateTimes in UTC and convert them to the locale appropriate timezone for your user as a final step. This will keep your datetimes accurate and error free from timezone changes or user preference changes.

###Timezone Example
