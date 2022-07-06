# TACI_CalciumImagingPlugin
A Plugin for analysis of confocal calcium imaging with sparse cells which are located in multiple z-positions.

---

### Table of Contents

- [Installation](#installation)
- [Description](#description)
- [Input and Output File Organization](#input-and-output-file-organization)
	- [Rename Organization Example A and B](#sorting-organization-example-a-and-b)
		- [Organization Example A](#organization-example-a)	
		- [Organization Example B](#organization-example-b) 	
	- [Organize Organization Example C and D](#fluorescence-extraction-organization-example-c-and-d)
		- [Organization Example C](#organization-example-c)
		- [Organization Example D](#organization-example-d)
	- [Extract Organization Example E](#merging-organization-example-e)
		- [Organization Example E](#organization-example-e)
	- [Merging Organization Example F](#merging-organization-example-f)
		- [Organization Example F](#organization-example-f)
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

***Rename*:**
- Creates a copy of the files and changes the file name to a format compatible with Organize in a new folder with _r added. 
- An option to perform Organize simultaneously is available.

***Organize*:**
- Makes a copy of the .tif files and sorts them into folders labeled by the z-positions. 
- An option to make the .tif gray is available.

***Extract*:**
- Combines MEAN_INTENSITY values Mean_Intensity#.csv files for each neuron in the folder. his requires specific file structure as an input. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section [Organization Example E](#organization-example-E).
- Subtracts the background and finds the maximal value for each timepoint. 
- Calculates the change in fluorescence (∆F/F<sub>0</sub>) and plots the ∆F/F<sub>0</sub> over time.
- Outputs the .csv files with the combined data and calculations, and a "Neuron Plots" folder containing the plots of the ∆F/F<sub>0</sub> as .png files into the "results" folder.
- An option to perform Merge is avaialable. 

***Merge*:**
- Combines all the ∆F/F<sub>0</sub> values for each neuron in the folder into one file. This requires specific file structure as an input. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section and [Organization Example F](#organization-example-F).
- Calculates the average and SEM of ∆F/F<sub>0</sub> values and plots the average ∆F/F<sub>0</sub> over time.
- Outputs "merged_data.csv" and "Average_dF_F0.png" into a "merged_data" folder within the "results" folder. 

## Input and Output File Organization
### Rename and Organize File Organization Example A-D

1) The file folder name must be identical to the beginning of each file name.
- Organize uses the Zeiss confocal file structure. Therefore, the files in the folder will have the same name as the folder with “_h#t#z#c#.tif”, (where the # stands for a number) at the end of each file to identify the phase (h#), time point (t#), z-position (z#), and channel (c#) of each file. In order for Organize to work, the original name of the folder must be kept, or if changed, the file folder name must be identical to the beginning of each file.
	- For example, in the practice folder, files are saved as “Neuron0to2_h#t#z#c#.tif”, so the folder name is “Neuron0to2”.
  
2) Rename must be executed if using other confocal microscopes that export .tif files:
	- The files must be exported as .tif. 
	- Since the naming systems are different for Nikon, Leica, Zeiss etc. the rename function is applied and the prefix, capitalization, digit count, and post text is defined within the name. Then each file is copied and renamed to be compatible with Organize. For example if a file is exported as **Neuron0to2Z#T#ni.tif**, the rename function would create a copy with the folowing name: **Neuron0to2t#z#.tif** 
	- Refer to [Organization Example A](#organization-example-a) for an example of how the files should be structured and [Organization Example B](#organization-example-b) for what the file structure looks like after execution.
	
3) The files from the original folder are copied and are saved into a new folder labeled with the file name and _gray_stacks one directory above (outside of the input folder) such that the original folder is never changed. For the example in Practice > CalciumImaging1 > Neuron0to2:
	- The folder Neuron0to2 should be chosen from the menu without being entered.
	- If there is a params.csv file, the parameters are automatically read. (Practice > CalciumImaging2 > Neuron3to5 example). 
	- If there are no params.csv file, the phase, t-position, z-position, channel, and whether images are already gray must be indicated.
		- If there is no phase or channel enter **Na**. There must be a numeric input for t-position and z-position. 
		- If the **Are Images grey?** box is unchecked parameter a grayscale function is performed on the copies.
	- The output folder "Neuron0to2_gray_stacks"the z-position folders named “Neuron0to2_1”, “Neuron0to2_2” etc. with the sorted gray files.
	- Refer to [Organization Example C](#organization-example-c) for an example of how the files should be structured and [Organization Example D](#organization-example-d) for what the file structure looks like after execution.

#### Organization Example A: 
##### Rename input example:

```bash
├── Analysis
├── CalciumImaging1
├── CalciumImaging2
└── renameNeuron0to2
    ├── renameNeuron0to2Z01T01ni.tif
    ├── renameNeuron0to2Z01T02ni.tif
    ├── renameNeuron0to2Z01T03ni.tif
    ├── renameNeuron0to2Z02T01ni.tif
    ├── renameNeuron0to2Z02T02ni.tif
    ├── renameNeuron0to2Z02T03ni.tif
    ├── renameNeuron0to2Z03T01ni.tif
    ├── renameNeuron0to2Z03T02ni.tif
    ├── renameNeuron0to2Z03T03ni.tif
    ├── renameNeuron0to2Z04T01ni.tif
    ├── renameNeuron0to2Z04T02ni.tif
    ├── renameNeuron0to2Z04T03ni.tif
    ├── renameNeuron0to2Z05T01ni.tif
    ├── renameNeuron0to2Z05T02ni.tif
    ├── renameNeuron0to2Z05T03ni.tif
    ├── renameNeuron0to2Z06T01ni.tif
    ├── renameNeuron0to2Z06T02ni.tif
    ├── renameNeuron0to2Z06T03ni.tif
    ├── renameNeuron0to2Z07T01ni.tif
    ├── renameNeuron0to2Z07T02ni.tif
    └── renameNeuron0to2Z07T03ni.tif

```
#### Organization Example B: 
##### Rename output example:

```bash
├── Analysis
├── CalciumImaging1
├── CalciumImaging2
├── renameNeuron0to2
└── renameNeuron0to2_r
    ├── renameNeuron0to2_r_t01z01.tif
    ├── renameNeuron0to2_r_t01z02.tif
    ├── renameNeuron0to2_r_t01z03.tif
    ├── renameNeuron0to2_r_t01z04.tif
    ├── renameNeuron0to2_r_t01z05.tif
    ├── renameNeuron0to2_r_t01z06.tif
    ├── renameNeuron0to2_r_t01z07.tif
    ├── renameNeuron0to2_r_t02z01.tif
    ├── renameNeuron0to2_r_t02z02.tif
    ├── renameNeuron0to2_r_t02z03.tif
    ├── renameNeuron0to2_r_t02z04.tif
    ├── renameNeuron0to2_r_t02z05.tif
    ├── renameNeuron0to2_r_t02z06.tif
    ├── renameNeuron0to2_r_t02z07.tif
    ├── renameNeuron0to2_r_t03z01.tif
    ├── renameNeuron0to2_r_t03z02.tif
    ├── renameNeuron0to2_r_t03z03.tif
    ├── renameNeuron0to2_r_t03z04.tif
    ├── renameNeuron0to2_r_t03z05.tif
    ├── renameNeuron0to2_r_t03z06.tif
    └── renameNeuron0to2_r_t03z07.tif

```

