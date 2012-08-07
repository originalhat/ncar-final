### slide 1

+ greeting
+ thank for coming
+ introduce project title

### slide 2

+ give overview of presentation
+ room for questions @ the end of presentation

### slide 3 - "Part I"

+ lets get started
+ set personal context in relation to NCAR

### slide 4 - "Background"

+ entering senior year
+ MSUD student
+ majoring in CS / MTH
+ previous experiences with code
	- Java / C / C++
	- experience with NCAR related tech was trivial
+ why work @ NCAR?
	- cool scientific research
+ where do I work @ NCAR?
	- summer undergraduate program for engineering research
	- part of EOL
	- more specifically under CDS-CTM divisions
	- mentored by Erik Johnson

### slide 5 - "Part II"

+ personal introduction over
+ dive into part II, the project I worked on

### slide 6 - "The Problem"

+ background information
	- field projects are the primary method in which research data is collected
	- the data varies in nature, but is very "digitally heavy"
	- data from field projects is stored on large servers, in databases, in directories, etc
+ occasionally these products don't make it home
	- an error occurs in the ingest process
+ not concerned with what that errors is
+ rather, we care about **the existence of this ingest error**
+ in summary, the problem we face is:
	- file ingest errors occurring somewhere between the field project and their respective resting place on an NCAR server

### slide 7 - "The Setup"

+ before showing the solution to the problem
+ a quick list of technologies used 
+ predominately:
	- ruby
	- nagios
	- and git / github
+ additionally:
	- IRC, as an invaluable communication tool
	- some database related stuff
	- and yet another markup language, primarily used for configuration files

### slide 8 - "The Approach"

+ we know the problem involves file ingest errors
+ the solution is simple, check for the existence of files
	- we need to ensure that the age of files deposited into their respective locations is within a specified boundary
+ a quick walk-through of typical process in which our problem is handled:
	1. data collected in the field 
	2. the file is ingested
	3. typically to some sort of persistent storage
	4. we then check this incoming data stream for recent updates
		- if the data stream has been recently updated, we don't care, we just keep on checking.
		- if an error is detected, we notify the appropriate agents, and keep on checking.
+ the abstracted view of how the problem is addresses is relatively straight forward
+ potential complexities arise:
	- dealing with huge file systems
	- working on SCALE devices
	- handling time appropriately
	- allowing for recursively parse-able directory structures with imbedded timestamps
+ this gives you an idea of how this problem was addressed
+ but also some of the challenges we faced when developing this application

### slide 9 - "The Results"

+ so, onto the results
+ no live demo
+ as I've opted to take the easy way out and simply show functionality with screenshots

### slide 10 - "Script Commands"

+ this is a list of the backend commands available to the program we created
+ a decent number commands, some of which I'll go into more detail about
+ examples of what the commands on the top are used for are, setting:
	- lower and upper age bounds
	- output detail
	- location of configuration file
	- etc.
+ the bottom commands are intrinsic to the functionality of the program
	- they handle the laborious task of loading and unloading huge quantities of files into databases
	- generating nagios compliant config files
	- deploying these config files to the appropriate location
	- also, a micronized version control system that moves deployments back and forth

### slide 11 - "Configurations"

+ here's a very simple example of what running this script would look like
+ the most important item is of course, the location
+ in this example there are some additional configurations for modifying the upper and lower age boundary
+ as can be seen in the "advanced usage", time is highly configurable and can be input as seconds, minutes, hours, and days
+ now only a few configurations are listed here, but configuration settings can get pretty complex
+ a big part of this program is the ability operate off of config files
+ The "YAML Usage" feature does exactly that
	- it loads config files that have pre-defined settings and executes the script based on that

### slide 12 - "Output"

+ so here's an example of what you could expect to be a healthy response
+ the status of the file is listed
+ under the higher levels of output verbosity many more specifics are listed
+ this includes:
	+ location
	+ age
	+ age-overflow
	+ and other user defined configuration settings

### slide 13 - "Many Files"

+ quickly I'll talk about the "many files" feature integrated with this program
+ it turns out that working on SCALE devices is incredibly slow
+ as a result running this program can not only take a tremendous amount of time, but also cause a significant strain on the hardware
+ one of the features we ended up implementing was a file guessing technique
+ this handles the problem of large file systems by, essentially, making an educated guess about the location of the file
+ progressively the filename is modified until a valid match is found
+ but, by simply implementing this hack, we introduced new problems
	- if unmanaged, this technique can actually cause more problems than we were originally attempting to negate
	- our solution to this was to dynamically bound this process and have it scale in accordance to the anticipated file age

### slide 14 - "Nagios Web Interface"

+ I've shown off some of the back end functionality, now it's time to look at it integrated with Nagios
+ the program directly interacts with this interface to provide an up to date, aggregated form, of information relating to incoming file streams
+ Nagios is highly configurable and is a fantastic platform for anyone interested in IT Infrastructure Monitoring of any sort

### slide 15 - "Email Notifications"

+ quickly, and example of an email notification you could expect to get when a problem is detected
+ this is almost verbatim of what was seen from the command line example
+ also highly configurable

### slide 16 - "Future Features"

+ what could be integrated with this product in the near future?
	- integrated and individualized contact information
	- a web interface for configuring product and service files
	- and, dynamically handling visually checked data that changes based on the sunrise and sunset time variable
	- not listed but significant: SMS notifications

### slide 17 - "Retrospective"

+ TODO

### slide 18 - "Development Stats"

+ for any one interested in some pretty graphs, here they are
+ this one shows code frequency, or in more precise terms, the addition and subtraction of lines of code thought the project
+ the project as it stands has around 5,000 lines of active code

### slide 19 - "Development Stats"

+ another pretty graph that shows the commit activity involved
+ it's clear that I'm a novice based on the code addition / subtraction ration
+ but more importantly is the improvement that's seen as time progresses
	- not only in the decreased "net loss" of code
	- but also the frequency in which items were committed
+ both graphs are from GitHub, which is was our development interface to the version control system "Git"
+ it was a tremendous asset when developing this program, I'd highly recommend it to anyone developing anything

### slide 20 - "Thanks"

+ that wraps things up, thank you all for coming and letting me talk about my summer
+ also I'd like to thank the following people
+ most importantly, my mentor, Erik Johnson. 
	- he was an outstanding resource and an incredible teacher
	- thanks to both Nick Potts and Chris Burghart for arranging the site visits and hosting weekly SUPER meetings
	- and also Whitney for helping with all that fun paper work stuff and setting up a place for me to stay over the summer
	- also thanks to everyone else at NCAR - my experience here was absolutely phenomenal 
+ that's about it, does anyone have any questions?
