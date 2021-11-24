---
title: 'Reducing the efforts to create reproducible analysis code with FieldTrip'
tags:
  - FieldTrip
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
    orcid: 0000-0003-0923-6610
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
date: 11 November 2021
bibliography: reproducescript_bib_JOSS.bib



---

# Summary

We describe `reproducescript`, a substantial addition to the FieldTrip `@oostenveldFieldTripOpenSource2011` -> "Oostenveld et al. (2011)" toolbox for the analysis of electro- (EEG) and magneto-encephalography (MEG) data. Typically, with FieldTrip, a researcher creates an analysis pipeline by scripting a sequence of high-level functions. Depending on researcher coding style, readability and reproducibility of the custom written analysis pipeline is variable. `reproducescript` is new functionality in the toolbox that allows complete reproduction of (MATLAB-based) scripts with little extra efforts on behalf of the user. Starting from the researchers' idiosyncratic pipeline scripts, this new functionality allows researchers to automatically create and publish analysis pipeline scripts in a standardized format, along with all relevant intermediate data. This new addition thus greatly facilitates reproducible, open science.


# Statement of Need

Unsound scientific practices have led to a replication crisis in psychological science in recent years `[@ opensciencecollaborationEstimatingReproducibilityPsychological2015; @ simmonsFalsePositivePsychologyUndisclosed2011]` -> "(Open Science Collaboration, 2015; Simmons et al., 2011)", and it is unlikely that cognitive neuroscience is an exception `[@buttonPowerFailureWhy2013; @gilmoreProgressOpennessTransparency2017; @szucsEmpiricalAssessmentPublished2017]` -> "(Button et al., 2013; Gilmore et al., 2017; Szucs et al., 2017)". One way to combat this crisis is through increasing methodological transparency `[@gilmoreProgressOpennessTransparency2017; @gleesonCommitmentOpenSource2017; @zwaanMakingReplicationMainstream2017]` -> (Gilmore et al., 2017; Gleeson et al., 2017; Zwaan et al., 2017), but the increased sophistication of experimental designs and analysis methods results in data analysis getting so complex that the methods sections of manuscripts in most journals is too short to represent the analysis in sufficient detail. Therefore, researchers are increasingly encouraged to share their data and analysis pipelines along with their published results `@gleesonCommitmentOpenSource2017` -> (Gleeson et al., 2017). However, analysis scripts are often written by researchers without formal training in computer science, resulting in the quality and readability of these analysis scripts to be highly dependent on individual coding expertise and style. Even though the computational outcomes and interpretation of the results can be correct, the inconsistent style and quality of analysis scripts make reviewing the details of the analysis difficult, and the quality of the scripts might compromise the reproducibility of obtained results. The purpose of `reproducescript` is to automatically create analysis pipeline scripts in a standardized format, along with all relevant intermediate data, that are fully reproducible and can directly be shared with peers.


# Examples



# Acknowledgements

The authors would like to thank Lau Andersen for publishing his original data and analysis scripts in `@andersenGroupAnalysisFieldTrip2018`  ->  "Andersen et al. (2018)" and his help in executing the pipeline.

# References
