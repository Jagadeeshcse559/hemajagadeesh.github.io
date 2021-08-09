In this Document we are going to see Zero downtime database upgrade from
12c to 19c using Oracle Goldengate.

**Environment Details: -**

**Source:** ORCL, 12.2.0.1.0, OEL7, HR

**Target:** ORCL19, 19.3.0.0.0, OEL7, HR

High Level Steps:

-   Install goldengate Software both side

-   Setup extract and datapump on source site

-   Setup replict on target side

-   Export and import initial load using SCN

-   Start the replicat using on scn

**Installation of GoldenGate:**

Download the software using below link

<https://www.oracle.com/in/middleware/technologies/goldengate-downloads.html#license-lightbox>

copy the software to the preferred source and target location

![](media/image1.png){width="6.268055555555556in"
height="0.30833333333333335in"}

After unzip go to the location and execute ./runInstaller.

![](media/image2.png){width="6.268055555555556in"
height="1.7236111111111112in"}

The below page will be open after run Installer, select the database
version and click next.

If your source database is 12c select the “oracle Goldengate for Oracle
Database 12c”

![](media/image3.png){width="5.840277777777778in"
height="3.6597222222222223in"}

Select software location it would be your OGG\_HOME, Database home
location as database location, manager port, make sure run manager is
checked or not. if that check box is enabled subdirectories
automatically created and manager is running state.

Click on NEXT

![](media/image4.png){width="6.268055555555556in"
height="4.747916666666667in"}

Click on Install

![](media/image5.png){width="6.268055555555556in"
height="4.658333333333333in"}

Go to GG home location execute ./ggsci

![](media/image6.png){width="6.229166666666667in" height="1.6875in"}

Our goal is migrating HR schema from 12c database to 19c database using
GoldenGate. We have HR schema at source side, but we don’t have HR
schema at target side.

![](media/image7.png){width="6.268055555555556in"
height="1.5541666666666667in"}

![](media/image8.png){width="6.268055555555556in"
height="0.9631944444444445in"}

![](media/image9.png){width="6.268055555555556in"
height="1.3770833333333334in"}

![](media/image10.png){width="3.96875in" height="2.4375in"}

Target side:

![](media/image11.png){width="6.268055555555556in"
height="1.8833333333333333in"}

![](media/image12.png){width="6.268055555555556in"
height="1.7180555555555554in"}

Prerequisites at source side.

At container level:

![](media/image13.png){width="6.268055555555556in"
height="3.272222222222222in"}

![](media/image14.png){width="3.8125in" height="0.4444444444444444in"}

![](media/image15.png){width="3.6805555555555554in"
height="0.5277777777777778in"}

![](media/image16.png){width="4.444444444444445in"
height="0.3888888888888889in"}

archive log mode should enable.

Target side:

![](media/image17.png){width="5.944444444444445in"
height="3.2291666666666665in"}

Source side:

Enable trandata at schema level and configure extract and pump.

![](media/image18.png){width="6.268055555555556in" height="4.23125in"}

Adding extract process:

![](media/image19.png){width="6.268055555555556in"
height="1.5256944444444445in"}

Adding exttrail:

![](media/image20.png){width="6.268055555555556in" height="0.6875in"}

Adding Pump:

![](media/image21.png){width="6.268055555555556in"
height="1.0743055555555556in"}

![](media/image22.png){width="6.268055555555556in"
height="0.28888888888888886in"}

![](media/image23.png){width="5.055555555555555in"
height="1.2638888888888888in"}

![](media/image24.png){width="6.268055555555556in"
height="2.5756944444444443in"}

![](media/image25.png){width="6.268055555555556in" height="2.04375in"}

![](media/image26.png){width="6.268055555555556in" height="2.075in"}

Target side:

Adding replicat:

![](media/image27.png){width="6.268055555555556in"
height="0.9465277777777777in"}

![](media/image28.png){width="6.268055555555556in"
height="1.726388888888889in"}

![](media/image29.png){width="4.659722222222222in"
height="1.1729166666666666in"}

Start the initial dataload using Datapump on source side 12c
database![](media/image30.png){width="3.6944444444444446in"
height="0.9305555555555556in"}

![](media/image31.png){width="4.305555555555555in"
height="0.5694444444444444in"}

![](media/image32.png){width="6.268055555555556in"
height="3.0555555555555554in"}

Copy the datapump files to 12c server to 19c server

After exporting dump insert record at source table.

![](media/image33.png){width="6.268055555555556in"
height="2.2895833333333333in"}

![](media/image34.png){width="6.268055555555556in"
height="5.490972222222222in"}

Check the stats of extract after performing insert record.

![](media/image35.png){width="6.268055555555556in"
height="6.284722222222222in"}

Target side start importing

![](media/image36.png){width="4.930555555555555in"
height="2.763888888888889in"}

![](media/image37.png){width="6.268055555555556in"
height="0.8159722222222222in"}

![](media/image38.png){width="4.979166666666667in"
height="0.4930555555555556in"}

![](media/image39.png){width="6.268055555555556in"
height="4.082638888888889in"}

Initial load was completed using data pump.

Start the replicat process using CSN. Before start replicat check the
data count and table count of schema with source.

Source:

![](media/image40.png){width="6.268055555555556in"
height="1.8916666666666666in"}

Target:

![](media/image41.png){width="5.083333333333333in"
height="1.8333333333333333in"}

Tables count matches but data of jobs table not matches, it is because
the insert operation performed at source after taking export of schema.
Now if replicat started at target side the count of data of jobs table
would match.

Starting replicat at target side:

![](media/image42.png){width="6.268055555555556in"
height="0.8277777777777777in"}

![](media/image43.png){width="6.268055555555556in"
height="1.0618055555555554in"}

![](media/image44.png){width="6.268055555555556in" height="3.34375in"}

Now check the count of jobs table.

![](media/image45.png){width="6.268055555555556in"
height="0.8548611111111111in"}

Successfully data transferred from 12c to 19c.

For data integrity we have used scn here. But from GG version 12.2 we
have new feature that will make initial load using data pump utility in
simpler way.

You don’t want to note down the SCN and this will be taken care
automatically when executing the ADD SCHEMATRANDATA or ADD TRANDATA
command with the optional parameter PREPARECSN at the oracle GOLDENGATE
level.

ADD SCHEMATRANDATA SCHEMA PREPARECSN

ADD TRANDATA SCHMA.TABLE PREPARECSN

At Target side we need to add the below parameter to the replicat
process parameter file.

DBOPTIONS ENABLE\_INSTANTIATION\_FILTERING

For reference, please check below link

https://docs.oracle.com/goldengate/c1221/gg-winux/GWURF/set\_instantiation\_csn.htm\#GWURF1236
