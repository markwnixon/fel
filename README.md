# crontasks
These tasks are run on our company server ('fserver') at specified intervals:
Here is the crontask general schedule:
* * * * * uploader.sh >> /home/mark/flask/crontasks/uploader.log 2>&1
3 * * * * gate.sh >> /home/mark/flask/crontasks/gate.log 2>&1
9 3 * * * shipreport.sh >> /home/mark/flask/crontasks/shipreport.log 2>&1

As shown,
uploader is run every minute
gate is run once an hour (3 min past hour)
shipreport is run once a day at 3:09

uploader collects files placed in any of the following folders the collect source documents:
/incoming/bills
/incoming/bookings
/incoming/dispatch
/incoming/general
/incoming/interchange
/incoming/ojobs
/incoming/pods
/incoming/titles
/incoming/tjobs
The uploader code creates a text file of the original document whether it is pdf or a scan
If it is a scan it performs OCR on the document
It then uploads the source and the text documents up to the website where it can be submitted into the database
However, if the document is an interchange ticket, a booking, or a dispatch...it will parse the text file and
enter the data into the company database automatically...before uploading the source document and text files.

gate.sh calls gatemonitor.py
gatemonitor scrapes the Port of Baltimore website to obtain current data on gate tickets for FELA trucking
The code utilizes a headless Firefox browser with Selenium to log on using the company credentials, search for tickets
issued over the last 24 hours+ and uses ghostscript to convert the html image provided by the port into a pdf file for
internal documentation.  The data obtained off the site has high fidelity and can be extracted robustly so it is entered
directly into the company database for interchange tickets.

shipreport.sh call shipreport.py
shipreport scrapes various shipline website to obtain the projected dates of FELA company bookings.  These dates
fluxuate greatly from the estimated arrival times.  This data is continually updated in the datebase to keep customers aprised of their shipment arrivals.
