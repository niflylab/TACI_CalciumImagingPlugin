# TACI_CalciumImagingPlugin
A Plugin for analysis of confocal calcium imaging with sparse cells which are located in multiple z-positions.

---

### Table of Contents

- [Installation](#installation)
- [Description](#description)
- [Input and Output File Organization](#input-and-output-file-organization)
	- [Sorting Organization Example A and B](#sorting-organization-example-a-and-b)
		- [Organization Example A](#organization-example-a)	
		- [Organization Example B](#organization-example-b) 	
	- [Fluorescence Extraction Organization Example C and D](#fluorescence-extraction-organization-example-c-and-d)
		- [Organization Example C](#organization-example-c)
		- [Organization Example D](#organization-example-d)
	- [Merging Organization Example E](#merging-organization-example-e)
		- [Organization Example E](#organization-example-e)
	- [Background List File](#background-list-file)
- [Code Documentation](#code-documentation)
	- [Sorting](#sorting)
	- [Fluorescence Extraction](#fluorescence-extraction)
	- [Merging](#merging)

---
## Installation
Download the .jar file. Install the plugin in FIJI (Plugins > Install) and restart FIJI. 

## Description
The Plugin has the following 4 functions:
***Rename***
- Creates a copy of the files and changes the file name to a format compatible with Organize in a new folder with _r added. 
- An option to perform Organize simultaneously is available.

***Organize***
- Makes a copy of the .tif files and sorts them into folders labeled by the z-positions. 
- An option to make the .tif gray is available.

***Extract***
- Combines MEAN_INTENSITY values Mean_Intensity#.csv files for each neuron in the folder. his requires specific file structure as an input. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section [Organization Example E](#organization-example-E).
- Subtracts the background and finds the maximal value for each timepoint. 
- Calculates the change in fluorescence (∆F/F<sub>0</sub>) and plots the ∆F/F<sub>0</sub> over time.
- Outputs the .csv files with the combined data and calculations, and a "Neuron Plots" folder containing the plots of the ∆F/F<sub>0</sub> as .png files into the "results" folder.
- An option to perform Merge is avaialable. 

***Merge***
- Combines all the ∆F/F<sub>0</sub> values for each neuron in the folder into one file. This requires specific file structure as an input. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section and [Organization Example F](#organization-example-F).
- Calculates the average and SEM of ∆F/F<sub>0</sub> values and plots the average ∆F/F<sub>0</sub> over time.
- Outputs "merged_data.csv" and "Average_dF_F0.png" into a "merged_data" folder within the "results" folder. 

