# TACI Calcium Imaging Plugin
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
		- [Params File](#params-file)
	- [Extract Organization Example E and F](#merging-organization-example-e-and-f)
		- [Organization Example E](#organization-example-e)
		- [Organization Example F](#organization-example-f)
		- [Background List File](#background-list-file)
	- [Merging Organization Example G](#merging-organization-example-G)
		- [Organization Example G](#organization-example-G)
		
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
- Outputs the .csv files with the combined data and calculations, and a "Neuron Plots" folder containing the plots of the ∆F/F<sub>0</sub> as .png files into the "results" folder. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section [Organization Example F](#organization-example-F).
- An option to perform Merge is avaialable. 

***Merge*:**
- Combines all the ∆F/F<sub>0</sub> values for each neuron in the folder into one file. This requires specific file structure as an input. Refer to the [Input and Output File Organization](#input-and-output-file-organization) section and [Organization Example F](#organization-example-G).
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
		- After running the Organize function, a params.csv file will be made, such that future runs have parameters filled in.
			-If parameters need to be changed, the created params.csv file must be deleted for changes to apply.  	
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
#### Organization Example C: 
##### Set-up for Organize:

```bash
├── Analysis
├── CalciumImaging1
│   └── Neuron0to2
│   	├── Neuron0to2_h01t01z01c2.tif
│   	├── Neuron0to2_h01t01z02c2.tif
│   	├── Neuron0to2_h01t01z03c2.tif
│   	├── Neuron0to2_h01t01z04c2.tif
│   	├── Neuron0to2_h01t01z05c2.tif
│   	├── Neuron0to2_h01t01z06c2.tif
│   	├── Neuron0to2_h01t01z07c2.tif
│   	├── Neuron0to2_h01t02z01c2.tif
│   	├── Neuron0to2_h01t02z02c2.tif
│   	├── Neuron0to2_h01t02z03c2.tif
│   	├── Neuron0to2_h01t02z04c2.tif
│   	├── Neuron0to2_h01t02z05c2.tif
│   	├── Neuron0to2_h01t02z06c2.tif
│   	├── Neuron0to2_h01t01z07c2.tif
│   	├── Neuron0to2_h01t03z01c2.tif
│   	├── Neuron0to2_h01t03z02c2.tif
│   	├── Neuron0to2_h01t03z03c2.tif
│   	├── Neuron0to2_h01t03z04c2.tif
│   	├── Neuron0to2_h01t03z05c2.tif
│   	├── Neuron0to2_h01t03z06c2.tif
│       └── Neuron0to2_h01t03z07c2.tif
├── CalciumImaging2
│   ├── Neuron3to5
│   └── params.csv
└── renameNeuron0to2

```

#### Organization Example D: 
##### After Organize is executed:

```bash
├── Analysis
├── CalciumImaging1
│   ├── params.csv
│   ├── Neuron0to2
│   └── Neuron0to2_gray_stacks
│       ├── Neuron0to2_1
│       │   ├── Neuron0to2_h01t01z01c2.tif
│       │   ├── Neuron0to2_h01t02z01c2.tif
│       │   └── Neuron0to2_h01t03z01c2.tif
│       ├──  Neuron0to2_2
│       │   ├── Neuron0to2_h01t01z02c2.tif
│       │   ├── Neuron0to2_h01t02z02c2.tif
│       │   └── Neuron0to2_h01t03z02c2.tif
│       ├── Neuron0to2_3
│       │   ├── Neuron0to2_h01t01z03c2.tif
│       │   ├── Neuron0to2_h01t02z03c2.tif
│       │   └── Neuron0to2_h01t03z03c2.tif
│       ├── Neuron0to2_4
│       │   ├── Neuron0to2_h01t01z04c2.tif
│       │   ├── Neuron0to2_h01t02z04c2.tif
│       │   └── Neuron0to2_h01t03z04c2.tif
│       ├── Neuron0to2_5
│       │   ├── Neuron0to2_h01t01z05c2.tif
│       │   ├── Neuron0to2_h01t02z05c2.tif
│       │   └── Neuron0to2_h01t03z05c2.tif
│       ├── Neuron0to2_6
│       │   ├── Neuron0to2_h01t01z06c2.tif
│       │   ├── Neuron0to2_h01t02z06c2.tif
│       │   └── Neuron0to2_h01t03z06c2.tif
│       └── Neuron0to2_7
│           ├── Neuron0to2_h01t01z07c2.tif
│           ├── Neuron0to2_h01t02z07c2.tif
│           └── Neuron0to2_h01t03z07c2.tif
├── CalciumImaging2
└── renameNeuron0to2

```
#### Params File: 
1) The params.csv file could be created in excel before running Organize. 
2) The .csv file should have 5 columns with the following headers: filename, phase, position_t, position_z, channel, is_gray.
	- Each column must contain the parameters that are wished to be filled in. For phase and channel, the prefix and number is included. If there is no phase or channel, *Na* is set. For is_gray, either *Yes* or *No* is set.
	- A params.csv file is created after running Organize with the inputs given. If the inputs need to be changed, the params.csv file should be erased or altered to match. 
	- The following is the params.csv file for Practice > CalciumImaging2 > Neuron3to5
		  
		|filename |phase |position_t |position_z |channel |is_gray |
		|---|---|---|---|---|---|
		|Neuron3to5 |h01 |3 |7 | c2 |No |


### Fluorescence Extraction Organization Example E and F
1) The “All Spots statistics” (old FIJI version) or "export" (new FIJI version) .csv files from TrackMate should be saved in the following format:
	- "Mean_Intensity#.csv", where # stands for the number of the z-position where values come from. 
		- For example, TrackMate “All Spots statistic” or "export" file for z-position 3 should be saved as “Mean_Intensity03.csv”. 
		- This way, the mean intensity values in each z-position can be tracked by replacing the general MEAN_INTENSITY column names with the file name “Mean_Intensity#” in the merged document.

2) The “Mean_Intensity#.csv” files for each neuron should be saved into a folder labeled “Neuron #”, where # stands for the number of the neuron. 
	- The first neuron in the set is always “Neuron 0”, the second is “Neuron 1” etc. If running only one neuron, label it "Neuron 0" [Organization Example E](#organization-example-e) and [Organization Example F](#organization-example-f) show an example of folder organization before and after execution respectively.

3) A “Background_list.csv” file must be present in the same folder as the “Neuron #” folders. 
	- The background_list has a column labeled “Neuron #” for each neuron and must have the background values for each z-positions. The number of z-positions for each neuron must equal to the number of “Mean_Intensity#.csv” files in each neuron folder. If they do not, this will lead to an error.
	- The columns must be in numerical order and must not skip values. Make sure there is no blank space in the header names before or after. For example " Neuron 0" and "Neuron 0 " will give an error. 

4) The Extract function will combine the "MEAN_INTENSITY" columns of the Mean_Intensity#.csv files in each “Neuron #” folder into one file, subtract the background, calculate the maximum intensity for each time point and the change in fluorescence (∆F/F<sub>0</sub>) for that neuron. These will be saved to a “results” folder with the name “Neuron #.csv”. Additionaly a plot for each neuron will be saved in a "Neuron Plots" folder within the "results" folder.
	- The “results” folder is created in the directory where the “Neuron #” folders reside. 
		- In [Organization Example E](#organization-example-e), the “results” folder would be created in the “Analysis” folder. 
		- Within the “results” folder, there are the .csv files with the merged mean intensity and the ∆F/F<sub>0</sub> calculation labeled after each neuron. 
		- There is also a folder called “Neuron Plots” that will have the output plots for each ∆F/F<sub>0</sub> value.
	- One folder, "Neuron 0" can be run if needed. 
5) An option to perform Merge is avaialable. 

#### Organization Example E: 
##### Set-up for Extract
``` bash
├── Analysis
│   ├── Background_list.csv
│   ├── Neuron 0
│   │   ├── Mean_Intensity_01.csv
│   │   ├── Mean_Intensity_02.csv
│   │   └── Mean_Intensity_03.csv
│   ├── Neuron 1
│   │   ├── Mean_Intensity_04.csv
│   │   ├── Mean_Intensity_05.csv
│   │   ├── Mean_Intensity_06.csv
│   │   └── Mean_Intensity_07.csv
│   └── Neuron 2
│   │   ├── Mean_Intensity_04.csv
│   │   ├── Mean_Intensity_05.csv
│   │   ├── Mean_Intensity_06.csv
│   │   └── Mean_Intensity_07.csv
│   ├── Neuron 3
│   │   ├── Mean_Intensity_01.csv
│   │   ├── Mean_Intensity_02.csv
│   │   └── Mean_Intensity_03.csv
│   ├── Neuron 4
│   │   ├── Mean_Intensity_04.csv
│   │   ├── Mean_Intensity_05.csv
│   │   ├── Mean_Intensity_06.csv
│   │   └── Mean_Intensity_07.csv
│   └── Neuron 5
│       ├── Mean_Intensity_04.csv
│       ├── Mean_Intensity_05.csv
│       ├── Mean_Intensity_06.csv
│       └── Mean_Intensity_07.csv
├── CalciumImaging1
├── CalciumImaging2
└── renameNeuron0to2
  
```
#### Organization Example F: 
##### After Extract is executed:
``` bash
├── Analysis
│   ├── Background_list.csv
│   ├── Neuron 0
│   ├── Neuron 1
│   ├── Neuron 2
│   ├── Neuron 3
│   ├── Neuron 4
│   ├── Neuron 5
│   └── results
│       ├── Neuron 0.csv
│       ├── Neuron 1.csv
│       ├── Neuron 2.csv
│       ├── Neuron 3.csv
│       ├── Neuron 4.csv
│       ├── Neuron 5.csv
│       └── Neuron Plots
│           ├── Neuron 0.png
│           ├── Neuron 1.png
│           ├── Neuron 2.png
│           ├── Neuron 3.png
│           ├── Neuron 4.png
│           └── Neuron 5.png
├── CalciumImaging1
├── CalciumImaging2
└── renameNeuron0to2
```  

#### Background List File
1) The background values are entered into an excel file that is saved as a "Background_list.csv" in the same directory as the Neuron # folders. 
2) The excel file should have a column for each neuron labeled as “Neuron #”. Make sure there is no space after the number in the neuron, as the following "Neuron # " will show an error.
	- Each column must contain the background values for the z-positions being analyzed. 
	- The number of background values must match the number of “Mean_Intensity#.csv” files in each neuron folder.
		- For example, in Practice > Analysis, “Neuron 0” has 3 “Mean_Intensity#.csv” files, “Neuron 1” with 4 “Mean_Intensity#.csv”, and “Neuron 2” with 4 “Mean_Intensity#.csv” files. 
		- The “Background_list.csv” would look like the following. 
		  
			|Neuron 0|Neuron 1|Neuron 2|Neuron 3|Neuron 4|Neuron 5|
			|--------------|---------------|---------------|--------------|---------------|---------------|
			|1.978       |  1.084       |  1.084       |1.978       |  1.084       |  1.084       |
			|2.435       |  1.150       |  1.150       |2.435       |  1.150       |  1.150       |
			|2.335       |  0.430       |  0.430       |2.335       |  0.430       |  0.430       |
			|            |  0.297       |  0.297       |            |  0.297       |  0.297       |
 ----------------

### Merging Organization Example G
1) The Merge function combines all the ∆F/F<sub>0</sub> columns from the “Neuron #.csv” files into one file and renames each column to match the “Neuron #.csv” file it came from. It then calculates the average and SEM of the ∆F/F<sub>0</sub> columns. The file is named “merged_data.csv” and is saved into a new “merged_data” folder within the "results" folder.
2) The code also creates and saves a graph named Average_dF_F0.png which plots the average ∆F/F<sub>0</sub> with the position T into the “merged_data” folder. 
3) All the “Neuron #.csv” files have to be in the same folder so the ∆F/F<sub>0</sub> of each neuron can be averaged and plotted.

#### Organization Example G: 
##### After merge_data() is executed:

``` bash
├── Analysis
│   ├── Background_list.csv
│   ├── Neuron 0
│   ├── Neuron 1
│   ├── Neuron 2
│   ├── Neuron 3
│   ├── Neuron 4
│   ├── Neuron 5
│   └── results
│       ├── merged_data
│       │   ├── Average_dF_F0.png
│       │   └── merged_data.csv
│       ├── Neuron 0.csv
│       ├── Neuron 1.csv
│       ├── Neuron 2.csv
│       ├── Neuron 3.csv
│       ├── Neuron 4.csv
│       ├── Neuron 5.csv
│       └── Neuron Plots
├── Neuron0to2
└── Neuron0to2_gray_stacks
```  
