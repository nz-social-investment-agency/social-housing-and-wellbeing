# Social Housing and Wellbeing

Using linked administrative and survey data to evaluate the wellbeing impacts of receiving social housing.

## Overview
This analysis presents the first attempt at applying the Social Investment Agency's (previously the Social Wellbeing Agency) wellbeing measurement approach to a policy issue.

The analysis uses the New Zealand General Social Survey data in the IDI, combined with administrative data to look at how placement in social housing impacts the wellbeing of people.

The method adopted in this paper (and code) aims to move beyond a simple descriptive approach, to instead identify the difference in wellbeing outcomes for people before and after being placed in social housing. While this is still not as good an estimate of the true causal impact of social housing as a genuine experimental evaluation, by providing a dynamic picture of the change in wellbeing outcomes associated with a social housing transition, it significantly enriches the available evidence base. The project aims to provide four important pieces of information to assess the impact of social housing interventions. These are:

* What impact does being placed in social housing have on housing outcomes (i.e. the quality of accommodation for social housing recipients – household crowding, temperature of residence, dampness, and the physical state of the house)?
* What impact does being placed in social housing have on other outcome domains important to the recipient’s wellbeing (e.g. health, social contact, jobs)?
* What is the impact of placement in social housing on the recipient’s overall wellbeing?
* How should we value the gain in recipient’s wellbeing for the purposes of cost-benefit analysis?

Beyond this, there are two additional objectives relating to the methodology used. These are:

* Can linked administrative and survey data be used in the IDI to identify the wellbeing outcomes of people before and after a social policy intervention?
* What are the key lessons for using the IDI to assess the impact of policy in wellbeing terms?

For the full report on the analysis, refer to https://sia.govt.nz/our-work/measuring-the-impacts-of-social-housing/.

## Dependencies
* It is necessary to have an IDI project if you wish to replicate this work. Visit the Stats NZ website for more information about this.
* You will find submodules in the `lib` folder of this repository. These submodules are additional repositories that may be required to replicate this project. These repositories are:
	* social_investment_analytical_layer (SIAL)
	* social_investment_data_foundation (SIDF)
	* SIAtoolbox

Instructions for the installation of each repository can be found in their respective readme files.

## Folder descriptions
This folder contains all the code necessary to build characteristics and service metrics. Code is also given to create an outcome variable (highest qualification) for use and as an example of how the more complex variables are created and added to the main dataset.

**include:** This folder contains generic formatting scripts.
**lib:** This folder is used to refer to reusable code that belongs to other repositories
**sasprogs:** This folder contains SAS programs. The main script that builds the dataset is located in here as well as the control file that needs to be populated with parameters for your analysis. 
**rprogs:** This folder contains all the necessary R scripts that are required to perform the analysis on the dataset created by the SAS code.
**output:** This folder contains all the outputs generated from the analysis. 

## Instructions to run the social-housing-and-wellbeing project
### Step A: Create analysis population and variables
1. Start a new SAS session
2. Open `sasprogs/si_control.sas`. Go to the yellow datalines and update any of the parameters that need changing. The one that is most likely to change if you are outside the Agency is the `si_proj_schema`. In case the IDI version that you're pointing to needs to be updated, edit the corresponding variables as well- the variables are `idi_version` and `si_idi_dsnname`. Note that the results in this paper are based on IDI_Clean_20171020. If you have made changes save and close the file.
3. Open `sasprogs/si_main.sas` and change the `si_source_path` variable to your project folder directory. Once this is done, run the `si_main.sas` script, which will build the datasets that are needed to do the analysis.

### Step B: Data Preparation & Analysis
There are 3 distinct streams of analysis for this project-

**Survey-Weighted Descriptive Statistics**
1. Start a new R session.

2. Open up `rprogs/1_of_weighted_gss_analysis_wrapper.R`. Edit the working directory by modifying with path specified at the first line of this file. In addition, also edit the variable `schemaname` to the appropriate schema that you are using. This is a wrapper script that runs all steps involved for generating the weighted descriptive statistics for the analysis. The script performs a linking of the GSS survey data with the IDI Spine, and reweights the survey to account for records that are unlinked with the IDI Spine. A comparison test between the distribution of the GSS variables is also performed pre and post-reweighting. This is to ensure that the IDI Spine linkage and the subsequent and re-weighting procedure does not bias the variables that is to be compared. The outputs of this operation can be obtained from the `output` folder. 


**Unweighted Before-After Analysis**
1. Start a new R session.
2. Open up `rprogs/1_run_analysis_treat_control.R`. This script creates loads up all required libraries and generates all the Before-After analysis results. This analysis does not take into account the survey weights, and compares the group that was housed 12 months before GSS interview to the group housed 15 months after. Bootstrap sampling is used to get confidence intervals around the estimates here. In addition to the main analysis, this code also performs a validation, by comparing the group that was housed 12 months before GSS interview to the group housed 12 months after, and another validation using propensity matched groups.  Additionally, this code also performs regression models for the outcome variables of interest. The outputs of this analysis can be obtained from the `output` folder. 

**Shiny House Effect Analysis**
1. Open up `rprogs/shiny_house.Rmd`. This is a markdown script that checks for the "shiny house" effect i.e., controlling for the short-term effects of moving into a brand new house. Running this file creates a markdown report in the `output` folder.

## Citation

Social Investment Agency (2019). Social housing and wellbeing. Source code. https://github.com/nz-social-investment-agency/social-housing-and-wellbeing

## Getting Help
If you have any questions email info@sia.govt.nz

