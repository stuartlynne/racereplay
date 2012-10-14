Race Replay Server Management                                   Stuart Lynne
                                                Sun Oct 14 02:30:21 PDT 2012 


0. Overview
***********

Currently the Race Replay server is managed via a small set of CSV files. 

It is contemplated that these will be replaced by a mysql database.

Currently:


    - CBC.csv
    - organizers.csv
    - venues.csv
    - loaners.csv
    - events.csv


1. CBC
******

List of users from Cycling BC. Typically these people already have a chip.

    "tagid","firstname","lastname",Gender,Team,YOB,"UCICat","AbilityCat,"UCICode"
    t026201,Jeremy,Crane,Male,,,,,
    t025CF3,Sean,MacTavish,Male,,1981,Citizen,,
    t025D0A,Trevor,Pearson,Male,CRC ,1995,Junior,,CAN19950821
    t025D0C,Terry,Sagert,Male,,1959,Citizen,,

2. Organizers
*************

A list of the organizers who put on events.

    "Organizer","Description","updatedby","timestamp"
    BVC,"Burnaby Velodrome Club",,
    EVSS,"Escape Velocity Cycling Club (Spring Series)",,
    EVWTNC,"Escape Velocity Cycling Club (WTNC)",,

3. Venues
*********

A list of the venues where events take place.

    "Venue","Organizer","Description","Distance","MinSpeed","MaxSpeed","gaptime","updatedby","timestamp","ActiveFlag"
    BVCTrack,BVC,"Burnaby Velodrome",.2,25,70,2,stuart.lynne@gmail.com,"2012-10-05 12:00:00",1
    CRCCrit,CRC,"Coastal Racing",1.345,10,60,5,stuart.lynne@gmail.com,"2012-10-05 12:00:00",0
    WTNC,EV,"Escape Velocity Cycling Club",1.58,20,55,5,stuart.lynne@gmail.com,"2012-10-05 12:00:00",0

4. Loaners
**********

A list of the TagID's for the chips that each organizer has to loan out for
events. Typically these are short term single day loans.

    "tagid","shortid"
    t025D8E,"Loaner 1"
    t0261D9,"Loaner 2"

5. Events
*********

A list of events that organizers put on at a venue. This sets the time, length
etc for and event and provides the name and description.


    "timestamp","venue","description","category","eventtype","laps","sprints"
    "2012-10-12 19.08.43","BVCTrack","Elimination to scratch","AGroup","Elimination",3,0


