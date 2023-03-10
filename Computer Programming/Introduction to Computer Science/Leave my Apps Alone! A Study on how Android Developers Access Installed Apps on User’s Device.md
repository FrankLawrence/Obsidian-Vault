(links:: [[Introduction to Computer Science]])
# Introduction
- APIs retrive various information related to the applications that are currently installed on the device
- Users have no knowledge of these methods
- IAMs: Installed Application Methods
- **How are IAMs used by Android developers?**
- 14,342 free Android apps on google play store and 7,886 open-source Android applications
- *What are the usage of IAMs* and *how aware are developers of the presence of IAMs in their app*
# Background
- `packagemanager class` contains methods to retrieve information on installed packages
- User traits can be concluded from the installed apps
	- gender predictablity: 70-80% -> 87%
	- religion, relationship status, spoken languages, countries of interest, age
	- life events: marriage, becoming a parent
	- IAMs exist for iPhone as well
		- applications of interest declared before-hand
-> User traits can be exploited to show more relevant ads 
# Study Design
- python scripts to scrape the web for open-source apps and to extract/analyze data 
## Research Question
*RQ1* – How are usages of IAMs distributed across app categories? 
*RQ2* – What kinds of information are most frequently accessed through IAMs?
*RQ3* – How are usages of IAMs distributed between app code and included libraries code?
*RQ4* – What is the declared main role that libraries calling the IAMs play?
*RQ5* – To what extent are developers aware of IAMs and tend to reflect that awareness?
## Data collection
- 7,886 public Github repositories (commit history and meta data from google play)
- 500 most popular free apps from 35 categories
  -> Binary files of 14,342 apps (without duplicates)
- developer questionnaire 
	- Where does your app use the methods
	- Why does it use those methods
	- comments
- each IAM call was recorded to find the field that was retrieved
- is the IAM call originating from a library or the code?
## Analysis
- RQ1-3: compute the count of IAM in each category, accessed fields and usages
- RQ4: Web search for official website of each library (or repository)
  manually(2 researchers) survey the websites for their purpose
- libraries included in min 5 apps -> 154 individual libraries accounting for 82.8%
## Results
### RQ1
- Average usage rate higher in commercial apps in comparison to open source apps (30% and 3% respectively )
- 4214 commercial and 228 open source
### RQ2
packageName is the information most frequently collected through IAMs, accessed by 47.62% and 46.90% of all IAMs calls performed in open-source and commercial apps, respectively.
### RQ3
- In commercial apps, the vast majority of IAMs calls are performed in included libraries, representing 83% of all usages in our dataset.
- *Comics*, *vehicles* and *art/design* have > 97% library IAM calls
- 62% in Open-source
### RQ4
- More than one third of libraries that employ IAMs are Advertising libraries and roughly one other third are Utility libraries. The remainder is mostly comprised of Custom libraries, Analytics and App promotion libraries.
- 10 advertising libraries account for 33% of IAM calls in libraries
### RQ5
- 7 of 47 developers who answered the questionnaire were oblivious to IAMs in their program
- Some developers are against the use of IAMs for advertising purposes - some are against the deprication or removal of IAMs
## Discussion
- libraries have a black-box nature
- advertising libraries use IAMs to attain user traits in order to produce more relevant adds
- Users should be able to activate IAM calls when they want to
- Users should look for open source apps
- Users can use app vetting services which can show privacy risks for users when installing an app