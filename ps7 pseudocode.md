The IV in this study is which of 4 stimuli (“profiles” of a male target individual varying by masculine- vs. 
feminine-stereotyped interests and male vs. female romantic partner) participants viewed prior to completing 
a mouse-tracking task; due to limitations of MouseTracker’s task design functions, this corresponds to which 
of 4 versions of the task they were randomly assigned to complete. 
The DVs in this study are participants’ accuracy, response times (RTs), and the maximum deviation (MD) and 
area-under-the-curve (AUC) of their mouse cursor trajectories in trials where they report the target 
individual’s sexual orientation; the latter two measures are computed by the MT Analyzer software, and 
provide indices of participants’ implicit attraction to incorrect response options. 

Pseudocode:

#Part 1: raw data consolidation
>copy task folders from all lab computers used in the study #MT Runner automatically creates a data folder 
for a given task in the same folder where the CSV encoding the task is stored
>merge raw data folders for corresponding versions of the task #i.e., copy all raw data from ES2A into a 
single folder, then do the same for ES2B, ES2C, and ES2D

#Part 2: data processing with MT Analyzer
>dataFolders=[ES2A, ES2B, ES2C, ES2D]
>for folder in dataFolders:
	#in MT Analyzer:
	>load folder
	>import data
	#import settings:
	>condition_1=experimental #using this setting while importing restricts the data output to trials 
	labelled as experimental in the task code; this will exclude distractor and instruction trials
	>Correct/incorrect analysis=True #data output will now include an “error” column which codes for 
	whether the participant’s response on a given trial matches the correct answer indicated in the task 
	code
	>visualize #necessary step prior to computing; produces visual array of participants' cursor trajectories
	>compute #necessary step prior to exporting; computes relevant variables based on cursor trajectories
	>export to csv #relevant data which the csv will include: subject number, trial number, response, 
	presence/absence of error, RT, MD towards unselected response, and AUC between actual trajectory and 
	idealized trajectory (i.e., a straight line from the "start" button to the chosen response button)
	#in the csv output file:
	>add column: task condition
	>task condition=folder #csv now also indicates which version of the task each participant completed
	>add column: stereotype condition
	>stereotype condition= -1 OR 1 #based on the stimulus they viewed: -1 for masculine-stereotyped target, 
	1 for feminine stereotyped target
	>add column: SO condition
	>SO condition= -1 OR 1 #based on the stimulus they viewed: -1 for heterosexual target, 1 for homosexual 
	target
>merge the 4 csv files into a single csv: merged_data.csv

#Part 3: data analyses in R Studio
>read merged_data.csv into R as data
>DVs.1=[error, RT]
>for dv in DVs:
	>run 2 x 2 ANOVA on data
		>main effect 1: stereotype condition
		>main effect 2: SO condition #the interaction between these conditions corresponds to whether 
		the target conforms to or violates gender inversion stereotypes about sexual orientation
>create subset data.correcttrials for trials where error = 0 #subset will contain only correct trials
>DVs.2=[MD, AUC]
>for dv in DVs.2:
	>run 2 x 2 ANOVA on data.correctrials #examines attraction to incorrect response options on trials 
	where participants answered correctly
		>main effect 1: stereotype condition
		>main effect 2: SO condition 
#for all analyses we anticipate a significant interaction effect where error/RT/MD/AUC is greater for targets who 
violate stereotypes, but no significant main effects. 