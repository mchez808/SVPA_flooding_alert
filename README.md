Snowqualmie Valley Preservation Alliance Flood Alert Bot
========================================================

Testing

### GENERAL NOTES:
The flood alert bot gathers data from water gages, digests that data and moves it to the people who need to know it.<br>
The program should also be fairly portable/abstract/OO, to allow for it to be adapted to new systems.<br>
The current iteration should interact with a Discord bot to communicate with people using discord or email.<br>
<br>**2020-12-19**


### DEPENDENCIES:
* tested on python 3.9
* pandas
* lxml
* will need discord.py


Currently uses pandas to read tabular tables.  
May be used for series processing later.  
The latest version of windows has a bug that interacts poorly with numpy. So as of now if installing on a windows machine install pandas 1.1.3, which should install numpy 1.19.3.  
In terminal:  
pip install pandas==1.1.3
<br>**2020-12-19**


### DEVELOPER NOTES:
Project notes and task list, for dev use:
Every few minutes a class activates a retrieval routine.
At a time based on past data it retrieves information from the guages.
It sends a message to each guage and then stores that information in a class
associated with that guage, deleting old data.
Then:
It looks through the predicted flood level data, and if it exceeds a certain level
(stored in class) and unless told otherwise will send an alert.
The always on class also responds to requests from the bot source:
Finds the latest historical data on a particular guage, as well as a set of relevant
data from that guage including warning levels.
Classes:
* FloodAlertBot runs and accepts events. Flood Alert Bot
* Gage, holds a snapshot of guage history and returns relevant information


### Tasks:

[x] Every startup, make gage classes
* Each guage has an object

[x] Start up and wait_time.
* Every wait_time amount of time, the process will do something

[x] On wait_time update data (and print)
* print proof data is up to date

[x] On getting data, show appropriate data
* E.g. gage, last few readings, all predictions, summary:
* Expected high point
* Expected time above thresholds
* relationship between high point and thresholds
* Recent history.


[x] Find gages that are over minimum threshold and print something else

[x] instantiate multi-threading
* Waits for requests, as timer is running

[] handle race conditions in threading via locking

[] On startup, gages are read from a separate file.
* "Gages.txt" file made, consisting of a list of gages.

[] Better gage class
* info held looks good
* __str will return good info

[] On request provide specific data.
* create functions to return specific data as needed


### **Notes and Detritus**
Max:
https://www.kite.com/python/answers/how-to-find-the-max-value-of-a-pandas-dataframe-column-in-python

Tricky point:
* How do we wait for commands and timer at the same time: Selector or multithreading?
* USGS says "Gage" instead of "Guage" for historical reasons

USGS test
https://water.weather.gov/ahps2/hydrograph_to_xml.php?guage=CRNW1&output=tabular

SVPA gage test
https://readingsvc.azurewebsites.net/api/GetGuageReadingsUTC?regionId=1&id=SVPA-17&fromDateTime=2020-01-06T16:00:07-08:00&toDateTime=2020-01-10T16:00:07-08:00&showDeletedReadings=false

SQUW1
CRNW1
CRNZ1 


**DETRITUS**

DF USGS
* type: list

'''
    # svpa = data_get.get_SVPA_reading(USGS_test_url)
    # usgs = data_get.get_USGS_reading(SVPA_test_url)
    
    # print(usgs[1])
    # print(svpa['readings'][2])


USGS_test_url = "https://readingsvc.azurewebsites.net/api/GetGageReadingsUTC?regionId=1&id=SVPA-17&fromDateTime=2020-01-06T16:00:07-08:00&toDateTime=2020-01-10T16:00:07-08:00&showDeletedReadings=false"

    def get_USGS_reading(self, url):
        try:
            df_list = pd.read_html(url)
            return df_list
        except Exception as e:
            print("Error in GetUSGS, get_USGS:\n", e)
            return None

    def get_SVPA_reading(self, url):
        try:
            df_list = pd.read_json(url)
            return df_list
        except Exception as e:
            print("Error in GetSVPA, get_SVPA:\n", e)
            return None



GAGES = [
        ('SQUW1', 'USGS-38', '12144500','Snoqualmie River - Below the Falls', [15, 20, 31, 41], 'https://water.weather.gov/ahps2/hydrograph.php?wfo=sew&gage=squw1'),
        ('CRNW1', 'USGS-22', '12149000', 'Snoqualmie River near Carnation', [50.7, 54, 56, 58], 'https://water.weather.gov/ahps2/hydrograph.php?wfo=sew&gage=CRNW1'),
        ('CRNZ1', 'USGS-9', '12150400', 'Snoqualmie River at Duvall', None, 'https://water.weather.gov/ahps2/hydrograph.php?wfo=sew&gage=CRNZ1')
        ]

'''
