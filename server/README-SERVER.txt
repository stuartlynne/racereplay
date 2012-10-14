Race Replay Server                                              Stuart Lynne
                                                Thu Oct 11 23:53:34 PDT 2012 

0. Overview
***********

The Race Replay Server acts as a venue located server to collect and distribute
WT Timing unit data to an Internet Website.


1. Data Collection - wtdb
*************************


The wtdb script collects data from the WT Timing System. It determines what venue it is located at
from the venues file, and uses various heuristics to determine the following for each piece of
timing data:

    1. Finish time for the lap
    2. Start time for the lap the previously seen time is within parameters
    3. Gap within a group 
    4. Elapsed time for a lap


The files are stored in dated files (one per calendar date) in:

    /var/racereplay/lapd/lapd-$VENUE-$DATESTAMP.csv


2. Data Processing
******************

The wtdb script utilizes various venue related parameters from the venue file:

Specifically:

    "Distance",
    "MinSpeed",
    "MaxSpeed",
    "gaptime",


The fields stored are:

    "timestamp","venue","boxid","eventid","rx","tagid","corr","finishms","startms","groupms","lapms"

The timestamp, venue, boxid, eventid, rx fields are used to disambiguate the record 
and allows for multiple timing units. The corr field is an indication of how many
times the chip had to transmit before the timing unit saw the transmission.

The finishms field is available in all valid timing data records. It is the elapsed time in
milliseconds from the time the WT Unit started.

If the chip TagID was previously seen within the appropriate lap time (minimum and maximum as
determined by the distance, minspeed and maxspeed parameters) then the startms field will be
set.

Timing entries are grouped through the use of the gaptime parameter. All entries are within the
gaptime (between events) will have the groupms field set to the milliseconds from the first
entry in the group. 

The lapms field is the difference between the finishms and startms fields.



3. Duplicated Data
******************

To ensure that timing data is not lost the implementation of the tailcsv script on the 
WT Timing unit replays all current data every time the wtdb script connects to it.

To ensure that the wtdb script overhead is kept low it does not attempt to de-duplicate
the received data.

This means that the resulting processed data in the saved lapd files may contain duplicate
data. This is easily de-duplicated wit reference to the specific timing event data which
will be unique to each timing data record:

    "timestamp",
    "venue",
    "boxid",
    "eventid",
    "rx",
    "tagid",


