### slide 1

+ hello and welcome, my name is Devin
+ thank you all for coming to our presentations today
+ I'm going to be primarily talking about my project, Field Catalog Monitoring with Nagios
+ but also briefly a little bit about who I am

### slide 2

+ okay, so here's a quick overview of my presentation
+ part 1 will briefly cover a little bit about me, who I am, where I come from, and so on
+ part 2, which will be the majority of the presentation will cover project related information
+ after that, I'll wrap things up by talking about future features and a retrospective
+ once that's over, there should be room for questions, if anyone has any

### slide 3 - "Part I"

+ so, lets get started
+ to reiterate, I'll be quickly setting some personal context and talking about my positioning at NCARe

### slide 4 - "Background"

+ as I mentioned, my name is Devin
+ and I'm entering my senior year at Metropolitain State University of Denver
+ where I'm majoring in CS / MTH
+ previous programming experiences
	- I primarily worked with the "classic languages," such as: Java / C / C++
+ why work @ NCAR?
	- cool scientific research
+ where do I work @ NCAR?
	- well I'm part of the SUPER program, which stands for summer undergraduate program for engineering research
	- part of EOL
	- more specifically under CDS-CTM divisions
	- mentored by Erik Johnson

### slide 5 - "Part II"

+ so that takes care of the personal introduction
+ now lets dive into part II, which focuses on my summer project

### slide 6 - "The Problem"

+ so everything in this project is based around field catalogs, which are directly integrated wtih field catalogs
	- now, field projects are the primary method in which research data is collected
	- the data varies in nature, but is very "digitally heavy"
	- data from field projects is typically stored on large servers, in databases, directories, etc
+ occasionally these products don't make it home
	- "not making it home" implies that there's some sort of error during the transportation and / or ingest process
	- the nature of these errors, or problems, can vary significantly
+ but we're not really concerned with what that errors is anyways
+ rather, what we care about is detecting **the existence of these error**
+ to reiterate, the problem we face is:
	- file ingest errors occurring somewhere between the field project and their respective resting place on an NCAR server

### slide 7 - "The Approach"

+ so we know the problem involves file ingest errors silently occuring without notice
+ so the solution to this problem is simple, we check for the age of incoming files and compare them to a pre-defined age-limit boundary
+ here's a quick walk-through of typical process in which our problem is handled:
	1. data is collected in the field,
	2. this data file is then ingested,
	3. typically, to some sort of persistent storage device.
	4. we then check this incoming data stream for recent updates (e.g. recently added files)
		- if the data stream has been recently updated, we don't care, and we just keep on checking the feed age.
		- if an error is detected, we notify the appropriate agents, and of course, keep on checking the incoming data stream.
+ this abstracted view of how the problem is addresses is relatively straight forward, and in theory solving the stated problem wasn't all that difficult
+ however, that doesn't mean we didn't run into some interesting problems along the way
+ potential complexities include:
	- dealing with huge file systems
	- working on SCALE devices
	- handling time appropriately
	- and allowing for recursive and dynamically parse-able directory structures with imbedded timestamps
+ so this slide gives you an idea of how this problem was addressed,
+ but also some of the potential challenges we would face when developing this application

### slide 8 - "The Setup"

+ before showing the solution to the problem
+ a quick list of technologies used 
+ predominately:
	- ruby (program backbone)
	- nagios (IT infrastructure monitoring framework)
	- and git / github (version control /  development interface)
+ additionally:
	- IRC, as an invaluable communication tool
	- some database related stuff
	- and yet another markup language, which was primarily used for configuration files

### slide 9 - "The Results"

+ so, onto the results

### slide 10 - "Script Commands"

+ this is a list of the backend commands available to the program we created
+ there are a decent number commands, some of which I'll go into more detail about in a bit
+ the commands in the top image, are used to set things like:
	- age boundaries
	- output detail
	- location of the data to be checked
	- location of a YAML configuration file
	- and so forth.
+ the bottom commands are intrinsically tied to the functionality of the program as well
	- they handle the laborious tasks of loading and unloading huge quantities of files into databases,
	- generating nagios compliant config files,
	- deploying these config files to the appropriate location,
	- and also include, a micronized version control system that moves deployed versions back and forth based on timestamped directories

### slide 11 - "Configurations"

+ here's a very simple example of what running this script would look like
+ the most important item is of course, the location of the data stream
+ in this example there are some additional configurations for modifying the age boundaries 
+ as can be seen in the "advanced usage", time is highly configurable and can be input as seconds, minutes, hours, and / or days
+ now only a few configurations are listed here, but configuration settings can get pretty complex when you start including all available commands
+ not to mention, a big part of this program's functionality is the ability operate off of configuration files
+ The "YAML Usage" feature does exactly that
	- it loads config files that have pre-defined settings and executes the script based on that

### slide 12 - "Output"

+ so here's an example of what you could expect to be a healthy response
	- a healthy response being a valid one
+ most importantly, the status of the file is listed
+ under the higher levels of output verbosity many more specifics are listed
+ this includes:
	+ location
	+ age
	+ age-overflow
	+ and other user defined configuration settings

### slide 13 - "Many Files"

+ quickly I'll talk about the "many files" feature integrated with this program
+ so it turns out that working on SCALE devices can be incredibly slow
+ as a result running this program can not only take a tremendous amount of time, but also cause a significant strain on the hardware
+ one of the features we ended up implementing was a file guessing algorithim,
+ which handles the problem of large file systems by, essentially, making an educated guess about the name and location of the file
+ progressively the filename is modified until a valid match is found
+ but, by implementing this hack, we introduced new problems
	- if unmanaged, this technique can actually cause more problems than we were originally attempting to negate
	- unchecked, this approach will end up making a far greater number of system calls when the file age becomes very large
	- so, our solution to this was to dynamically bound this process and have it scale in accordance to the anticipated file age

### slide 14 - "Nagios Web Interface"

+ I've shown off some of the back end functionality, now it's time to look at this script integrated with Nagios
+ this is an example of the Nagios client up and running with a couple dozen integrated services,
	- some of which are actual products from DC3
+ this file age check program directly interacts with this Nagios interface to provide an up to date, aggregated form, of information relating to incoming file streams

### slide 15 - "Email Notifications"

+ while on the topic of nagios,
+ here's a example of an email notification you could expect to get when a problem is detected
+ this is almost verbatim of what was seen from the command line example
+ email notifications are handled though nagios, and are extremely configurable in many regards

### slide 16 - "Future Features"

+ that wraps up features implemented thus far, here's a look at some of the features that might exist in the near future…
	- integrated and individualized contact information
	- SMS notifications, simplified versions of email notifications
	- a web interface for configuring product and service configuration files
	- and, dynamically handled product check-times, which is based on some fancy math for calculating sunrise and sunset on a day to day basis

### slide 17 - "Retrospective"

+ wing it son

### slide 18 - "Development Stats"

+ for any one interested in some pretty graphs, here they are
+ this one shows code frequency, or in more precise terms, the addition and subtraction of lines of code thought the project
+ the blue line indicates the cumulative number of lines of code in the project
+ and as it stands, the program has around 7 to 8 thousand lines of active code

### slide 19 - "Development Stats"

+ another pretty graph that shows the commit activity involved
+ an interesting comparison between the contributes of the project, both of us had about the same number of lines of code, but one of us did it in a lot less commits, and had a lot less subtractions

### slide 20 - "Thanks"

+ that wraps things up, thank you all for coming and letting me talk about my awesome summer project
+ before we go to quesitons I'd like to thank the following people
+ most importantly, my mentor, Erik Johnson. 
	- he was an outstanding resource and an incredible teacher - i've very lucky to have had him
	- thanks to both Nick Potts and Chris Burghart for arranging the site visits and hosting weekly SUPER intern meetings
	- and also Whitney, thank you for helping with all that fun paper work stuff
	- finally thanks to everyone else at NCAR - my experience here was absolutely phenomenal 
+ that's about it, does anyone have any questions?
