Data Synchronization                                            Stuart Lynne
                                                Sun Oct 14 02:10:47 PDT 2012 


0. Overview
***********

The Race Replay server collects and pre-processes the timing data from the
WT Timing Unit.

It also makes a backup of the WT Timing units raw CSV files.

There are also a set of management files that determine how the race server
operates.

The management files, backup files and processed data files need to be synchronized
to the Internet web server.


1. Race File Backup
*******************

The wgetpull script is used to mirror the WT Systems race data files. It is typcially
run from cron and stores the files in:

        /var/racereplay/race_data

Typically this should be run daily via crontab:

        /etc/cron.daily/wgetpull


2. Data Synchronization
***********************


The ftppush script is used to push the management and lap data files to the Internet
repository. Typically it will be run from cron to push changed files from:

        /var/racereplay/race_data

To the Internet website in the same location.

Typically this will be run from crontab with a suitable time schedule:

    1 * * * * root /usr/local/bin/ftppush

Currently the ftppush script is implemented with lftp. It is configured using:

    /var/racereplay/.lftp

See the lftp-example.

