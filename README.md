# Great Barrier Reef (GBR) Data Management System (DMS) training repository

This repository contains guidelines and example scripts for the GBR DMS training sessions. 
  
## Running example notebooks in this repository
You can either download or clone this repository to your local machine if you want to run the example notebooks included here. Below we include some instructions on how to set up you machine before you can successfully run the example notebooks.  
If you are interested in learning about how to work with other datasets available in the DMS, you can check our [rimrep-examples repository](https://github.com/aodn/rimrep-examples).  
  
## Setting up your machine

If you do not have `R` or `Python` installed in your computer, you can check the [Pre-event Instructions](https://github.com/aodn/rimrep-training/blob/main/Pre-Event%20Instructions.pdf) document for more details about how to do this. If you already have them available in your machine, simply follow the steps below.  
  
After making this repository available locally by either cloning or downloading it from GitHub, you need to ensure all packages used in this repository are installed in your local machine before running any notebooks. If any packages are not installed in your machine, you will not be able to run the example notebooks.
  
The good news is that you will not need to go through every notebook checking the libraries you need. We are providing some files that will automate this process for you whether you use `R`, `Python`, or both.  

**Note:** You will only need to complete the set up steps once per machine. This should be done prior to running the notebooks for the first time. Also note that if you plan to use notebooks in one language, either `R` or `Python`, there is no need to follow the set up steps for the programming language that you do NOT need.
  
<details>
<summary><b> Instructions for R users </b></summary>

If you are using the `R` notebooks, run the following two lines in the `RStudio` console:  
  
```R
  source("Installing_R_libraries.R")  
  checking_libraries()
```  
  
The lines above will run a function that automatically checks if any `R` libraries used in this repository are not installed in your machine. If any libraries are missing, it will install them automatically. Bear in mind that these notebooks were developed in `R` version 4.3.1, so you may need to upgrade your `R` version if you encounter any problems during package installation.  

</details>

<details>
<summary><b> Instructions for Python users </b></summary> 

We are also including a `requirements.txt` file, which contains all `Python` packages used in the `Python` notebooks included in this repository. You can use this file to create a [conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html) with all the required packages. To do so, run the following command in the Anaconda Prompt (Windows) or in your terminal (MacOS, Linux):  
  
```bash
conda env create -f requirements.txt -n rimrep
```
  
where `rimrep` is the name of the environment. You can use a different name for the environment if you prefer.    
  
**Note**: Before running the line of code above, make sure the terminal directory matches the directory where the `requirements.txt` file is located (e.g, `C:/user_name/Documents/rimrep-training`). Otherwise, the code above will not work. If you need to change the terminal directory, you can use the `cd` command as follows:

```bash
cd C:/user_name/Documents/rimrep-training
```
  
Alternatively, you can specify the full path to the `requirements.txt` file. For example, if your terminal window is in the `Documents` folder, you could simply type:  
  
```bash
conda env create -f rimrep-training/requirements.txt -n rimrep
```
    
Finally, you will need to activate this environment before you are able to run the `Python` notebooks included here. To do so, run the following command in your terminal window:  
  
```bash
conda activate rimrep
```
  
When you are done running the notebooks, you can deactivate the environment by running `conda deactivate` in the terminal window.
activate.  
</details>
  

