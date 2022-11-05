---
title: 'Reducing the efforts to create reproducible analysis code with FieldTrip'
tags:
  - FieldTrip Toolbox
  - MATLAB
  - reproducibility
  - analysis pipeline
  - open science
  - MEG
  - EEG
authors:
  - name: Mats W.J. van Es
    orcid: 0000-0002-7133-509X
    affiliation: 1, 2
  - name: Eelke Spaak
    orcid: 0000-0002-2018-3364
    affiliation: 1
  - name: Jan-Mathijs Schoffelen
  - orcid: 0000-0003-0923-6610
    affiliation: 1
  - name: Robert Oostenveld
    orcid: 0000-0002-1974-1293
    affiliation: 1, 3
affiliations:
 - name: Donders Institute for Brain, Cognition and Behaviour, Radboud University 	Nijmegen, The Netherlands
   index: 1
 - name: Oxford Centre for Human Brain Activity, University of Oxford, United Kingdom
   index: 2
 - NatMEG, Karolinska Institutet, Sweden
   index: 3
date: 5 November 2022
bibliography: reproducescript_bib_JOSS.bib



---

# Summary

FieldTrip `@oostenveldFieldTripOpenSource2011` -> "Oostenveld et al. (2011)" is a `@MATLAB2020` -> "MATLAB" toolbox for the analysis of electroencephalography (EEG) and magnetoencephelography (MEG) data. Typically, a researcher will create an analysis pipeline by scripting a sequence of high level FieldTrip functions. Depending on researcher coding style, readability and reproducibility of the custom written analysis pipeline is variable. `reproducescript` is a new functionality in the toolbox that allows complete reproduction of MATLAB-based scripts with little extra efforts on behalf of the user. Starting from the researchers' idiosyncratic pipeline scripts, this new functionality allows researchers to automatically create and publish analysis pipeline scripts in a standardized format, along with all relevant intermediate data and final results.


# Statement of Need
Unsound scientific practices have led to a replication crisis in psychological science in recent years `[@ opensciencecollaborationEstimatingReproducibilityPsychological2015; @ simmonsFalsePositivePsychologyUndisclosed2011]` -> "(Open Science Collaboration, 2015; Simmons et al., 2011)", and it is unlikely that cognitive neuroscience is an exception `[@buttonPowerFailureWhy2013; @gilmoreProgressOpennessTransparency2017; @szucsEmpiricalAssessmentPublished2017]` -> "(Button et al., 2013; Gilmore et al., 2017; Szucs et al., 2017)". One way to combat this crisis is through increasing methodological transparency `[@gilmoreProgressOpennessTransparency2017; @gleesonCommitmentOpenSource2017; @zwaanMakingReplicationMainstream2017]` -> (Gilmore et al., 2017; Gleeson et al., 2017; Zwaan et al., 2017), but the increased sophistication of experimental designs and analysis methods results in data analysis getting so complex that the methods sections of manuscripts in most journals is too short to represent the analysis in sufficient detail. Therefore, researchers are increasingly encouraged to share their data and analysis pipelines along with their published results `@gleesonCommitmentOpenSource2017` -> (Gleeson et al., 2017). However, analysis scripts are often written by researchers without formal training in computer science, resulting in the quality and readability of these analysis scripts to be highly dependent on individual coding expertise and style. Even though the computational outcomes and interpretation of the results can be correct, the inconsistent style and quality of analysis scripts make reviewing the details of the analysis difficult for other researchers that are either involved in the study or not, and the quality of the scripts might compromise the reproducibility of obtained results. The purpose of `reproducescript` is to automatically create analysis pipeline scripts in a standardized format, along with all relevant intermediate data, that are fully reproducible and can directly be shared with peers.

# State of the field
A number of strategies have been proposed to enhance the reproducibility of analysis pipelines and scientific results. One option to improve reproducibility and efficiency through reuse of code is through automation using pipeline systems (e.g. Taverna, Galaxy, LONI, PSOM, Nipype, Brainlife; (10–15) or batch scripts (e.g. SPM’s matlabbatch (16)). Generally, these provide the researcher with tools to construct an analysis pipeline, manage the execution of the steps in the pipeline and, to a varying degree, handle data. 
Some drawbacks of pipeline systems in general are that they require the researcher to learn how the pipeline software works on top of learning the analysis itself, that the execution requires extra software to be installed, or that it requires moving the execution from a local computer to an online (cluster or cloud-based) system, they do not allow interactive analysis steps, and that the flexibility of pipeline systems is limited. For example, MNE-Python, a widely used Python package for the analysis of electrophysiology data has recently implemented the MNE-BIDS-Pipeline, which produces a standardized analysis pipeline. However, the analysis steps that are incorporated in this pipeline, as well as their order is limited. Furthermore, many researchers use MATLAB for analysing their data, which is incompatible with this Python based software package. 


# Example
A detailed document with three examples that build up in complexity can be found on bioRxiv as [Reducing the efforts to create reproducible analysis code with FieldTrip](https://doi.org/10.1101/2021.02.05.429886). These examples have also been incorporated on the [FieldTrip website](https://www.fieldtriptoolbox.org/), see the [example 1](https://github.com/fieldtrip/website/blob/master/example/reproducescript.md), [example 2](https://github.com/fieldtrip/website/eblob/master/xample/reproducescript_group.md), and [example 3](https://github.com/fieldtrip/website/blob/master/example/reproducescript_andersen.md) on the corresponding GitHub pages. Below we list the core usage of the functionality, as well as how it is implemented. Note that more detailed information on the structure of the FieldTrip toolbox can be found at [Introduction to the FieldTrip toolbox](https://github.com/fieldtrip/website/blob/master/tutorial/introduction.md) and [Toolbox architecture and organization of the source code](https://github.com/fieldtrip/website/blob/master/development/architecture.md).

The FieldTrip toolbox is not a program with a user interface where you can click around in, but rather a collection of functions. Each FieldTrip function implements a specific algorithm, for which specific parameters can be specified. These parameters on how the function behaves is passed as a configuration structure.
	
	cfg1                         = [];
	cfg1.dataset                 = 'Subject01.ds';
	cfg1.trialdef.eventtype      = 'backpanel trigger';
	cfg1.trialdef.eventvalue     = 3; % the value of the stimulus trigger for fully incongruent (FIC).
	cfg1.trialdef.prestim        = 1;
	cfg1.trialdef.poststim       = 2;

	
Users mainly use the high-level functions as the main building blocks in their analysis scripts. These functions are executed with the configuration structure (`cfg`) and in most cases with a data structure as inputs. For example:

	cfg1         		= ft_definetrial(cfg1);
	dataPreprocessed	= ft_preprocessing(cfg1);
	
	cfg2         = [];
	dataTimelock = ft_timelockanalysis(cfg2, dataPreprocessed);

When you are using FieldTrip, your analysis protocol is the MATLAB script, in which you call the different FieldTrip functions. Such a script (or set of scripts) can be considered as an analysis protocol, since in them you are defining all the steps that you are taking during the analysis. 

The high-level functions (which take a cfg argument) mainly do data bookkeeping and subsequently pass the data over to the algorithms in the low-level functions. There are a number of features in the bookeeping that are always the same, hence these are shared over all high-level functions using the ft_preamble and ft_postamble functions, which are called at the start and end, respectively.

At the start of the high-level functions the preamble scripts ensure that the MATLAB path is set up correctly, that the notification system is initialized, that errors can be more easily debugged, that input data is read and provenance tracked. At the end of the high-level functions the preamble scripts among others take care of debugging, handle the provenance and save the output data.

The new functionality, called *reproducescript*, which we propose here, is an addition to the configuration structure that calls a number of low level functions in the pre- and postamble scripts which automatically create analysis pipeline scripts in a standardized format, along with all relevant input, intermediate, and output data, which are linked in the standardized script. Together, they represent a non-ambiguous, standardized, and fully reproducible version of the original analysis pipeline. Moreover, this functionality is enabled without much effort from the researcher, namely by embedding the analysis pipeline in the wrapper below. 

	global ft_default
	ft_default = [];
	
	% enable reproducescript by specifying a directory
	ft_default.reproducescript = 'reproduce/';
	
	% the original analysis pipeline with calls to (high level) FieldTrip functions should be written here
	
	% disable reproducescript
	ft_default.reproducescript = [];

`ft_default` is the structure in which global configuration defaults are stored; it is used throughout all FieldTrip functions and global options at the start of the function are merged with the user-supplied options in the `cfg` structure specific to the function. 

The directory containing the reproducible analysis pipeline is structured as below. The standardized script is in `script.m`, all the data files are saved with a unique identifier to which is referred in `script.m`, and `hashes.mat` contains MD5 hashes for bookkeeping all input and output files. It furthermore allows any researcher to check the integrity of all the intermediate and final result files of the pipeline.

	reproduce/
		script.m
		hashes.mat
		unique_identifier1_ft_preprocessing_input.mat
		unique_identifier1_ft_preprocessing_output.mat
		...
		


# Acknowledgements

The authors would like to thank Lau Andersen for publishing his original data and analysis scripts in `@andersenGroupAnalysisFieldTrip2018`  ->  "Andersen et al. (2018)" and his help in executing the pipeline.

- Author MVE is supported by The Netherlands Organisation for Scientific Research (NWO Vid: 864.14.011), and Wellcome Trust (215573/Z/19/Z). 
- Author ES is supported by
- Author JMS is supported by
- Author RO is supported by 

# References
