WT Active Utilities                                             Stuart Lynne
                                                Sun Oct 14 01:37:53 PDT 2012 


0. Overview
***********

The WT Active timing unit has a complicated access protocol that makes implementing
access to the timing data difficult.

The tailcsv script simplifies this by allowing access to the data as an network
server. It is run from inetd or xinetd.

There are several advantages to this approach:

    - multiple connections are allowed
    - all data is served on each connection reducing the possibility of lost data


1. WT Active 
************

The WT Active unit stores the active data in a CSV file in the directory:

    /home/wt/race_data

The unit maintains an event id that is incremented on each reboot and is used
to form the file name:

    RACE_00131_00.csv

The following fields are available in each CSV file:

    "RX#",
    "TAG_ID",
    "DATE(YYYYMMDD)",
    "TIME (HH.MM.SS)",
    "time (ms)",
    "BATT",
    "EVENT",
    "box label",
    "corr",
    "time from start (HH.MM.SS)",
    "ms",
    "Box id: 0x3C5A7B"

The RX# is an incrementing count associated with the timing data.

The TAG ID is from the chip that activated the unit.

The DATE field is the system local date the unit recorded the timing information.

The TIME field is the local time the unit recorded the timing information.

The time field is the MS of the time the unit recorded the timing information.

The BATT field is the battery status.

the EVENT field is the incrementing counter (in hex) that is used to create the
stored CSV file.

The box label allows for multiple timing units to be used.

The corr field is the number if the transmission (0-N-1) that was received by
the unit. The activated chip transmits its ID with an incrementing counter
multiple times. Non-zero values indicate that either the chip was masked, there
was a transmission collision or that their is problem with antenna positioning.

The time from start field is the elapsed time from the start. This is either from 
when the unit was turned on, or with an external start switch, when the unit was told
to reset the time to zero.

The ms field is the milliseconds portion of the elapsed time.

The Box id is a testing field (purpose unknown.)



2. inetd configuration
**********************

E.g.

    20002 stream tcp nowait root /usr/local/bin/tailcsv  tailcsv /home/wt/race_data



3. tailcsv
**********

The script finds the most recently modified file in the specified directory and
simply does:

    tail -f -c +0 $RACE

This sends all of the data and then does a tail -f (follow) on the file. Generally
this means that newly created timing events are made available within about one 
second to the client.


4. Installation
***************

Access to the WT Unit requires that the root password to the unit be set so that
ssh access to root@192.168.1.100 is available. This is simply done by removing the
top plate of the unit, plugging in a standard PS keyboard and VGA screen. Turn on
the unit and hit escape to get a boot prompt. Select the safe boot option to boot
to a single user command mode.

Once the root password is set ssh access is available and can be used to add files
and configure the unit. It is a (slightly dated) Linux distribution.



