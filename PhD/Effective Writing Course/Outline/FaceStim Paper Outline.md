## General question
Can we find a neuro marker indicative for depression induced cognitive impediment
## Intro
- Why are we looking into depression
	- Large population (~20%) is at risk of suffering at least once during their life time under depression, out of those x% will have a case of sever depression.
		- What can be done about it
			- Prevention
			- Treatment methods: psychotherapy, medication, physical stimulation. About 30% are diagnosed as treatment resistant, i.e. therapy, medication and classical stimulation like with trans cranial electric or magnetic stimulation fail.
			- Novel methods of DBS
				- What are the problems of DBS
					- Side effects under constant stimulation
					- Task or day specific needs, especially for emotion based impairment
				- slMFB vs Subcallosal Cingulate (SCC - white matter)
				
- What is currently known about neuro- / biomarker
	- Overview (Neto?? paper 2021)
	- Different domains of processing in terms of neuro markers
		- Time domain:
			- N170 stronger for non depressed
			- Late positive response (after 300ms) for depressed
			- ... others ?
		- Frequency domain:
			- Alpha (higher and lower -> contradicting answers)
			- Gamma similar as alpha
			- Alpha Assymmetrie larger in depressed
		- Complexity
			- Entropy measures
			- Hyguchi dimension
			- Others
			- -> Overall, more complex structures in depressed it seams
- What can we do about this heterogeneity of answers?
	- We could setup experiments
		- What would the experiment need to cover
			- Be a task which is known to be better / easier performed by non depressed or at least split
			- Should be repeatable quickly to generate data for ML
			- Limit factors which deteriorate data quality, such as patient movement
		- What would we hypothesis
			- DBS ON leads to better performance under the task than DBS OFF
			- We can find a neural correlate which helps to identify the task performance or the stimulation state
		

## Methods
- What did we try
	- FaceStim with N patient - but paradigm evolved as we did not see behavioural difference as we hypotheses.
		- What did you do with how many:
			- Table overview of who did what and demographics
	- Describe different type of sessions
- How did we run the experiments
	- We showed faces and asked patients to react accordingly to has emotion / no emotion.-> later a control group task was included
	- What pictures did we show
		- Karolinska picture set, first n balanced pictures, handselecting a few which very different contrast or are off center
		- Pictures are harmonized for overall brightness to be the same. (Percieved vs actuall brightness)
			- [ ] What defines percieved brightness and what did we use in the end
	- What was the aim for the behavioural experiment
		- See behavioral difference between stim on and off
			- E.g. faster reaction or more accurate reaction
				- How did you determine significance
					- Looking at the distributions under various segmentations (i.e. ON/OFF total session, within emotion / gender task) then performing a Welch-t test with alpha=0.05
		- We did not see an effect, therefore tried various interations:
			- Happy Sad Neutral (Happy too easy)
			- Reduced picture show time (even one frame vs grey has an after burn effect)
			- Morphed states (harder but still no difference)
			- Rebalancing for gender (no change)
			- Addind self faces
			- Sad Neutral only
				- emotion
				- gender
			
- What where the data modalities
	- We recorded EEG (Brain Products @1kHz), LFP for the acute sessions (AO @22kHz)
		- EEG recordings where 128 ch gel based passive system, but with some left out in patched area if acute session. Reference vs Nose
		- Bio markers recorded for resp, heart rate and galvanic skin response via brain product ExG
			- Galva looking at the vendor recommended 25uS slope
	
		- LFP was referenced vs mastoid. Bipolar stimulation titrated with clinicians to below level of side effects (strong variation from patient to patient)
	- What did we do with the data
		- General preprocessing pipeline consisted of
			- identifying bad channels by inspection of PSDs and time series¡
			- Trained ICA removed components by visual inspection
			- Applied an additional peak-to-peak criterion of xx uV and xx uV flat epochs.
			- LFP - artifact removal (??) How to add this to the paper
	- How did you analyse the data
		- Data was processed in custom scripts written in python using mne for EEG and LFP data
			- What did you do in those scripts
				- The epochs where subject to an ERP analysis by baselining and bandpass filtering. Then a cluster significance test based of F-statistics with alpha=0.05 was used. 
					- Here the general caveats of this method shall be kept in mind. It does tell us if there is a significant difference but not exactly where in time
		- For the biosignals we used the python toolbox xxxx
			- It does ... for the galvanic skin response (two cycles)
			- It does ... for the heart rate
			- It does ... for the respiratory cycle

## Results
- What behavioural results did we find
	- We did not find any behavioural difference in on vs off stim scores neither in ON nor OFF stim
	- Self images, although very few, did also not show any significant difference
- What did we do with the neural and bio data
	- We looked at EEG and LFP data with the aim of decoding ON vs OFF state to get an understanding of how stimulation might change the processing of the cognitive task

		- What did we find
		
			- *Time domain* - We looked at the time domain where we found differences in ERP for some patient. Interestingly this was only the case if control task.
				- Differences where found in the frontal channel and at xx s from the picture presentation
				- The difference within a single session was reliable once identified and would lead to a decoding acc of ...
				- No significant difference found in the visual invoke N170 negativity or latency, also nothing found for the 
				- No differences where found for if split for sad and neutral faces. (MD redo the analysis to verify this indeed)
				
			- *Freq. domain* - Analysis in frequency domain was performed by analysing time frequency plots and calculating significant differences therein. And decoding via CSP + regularised LDA.
				- What did we find:
					- No significantly different frequency in TFR plots for EEG or LFP data
					- LFP spectra could be cleaned
					- High correlation in the LFP data tells us that we have high redundancy...
					- CSP + LDA generally not well in decoding
						- However if cross validation is done with a shuffled set (overfitting as non-stationarity is learned), we can get passable decoding scores
						- Resulting pattern might indicate that ....
			- *Galvanic Skin Response*
				- Galvanic skin response did show very interesting pattern for a well reacting patient. She did overall respond strongly to stimulation and amplitudes were on the high side for her.
				- Others did not show a conclusive result.
				- --> Box plots of the slope across on and off sessions and multiple slopes in on plot.
			- *Heart Rate and respiratory cycle*
				- Nothing to be seen there - box plots for the simple measures
				

## Discussion
### Impact
- What is the meaning of what you found:
	- *Time domain* - the differences we saw in the channels ... are in agreement with reporting found in
		- The finding itself is helping to define a region of interest for further investigations with longer observation times.
		- Not finding ... has the implication
	- *Frequency domain*
		- No single freq band could be identified which would show a modulation induced by DBS as it would be eg the case for PD patients
		- The strong difference in decoding performance for the shuffled cross val is an interesting indication for a strong impact of non-stationary processes. It would thus be very interesting to see a longer term recording.
	- *Bio signals*
		- The finding for GSR in one patient 

### Short Comings
- What are the main problems with our approach
	- First of course, the behavioural paradigm did not produce contrast for the given task
		- What could one do better different
			- The problem is that usually the participants have a rather high accuracy, although the score feedback is calibrated in a way that even blindly pressing just one direction as fast as possible leads to much higher scores (tested in self-experiment). Non of the participants with code found out about this strategy (probably have this at a different section)
			- As many similar tasks are used with depressed vs healthies, it is in general capable of providing information about the depression state, but for the DBS ON vs OFF case, the induced effect is most likely to slow changing as that it could be resolved within ~3min being activated.
			- A different paradigm would need to be created, which however is difficult as any induced change might be very minuscule an hence not traceable in behaviourally. If keeping with the emotion aspect, the reaction to the different 
	- Due to the pilot nature, we switched the paradigm quite often in search of a good behavioural result. This makes it impossible to compare across patients and whatever result we found needs to be considered as a N=1 finding. More similar measurements would be needed to analyse if the effects generalise.
	- Stimulation parameters where rather heterogeneous. In some cases, especially for the chronic recordings, it stays questionable if the absence of a recordable effect is due to insufficient stimulation.
	- Last, the recording time could have been insufficient to capture any slow change
		- What to do about it
			- Multiple recordings across a single day could be problematically effected by circadian rythm and administered medication
			- It would be necessary to record multiple days, which could prove difficult in terms of receiving an ethics approval
			- Use of modern sense and stim technology might however provide novel insight by investigatin long term recordings
## Conclusion
- What do you make of said results:
	- slMFB stim has an impact which can likely be controlled in a closed-loop setting. 
	- A core question is however the time scale of the anti depressant effect, which could not be investigated with the present study
	- Using the FaceStim task and potentially related similar tasks are not fit to investigate behavioural impact
		- A different protocol with longer time would need to be used to investigate this
	- Cognitive paradigms stay much more challenging as compared to motoric tasks, but the increasing interest will lead to many interesting insight and we hope the provided study does help generating ideas for further studies as well as show limitations of similar approaches.