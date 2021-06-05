DoseCalcs Simulations
=====================


Simulation Units
----------------

The DoseCalcs execution scenario can be done in three computation modes : sequential (SEQ), multi-threading (MT) and Message Passing Interface (MPI). For each scenario three run modes are available, for data generation mode (Gen), calculation mode (B), and graphical visualization mode (G) only for (SEQ) and (MT) scenarios. After commands parameters initialization by messenger classes, the simulation needs five data types which are used in the five simulation steps, material and geometry constructing, physics setting, source defining, simulation run and scoring, and results analysis. Each simulation step is performed basing on the appropriate commands. To achieve the simulation task, there are two files that can be considered as principal code inputs, macro command file and geometry file as shown in the figure 

Build a Simple Example
----------------------

To simulate a dosimetry problem with DoseCalcs application, it's necessary to uses to user files as an inputs data, first is the input commands file, second is the geometry description file(or files if needed). The input file contains the commands to set the needed parameter for each simulation unit, such as geometry (materials and volumes and/or geometry file), physics, source, run and score, analysis. Each category of those uses a specific commands.

The simulation needs five data types which are used in the five simulation steps, material and geometry constructing, physics setting, source defining, simulation run and scoring, and results analysis. Each simulation step is performed basing on the appropriate commands.

Input command Files
+++++++++++++++++++

The macro(or input) file is an ASCII file based on the Geant4 macro syntax. The code loads this file to initialize the simulation by selecting the parameters from commands given in this file, then avoiding the need to recompile the source code for any change in the simulation.

1. Input file example

 .. code-block::
    :linenos:
    :emphasize-lines: 1,15,22,27,34,40

    #######################################################  Materials  ########################
    /MaterialData/createElement 1 1.008 Hydrogen
    /MaterialData/createElement 6 12.011 Carbon
    /MaterialData/createElement 7 14.007 Nitrogen
    /MaterialData/createElement 8 16 Oxygen
    /MaterialData/createElement 11 12.9898 Sodium
    /MaterialData/createElement 12 30.974 Phosphorus
    /MaterialData/createElement 16 32.065 Sulfur
    /MaterialData/createElement 17 35.453 Chlorine
    /MaterialData/createElement 19 39.0983 Potassium
    /MaterialData/createMaterial Phantom 95 9 1.05 g/cm3 frac
    /MaterialData/addElements Hydrogen 10.2 Carbon 13.1 Nitrogen 3.1 Oxygen 72.4 Sodium 0.2 Phosphorus 0.2 Sulfur 0.3 Chlorine 0.2 Potassium 0.3
    /MaterialData/setNistMaterialNameAndID G4_AIR 1

    #######################################################  Geometry ##########################
    /GeometryData/createWorld World G4_AIR yes 20. 20. 20. cm
    /GeometryData/createSolid STL solid11 Scripts/solid11.stl
    /GeometryData/createVolume STLVol11 solid11 G4_AIR World 8. 8. 8. 30. 10. 130. cm degree
    /GeometryData/createSolid Box BoxSol 50. 40. 60. cm
    /GeometryData/createVolume BoxVol BoxSol Phantom World 80. 80. 80. 30. 10. 130. cm degree

    #######################################################  Physics  ##########################
    /PhysicsData/setPhysicsData EMS3
    /PhysicsData/setCutsData 1. 1. mm keV
    /PhysicsData/generateCrossSectionFor gamma MeV 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1

    #######################################################  Source  ###########################
    /SourceData/setEventsParticleNameData gamma
    /SourceData/setEventsInitialPosData cm Volume STLVol11 2. 2. 2. BoxVol 1.2 1.2 1.2
    /SourceData/setEventsInitialEneData MeV Mono 0.01 0.02 0.015 0.03 0.05 0.1 0.2 0.5 1
    /SourceData/setEventsInitialMomDirData degree Isotropic
    /SourceData/setSourceGenerationData 10000 yes yes yes

    #######################################################  Run and Score  ####################
    /RunAndScoreData/setRegionsToScore STLVol11 BoxVol
    /RunAndScoreData/setQuantitiesToScore SAF
    /RunAndScoreData/setNumberOfThreads 4
    /RunAndScoreData/setSimNumOnRanks m

    #######################################################  Analysis  #########################
    /AnalysisData/generateSelfCrossGraphs Reference_Result Self_Cross MIRD /home/tarik/Desktop/WorkSpace/Projects/DoseCalcsCore/Results/ReferenceData.txt
    /AnalysisData/generateRelativeErrGraph
    /AnalysisData/generateRelativeSDevGraph
    /AnalysisData/generateVariableRegionGraph Mass
    /AnalysisData/generateEventsDataHisto
    /AnalysisData/setGraphsParameters yes no yes yes RightTop 0.15 0.23 .pdf

2. How the data are used


Geometry Input Files
++++++++++++++++++++

Each geometry type uses a specific geometry file, the geometry files that can be used by DoseCalcs:

GDML Geometry File : Contains all geometry description data, where materials, solids, logical and physical volumes parameters are given in tags format.

TEXT Geometry File : Contains all geometry description data, where materials, solids, logical and physical volumes parameters are given in TEXT format.

STL Solids Files : Each solid is described by an STL file, adding the materials given in input commands file to construct geometry volumes.

.. Voxel IDs Geometry File : Contains IDs, each ID represents a specific material that fill the correspondent voxel.

.. DICOM Files : Each file contains a slice data, DoseCalcs can read slice pixels that represents either density or activity.

Execution
---------

Execution Command
+++++++++++++++++

Of course, DoseCalcs code building requires Geant4 to be installed first, and it is preferred that Geant4 be built for multi-threading computation mode. Then, to run DoseCalcs sequentially. With a command line processing interface, the execution needs three parameters to be specified by the user [Run Mode], [input file] and [Events Number Per Thread].

 .. code-block:: bash

    $ ./simulate [Run Mode] [input file] [Events Number Per Thread]

[input file] : inputs commands file

[Events Number Per Thread] : for sequential execution, a computer is considered as one core (one thread). Whereas for Multi-Threaded or MPI computation modes, each rank simulate this number on thread or rank respectively.

Computation Modes
+++++++++++++++++

1. Sequential Execution Command

 .. code-block:: bash

    $ ./simulate B inp.mac 100000

Total number of events in simulation is 100000

2. Multi-threaded(MT) Execution Command

 .. code-block:: bash

    $ ./simulate B inp.mac 100000

100000 Events per Thread. The total number of events in the simulation will be 100000*ThreadNumber. ThreadNumber is set by command /RunAndScoreData/setNumberOfThreads

This mode is used when Geant4 is built in multi-threading.

3. MPI Execution Command

In MPI computation mode, the mpiexec or mpirun command should be set firstly as given in the following command:

 .. code-block:: bash

    $ /home/../mpirun -np [Rank Number] ./simulate B inp.mac 100000

[Rank Number] : number of parallel simulations on the cluster, where each simulation runs on a cluster unit(i.e core). DoseCalcs uses different Events Data for each simulation in order to avoid the results repetition.

100000 Events per Rank. The total number of events in the simulation will be 100000*[Rank Number].

This mode is used when the DoseCalcs is built with WITH_G4MPI_USE=ON, and and setting the OpenMPI1.8.1 path.

Run Modes
+++++++++

The [Run Mode] can be one of B(batch), G(Graphical) or Gen(Generation) modes. The Graphical mode is used to visualize(v) the geometry read by DoseCalcs, as well as to test the events initial positions by setting the command /TestEventsInitialPositions. It allows also the box dimensions visualization by setting the command /ShowSourceBox in the input file. As mentioned before, the graphical mode can be used only in sequential and multi-threading computation modes. However, MPI computation mode is directed just for Batch and Generation Run Modes. The Batch run mode is dedicated to simulation and scoring and the Gen run mode is used for events data generation such as initial positions, energies and momentum directions. 

[Run Mode] : can be : B (Batch "default if we don't set [Run Mode]), G (Graphical), T (Terminal) or Gen (Generation).

1. G Run Mode

G mode is used just for geometry visualization in Sequential mode (DoseCalcs built with -DWITH_G4MPI_USE set to OFF), then we can set the commands /SourceData/TestEventsInitialPositions and /SourceData/ShowSourceBox

 .. code-block:: bash

    $ ./simulate G [input file] [.]

[.] : can be : v or empty value which mean visualize geometry with Qt interface using OGL driver; d means download the geometry image in PS format, by specifying the image axis and and depth on this axis; visualization macros file to visualize your geometry with your own Geant4 visualization commands.

2. Gen Run Mode

Gen mode is used just for events data generation, this data which is used after in the simulation with B, G, or T run modes. The data to generate must be described in inputs commands file using a set of /SourceData/ commands.

In  Multi-threaded(MT) or Sequential execution modes
 .. code-block:: bash

    $ ./simulate Gen [input file]

The data units are generated progressively, begins with initial positions in source volume 1, then source volume 2, ..., energy 1, energy 2 ..., momentum direction.

In  MPI execution modes
 .. code-block:: bash

    $ /home/../mpirun -np [Rank Number] ./simulate Gen [input file]

[Rank Number] : must be equal to the data that will be generated, where each rank generates a data unit, for example, to generate initial positions in three source volumes, two energies, and one momentum direction, the [Rank Number] must be set to 6.

3. B Run Mode

B mode is used to simulate the events interactions and gives the dosimetric quantities outputs scored.

Supposing that we have as an input, A particles(Part), B source Regions(Src), C energies(Ene) and D momentum directions(MomDir), the data generation and the calculation tasks are distributed among the ranks (or threads). As a consequence, the number of ranks(or threads) needed to generate these events data is N=B+C+D, where each rank generates a specific data file, whereas the number of ranks (or threads) needed for simulations will be the number of combinations Part-Src-Ene-MomDir which is ( N = A * B * C * D ). An Additional option, is the capability to simulate one Part-Src-Ene-MomDir combination (A=B=C=D=1 -> N=1) on all ranks (or threads) which means one simulation on all ranks using the same source data. Note that the total number of simulated events in this case for one simulation (for one combination) is ([TotalEventsNumber]=N * [EventsNumber]).

 .. image:: /images/SimModeGen.png

 .. image:: /images/SimModeGen.png

For Multi-threading or MPI modes, the total number of simulated events is the EventsNumber multiplied by thread number or rank number respectively. Note that this total number should not exceed the maximum G4int value. 

Results
-------

DoseCalcs Results are given in text format files, each thread calculation produce its own results, whereas the master thread produce SimData.txt file, which contains the geometry regions data and the needed user inputs in global results calculation and analysis. Note that the global results calculation produce ResultsData.txt, and the analysis produce graphs and histograms based on the data given in ResultsData.txt, SimData.txt and events data files.

SimData.txt
+++++++++++

The file called SimData is containing simulation data with the needed parameters to generate the final results file, and to be used also by [analysis] executable as an input file to generate graphs, histograms and tables. First, the regions data are written after ">> Regions Data" tag. All the needed simulation data are written after ">> Inputs Data" tag. 

 .. code-block::
    :linenos:
    :emphasize-lines: 1, 15

    >> Regions Data

    # Region Name                    Mass(Kg)            Density(g/cm3)      Volume(cm3)         PosX(mm)            PosY(mm)            PosZ(mm)
    Adrenal                          0.0155403           0.9869              15.7466             -45                 -80                 345.5
    Heart                            0.597847            0.9869              605.783             -1.50876e-14        30                  468.7
    Liver                            1.80617             0.9869              1830.14             0                   0                   315.5
    Lung                             0.999564            0.2958              3379.19             73.3                -1.83575e-14        392.1
    Kidney                           0.284054            0.9869              287.825             51.8                -73                 293
    Pelvis                           0.324583            1.4862              218.398             3.00957e-14         29.4                99.15
    Spleen                           0.173426            0.9869              175.728             -107.9              -39.4               333.5
    Stomach                          0.396212            0.9869              401.471             -69                 39.2                315.5
    Thymus                           0.0248035           0.9869              25.1327             -20                 60                  570.5
    Pancreas                         0.0602709           0.9869              61.0709             20                  -30                 305.5

    >> Inputs Data

    CutsDistance                     0.1
    CutsEnergy                       0.001
    particleName                     gamma
    particleSourceEnergy             1
    SourceType                       Volume
    SourceRegionName                 Liver
    EnergyDistribution               Mono
    MomDirDistribution               Isotropic
    GaussSDev                        0
    GaussMean                        0
    UniformEmin                      0
    UniformEmax                      0
    RayleighEmax                     0
    MonoEnergy                       1
    GraphsData                       Reference_Result
    CompareType                      Self_Cross
    GraphsExt                        .pdf
    RefFilePath                      /home/tarik/Desktop/WorkSpace/Projects/DoseCalcsCore/Results/ReferenceData.txt
    RefName                          MIRD
    GenerateRegionsVariableGraph     yes
    RegionVariableName               Mass
    GenerateRelativeSDevGraph        yes
    GenerateRelativeErrGraph         yes
    PositionDataFile                 EventsData/Pos_Liver_Volume_200000.bin
    EnergyDataFile                   EventsData/Ene_Mono_1_200000.bin
    MomDirDataFile                   EventsData/MomDir_Isotropic_200000.bin
    EventsDataHistograms             yes
    MPISimulationNum                 m
    QuantitiesToScore                SAF
    OrgansNamesToScoreString         Liver Kidney Adrenal
    AccuracyCalculationLevel         StepLevel
    ExecutionMode                    MT
    NumberOfRanks                    1

ResultsData.txt
+++++++++++++++

The text file contains the internal dosimetry quantities (AE, AD, AF, SAF, S, H and E) for each source region, particle name, particle energy and target region. It is structured as a sequence of text lines summarizing obtained data for each target, such as quantity value, standard deviation, relative standard deviation in percent, interaction number in the target, etc.

For each simulation, i.e source volume, particle and energy combination, the obtained scores are appended to results file. The generated data are written in a simple format, first line as a simulation header file which contains simulation data such as scored quantity, source volume name, particle name, etc. followed by data lines, each line contains the results for a scored volume.

 .. code-block::
    :linenos:
    :emphasize-lines: 1, 11, 21, 31

    ****** SAF Liver gamma 0.01 0.1mm 0.001MeV 40000000 83463224 Isotropic Mono MPI 0 1.40279e-09Sv StepLevel Liver 0.0101314% Adrenal 4.01135% 54.3456min
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   5.362184e-01   6.707596e-13   0.010      80992726    1.811      1834.710       0.987
    Spleen                  0.000000e+00   0.000000e+00   0.000      0           0.173      175.728        0.987
    Pancreas                2.467882e-04   5.186450e-09   2.585      1230        0.060      60.967         0.987
    Lung                    1.708084e-04   9.064944e-11   0.760      14318       1.000      3381.770       0.296
    Kidney                  3.561619e-05   1.310085e-09   3.112      846         0.284      287.823        0.987
    Adrenal                 4.061095e-04   3.005619e-08   4.011      542         0.016      15.711         0.987
    Thyroid                 0.000000e+00   0.000000e+00   0.000      0           0.019      19.358         0.987
    * ----------------------------------------------------------------------------------------------------------------------------------------------
    ****** SAF Adrenal gamma 0.05 0.1mm 0.001MeV 40000000 214327182 Isotropic Mono MPI 10 9.95363e-08Sv StepLevel Trunk 0.0184312% Skin 27.3457% 75.8041min
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   2.501552e-02   8.048937e-13   0.053      16544890    1.811      1834.710       0.987
    Spleen                  2.870195e-02   2.549083e-11   0.160      1801397     0.173      175.728        0.987
    Pancreas                1.335481e-02   1.822805e-10   0.395      289293      0.060      60.967         0.987
    Lung                    1.902117e-02   2.087781e-12   0.075      6866491     1.000      3381.770       0.296
    Kidney                  6.947108e-02   7.654241e-12   0.081      7380409     0.284      287.823        0.987
    Adrenal                 2.308446e+00   9.750009e-11   0.062      14607902    0.016      15.711         0.987
    Thyroid                 3.971891e-04   5.855831e-09   3.995      2710        0.019      19.358         0.987
    * ----------------------------------------------------------------------------------------------------------------------------------------------
    ****** SAF Kidney gamma 0.03 0.1mm 0.001MeV 40000000 145503849 Isotropic Mono MPI 15 3.14723e-08Sv StepLevel Trunk 0.0184561% Thyroid 48.9205% 63.2902min
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   1.602105e-02   2.354789e-12   0.072      4902389     1.811      1834.710       0.987
    Spleen                  3.963621e-02   5.041373e-11   0.148      1164538     0.173      175.728        0.987
    Pancreas                2.581236e-02   3.040796e-10   0.311      263860      0.060      60.967         0.987
    Lung                    1.847074e-03   1.752996e-11   0.283      297900      1.000      3381.770       0.296
    Kidney                  1.099643e+00   4.609239e-12   0.022      53641500    0.284      287.823        0.987
    Adrenal                 9.895541e-02   1.179701e-09   0.316      265235      0.016      15.711         0.987
    Thyroid                 3.645785e-06   1.783536e-07   48.920     10          0.019      19.358         0.987
    * ----------------------------------------------------------------------------------------------------------------------------------------------
    ****** SAF Pancreas gamma 1 0.1mm 0.001MeV 40000000 538757866 Isotropic Mono MPI 26 6.74178e-07Sv StepLevel Trunk 0.00563887% Skin 8.92441% 173.907min
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   2.564405e-02   8.157255e-14   0.015      45817911    1.807      1830.910       0.987
    Spleen                  1.289698e-02   3.365621e-12   0.067      2580908     0.173      175.728        0.987
    Pancreas                6.678639e-01   2.640210e-12   0.015      37145582    0.060      61.052         0.987
    Lung                    3.877325e-03   2.070569e-13   0.036      6758849     1.000      3379.400       0.296
    Kidney                  2.220658e-02   1.242771e-12   0.040      7089068     0.284      287.559        0.987
    Adrenal                 1.071874e-02   9.966240e-11   0.240      257754      0.016      15.713         0.987
    Thyroid                 4.844505e-04   3.121624e-10   1.029      15964       0.019      19.303         0.987
    * ----------------------------------------------------------------------------------------------------------------------------------------------

The file above represent results for 4 simulations, each simulation is indicated by the a header file and the corresponding scores quantity data in the scored region, each scored region data is given in a separated line.

1. Header line

SAF 		: scored quantity
Liver 		: source region name
gamma 		: particle name
1 		: particle initial energy in (MeV)
Mono 		: particle energy distribution
Isotropic 	: particle momentum direction distribution
GDML 		: Geomeytry file format
400000Event 	: total number of simulated events
13737602Step 	: total number of simulated steps
0.1mm 		: cut in range
0.001MeV	: energy threshold
1Wr		: radiation factore
MT		: computation mode
0		: Rank ID
StepLevel	: accuracy calculation level
400000MeV	: total emitted energy
227380MeV	: total ebsorbed energy in phantom
6,02386e-09Sv		: effective dose 
Liver 0,0597098%		: minimum registered Standard Deviation
Eye_lense_right 98.6154%	: maximum registered SD
0,000307729ms	: mean dutarion for events simulation 
1.95382min	: mean dutarion for simulation

2. Scored Data lines

The results for each scored volume are written in the format: 
 
Scored_Region Scored_Quantity Standard_Deviation Relative_Standard_Deviation Number_Of_Steps Region_Mass  Region_Volume Region_Density
    
How to get this results
+++++++++++++++++++++++

1. Which Computation mode!

DoseCalcs code provides three computation modes, sequential, multi-threading and MPI. And for each computation mode, three run modes are available, for data generation mode (Gen), calculation mode (B), and graphical visualization mode (G) only for (SEQ) and (MT) scenarios.

Sequential : simulation of total number of events on one thread (master thread)

Multi-threading :simulation of total number of events is divided on the number of threads, and each thread simulates corresponding events number with the same source data.

MPI : each rank simulates one run sequentially and the total number of runs will be the number of ranks, each rank simulates a specific source data separately.

2. merge executable and input file

In the case a simulation on a rank crashed due to memory problem, the merge executable which is built as a helper for results calculation, can be used to generate global results from threads or ranks files that achieved successfully the simulation and have produced the results file related to their own thread or rank.

The results calculation steps are based on three tupe of files, simulation data file [SimData], tissue and radiation factors files, to be used in in the calculation of of equivalent and effective dose, and thread or rank region result data files, with the objective of getting all the needed data, to merge threads or ranks results then calculating and writing the final simulation results to the file [ResultsData] .

The printed quantities in the scored volumes are those passed by user commands. Where for each simulation, i.e source volume, particle and energy combination, the obtained scores are appended to results file [ResultsData], the generated data are written in a simple format, first line as a simulation header file which contains simulation data such as scored quantity, source volume name, particle name, etc. followed by data lines, each line contains the results in a scored volume. 

Analysis
--------

prequisites to analysis
+++++++++++++++++++++++

The purpose of a graph or histogram is to present data that are too numerous or complicated to be described adequately in the text and in less space. In order to make graphs and histograms easily, [analysis] executable has been developed as a direct interface to ROOT Analysis System ROOT. This executable serves to generate graphs according to the /AnalysisData/ commands parameters. Note that this command parameters such as graph type, comparing type, reference name, reference file path, and other analysis commands parameters, must be given in the macros file DoseCalcs save them to the simulation data file [SimData] created [simulate] executable. This file is to be reused beside the results file like the one shown in figure [ResultsFile] as an input file for analysis tasks. However, the parameters in [SimData] file should be adjusted to create the desired analysis tasks by [analysis]. 

To use the [analysis] executable, two coditions are needed:

1. The user must build the DoseCalcs code with option WITH\_ANALYSIS\_USE=ON and set the ROOT\_Dir=/../../root . This will build an executable called analysis.

 .. code-block:: bash
  
   -DWITH_ANALYSIS_USE=ON  -DROOT_DIR=/home/.../RootInstallDir


2. The first /AnalysisData/generateSelfCrossGraphs parameter should point to Result or Reference_Result value, and if the value is Result_Reference, the reference data file should be given and written in a specific text format in order to be readable by [analysis] executable. An example of this command is given below :

 .. code-block:: bash
  
  /AnalysisData/generateSelfCrossGraphs Reference_Result Self_Cross .pdf MIRD /home/User/DoseCalcs/Results/ReferenceData.txt 

After building DoseCalcs with setting Root install directory, by executing the [analysis] executable can generate graphs according to the /AnalysisData/generateSelfCrossGraphs command parameters. Note that this command parameters are such as comparing type, graph type, reference name, reference file path, and other analysis commands parameters.

A Self-absorption graph is a graph that contains data when the source and target organs are the same. Cross-irradiation graph or source graph contains data of all target organs that receive the radiation from a unique source organ. Given the reference data in a file with a simple determined format, the code can compare the results of the simulation with this reference data automatically and produces graphs to make the measurement of the simulation accuracy easy, in order to search in the physical context the origin of the discrepancy between the reference data and code results.

The graphs and histograms are created by a direct interface to ROOT Analysis System which supports multi files format of the generated graphs and histograms. The user can choose the appropriate format in DoseCalcs. For the graphs type, the self-absorption and cross-irradiation data can be generated, besides other graphs such as relative errors and relative standard deviation, macroscopic cross section, etc. In addition, the simulation inputs such as initial positions, energies and momentum directions can be drawn in a 2D and 3D histograms.

analysis executable and input file
++++++++++++++++++++++++++++++++++

The the simple command line to analysis is:

 .. code-block:: bash
  
   $ ./analysis

the input file of analysis is the SimData file created by calculation. After the execution, the resulted files are produced in the Results/Graphs_Histograms directory.

Analysis output directory and files
+++++++++++++++++++++++++++++++++++

Build directory Final Structure
-------------------------------



