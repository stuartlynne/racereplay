Race Replay Server                                              Stuart Lynne
                                                Thu Oct 11 23:53:34 PDT 2012 

0. Overview
***********

The Race Replay Server acts as a venue located server to collect and distribute
WT Timing unit data to an Internet Website.


1. WT Unit modifications
************************

The Race Replay Server requires a simple modification to the WT Timing system
to support collection of the data with a simple TCP connection. Typically this
involves installation of the tailcsv script to start on port 20002 pointing
at the race_data directory. 

When a connection is made on port 20002 inetd starts the tailcsv script, It
finds the active race file and does two things:

        1. sends the contents to STDOUT
        2. does a tail -f on the file to STDOUT

N.B. This does mean that duplicate data may be sent if the system is connected
to multiple times in a single session. But this also means that no data should
be lost. It is assumed that de-duplication is simpler than providing an alternate
mechanism to retrieve potentially lost data if the Race Server needs to restart.

This data is in the form of CSV records in the following format:


        "RX#","TAG_ID","DATE(YYYYMMDD)","TIME (HH.MM.SS)","time (ms)","BATT",
                "EVENT","box label","corr","time from start (HH.MM.SS)","ms","Box id: 0x3C5A7B"

        "00001","t025F5F","2012/08/31","03.54.23","618","1","0x005F","00","0x00","00.02.32","117","03.54.23","640"




1. Race Replay Management
*************************


The Race Replay Server is managed with a set of configuration files:

        /var/racereplay/organizers.csv  - series organizers
        /var/racereplay/venues.csv      - locations events are run
        /var/racereplay/names.csv       - generic list of riders
        /var/racereplay/users.csv       - map tagids to riders names for an orgnnizer
        /var/racereplay/events.csv      - specify event times for an organizer

A limited set of CGI scripts are available for interacting with these:

        /usr/lib/cgi-bin/venues.pl      - manage active venue and organizers
        /usr/lib/cgi-bin/recent.pl      - find recent tagids that are "unknown"

In general the system currently stores this information in simple CSV files (this
may change to mysql in the future.)

The Organizers file stores information about race orgainzers:

        "Organizer","Description","updatedby","timestamp"
        BVC,"Burnaby Velodrome Club",,
        EVSS,"Escape Velocity Cycling Club (Spring Series)",,
        EVWTNC,"Escape Velocity Cycling Club (WTNC)",,

The Venues file stores information about race venues. Generally this file
also indicates which venue is "active". Race data collected will be tagged
with the identifier of the "active" venue.

        "Venue","Organizer","Description","Distance","MinSpeed","MaxSpeed","gaptime","updatedby","timestamp","ActiveFlag"
        BVCTrack,BVC,"Burnaby Velodrome",.2,25,70,2,stuart.lynne@gmail.com,"2012-10-05 12:00:00",1
        CRCCrit,CRC,"Coastal Racing",1.345,10,60,5,stuart.lynne@gmail.com,"2012-10-05 12:00:00",0
        WTNC,EV,"Escape Velocity Cycling Club",1.58,20,55,5,stuart.lynne@gmail.com,"2012-10-05 12:00:00",0

The Names file stores generic information about users. This information does not typically change as
people race with different organizers. But is stored separately so that organizer specific
information (loaner tags, bib number, category information) can be store separately.

The Users file stores the Organizer specific information about users.

The Events file stores Organizer and Event specific information about actual
events (aka races.) The date and time started, category, laps, etc.




2. Data Collection - wtdb
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

The fields stored are:

        "timestamp","venue","boxid","eventid","rx","tagid","corr","finishms","startms","groupms","lapms"

The timestamp, venue, boxid, eventid, rx fields are used to disambiguate the record 
and allows for multiple timing units. The corr field is an indication of how many
times the chip had to transmit before the timing unit saw the transmission.



