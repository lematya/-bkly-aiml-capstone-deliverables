README FILE for CAPSTONE PROJECT
Prem Isaac 

Problem Statement: Build a Machine Learning Model to accomplish either 
a)	Prediction of Exoplanet Orbital Period from lightcurve already known to have Exoplanets 
b)	OR 
Binary Classification of LightCurves inferring the Presence or Absence of Exoplanets.
1. Overview of the Question
The discovery of exoplanets, which are planets outside our solar system is important to understanding the potential for extraterrestrial life. This ML project aims to identify potential exoplanet candidates from astronomical data. The goal is to analyze light curves from distant stars (changes in brightness over time) to detect periodic dips in brightness, which might indicate a planet passing in front of the star, which is known as a "Transit Event"
2. Understanding the Problem Domain 
A Star may or may not be a Host around which one or more Exoplanets orbit. The Kepler Space Telescope has over several years recorded Light from various stars at regular intervals (cadence). The brightness of light is recorded in terms of photons per second¸ henceforth referred to as Flux. This time series consisting of Observation Time and Flux, augmented by a serial cadence number and an integer Quality attribute (with a value of 0 indicating good quality) is referred to as a Lightcurve, carries information about both the star and about any exoplanets orbiting the star.
	Low frequency waxing and waning of Flux is due to Stellar variability, as some stars pulsation in brightness(like a strobe light), while others can have dark spots which periodically come into view due to Stellar Rotation i.e., when the star spins on its own axis. Additionally, high frequency variability is due to one or more exoplanets blocking the light when it crosses in front of the star as viewed through the line of sight of the Kepler telescope.This is known as a Transit Event, and is characterized by 3 parameters: 
a)	the Orbital Period of the Planet, 
b)	the Transit Duration (time taken to cross the front of the star as seen from the telescope), and 
c)	the Transit Depth which is the percentage drop in the flux value each time a transit occurs.
The presence of Exoplanets may be inferred by removing Trend and Seasonality from the lightcurve, and examining the Residuals for high frequency Transit-like signals, leading to the estimation of these 3 parameters.   


Type of Data Needed
The following types of data would be required:
•	Metadata about Stars: Stellar Characteristics such as brightness, temperature, size, mass, distance, color, etc
•	Light Curve Data: Publicly available Time-series data tracking the brightness of stars over time, gathered by telescopes which are orbiting the earth, like Kepler or TESS (Transiting Exoplanet Survey Satellite)
•	Labeled Examples: A dataset with labeled light curves indicating known exoplanet transits to use for supervised learning and validation.

3. Data Source 
Source: The NASA Exoplanet Archive (https://exoplanetarchive.ipac.caltech.edu/ ) 
Interface: 
a)	The website permits manual download of Metadata about Stellar Targets (all stars observed in the Kepler Mission). 
b)	Additionally, a Python API exists for programmatic download of both Metadata and Lightcurves of individual Stars

AI / ML Pipeline for Exoplanet Detection
General sequential outline of tasks
1.	Download Metadata about Stars which have confirmed Exoplanets (“Host” stars). 

2.	Perform Exploratory Data Analysis (EDA) on Orbital Period, Transit Duration, Transit Depth to understand the nature of the distribution of values, 

3.	Using parameters of Distributions best fitted to these 3 variables as Criteria, define a population of Host Stars to be used as Targets for building ML Models.

4.	For each of these Targets, download 4 consecutive quarters of Lightcurves 

5.	Perform Exploratory Data Analysis of 1 Exoplanet Lightcurve to understand and remove Seasonality and Trend. Estimate Orbital period by analyzing the residuals in the Frequency Domain, looking for high frequency oscillations.

6.	Split the Lightcurves data  from Step 4 into Training, Testing, and Validation sets, and build and evaluate ML Models to predict Orbital Period. 

PYTHON NOTEBOOKS Used
1.	EDA_ZERO.ipynb: Exploratory Data Analysis on Metadata about Exoplanets, focusing on parameters needed for detecting Transit Events. The distribution of these parameters is understood, and filters are applied to remove outliers from the list, to obtain a Population most likely to be modeled well by an ML model. 
Demonstrates that after outlier removal, there is insufficient input data for ML Model Training, necessitating the production of Simulated Exoplanet Light Curves. 

2.	download_raw_kepler_curves.ipynb: Data Retrieval Code to use an API to download Kepler Exoplanet lightcurves from the NASA Exoplanetary archive

3.	EDA_single_lightcurve.ipynb: Exploratory Data Analysis on a single lightcurve, Kepler-10 b, with the goal of determining what kind of Preprocessing will need to precede building ML Model.

4.	choose_exoplanet_lightcurves.ipynb:  Retrieval of Lightcurve (quarters 4 thru 7) for each Exoplent in the Exoplanet Population defined by EDA_ZERO, 

5.	chosen_kplr_curve_feat_extraction.ipynb: For the list of curves chosen in choose_exoplanet_light_curves.ipynb, use the TSFRESH Library to create more than 700 features


6.	create_simulated_exoplanet_lightcurves : Owing to scarcity of True Kepler Exoplanetary Lightcurve, Data, use the BATMAN package to simulate 10,000 lightcurves

7.	XGBoost_Model_for_orbit_prediction_using_TSFRESH_features.ipynb: XGBoost model to predict orbital period using Simulated Lightcurve data.

SAMPLE DATAFILES PROVIDED
Various Metadata Files have been provided as CSV. Some of these are downloaded from the NASA website, while others contain lists of files to aid the data retrieval process.



MAJOR CHALLENGES  and LESSONS LEARNED
1.	Overall Wrong problem choice for Capstone Project: My choice of orbital period prediction had too many preprocessing steps, and requires a knowledge level with Time Series Data 50% of which is mostly out of scope for the Berkeley AI/ML Course. Given my normal day job, it was prohibitively costly (in terms of time spent) . 

2.	Very Complex Data Retrieval Process: I spent far too much time (after my day job) trying to decipher and understand the NASA Exoplanet Archive, its file formats, the various ways to download data, learning the API, and working out technical glitches.

3.	Data Retrieval/Storage was very Time Consuming: This problem was twofold: 

a)	Owing to very large datafiles (each raw lightcurve file was between 700 MB and 950 MB) , data retrieval for 1400 such lightcurves took 15-20 hours at a time, because of slowness in the NASA Exoplanet Archive Serve 

b)	Interruptions due to Google Drive Quota violations (even though I had purchased 2 TB of space). Each time this happened, I lost 7 – 15 hours to process anywhere near 1000 lightcurves.

4.	Scarcity of Actual Input Data: I came to discover, a little too late for my advantage, that there is simply not enough high quality Lightcurve data with known Exoplanets to train an ML Model sufficiently, and thus, after consulting literature, I had to rely upon Simulated Light Curves, and this somehow led to OVERFITTING in the Model, with poor performance

As a result, I was unable to spend quality time on the modeling process. However, I hope to have demonstrated sufficient proficiency in Pandas, Visualization, and the General Model-building process (EDA, Preprocessing, Feature Extraction, Modeling) to be able to obtain the certificate!


