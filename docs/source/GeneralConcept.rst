DoseCalcs Simulations
=====================


Simulation Units
----------------

The DoseCalcs execution scenario can be done in three computation modes : sequential (SEQ), multi-threading (MT) and Message Passing Interface (MPI). For each scenario three run modes are available, for data generation mode (Gen), calculation mode (B), and graphical visualization mode (G) only for (SEQ) and (MT) scenarios. After commands parameters initialization by messenger classes, the simulation needs five data types which are used in the five simulation steps, material and geometry constructing, physics setting, source defining, simulation run and scoring, and results analysis. Each simulation step is performed basing on the appropriate commands.

Build a Simple Example
----------------------

To simulate a dosimetry problem with DoseCalcs application, it's necessary to uses to user files as an inputs data, first is the input commands file, second is the geometry description file(or files if needed). The input file contains the commands to set the needed parameter for each simulation unit, such as geometry (materials and volumes and/or geometry file), physics, source, run and score, analysis. Each category of those uses a specific commands.

Input command Files
+++++++++++++++++++

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
    /AnalysisData/setGraphsParameters yes no yes yes RightTop .pdf

2. How the data are used


Geometry Input Files
++++++++++++++++++++

Each geometry type uses a specific geometry file, the geometry files that can be used by DoseCalcs:

GDML Geometry File : Contains all geometry description data, where materials, solids, logical and physical volumes parameters are given in tags format.

TEXT Geometry File : Contains all geometry description data, where materials, solids, logical and physical volumes parameters are given in TEXT format.

STL Solids Files : Each solid is described by an STL file, adding the materials given in input commands file to construct geometry volumes.

Voxel IDs Geometry File : Contains IDs, each ID represents a specific material that fill the correspondent voxel.

DICOM Files : Each file contains a slice data, DoseCalcs can read slice pixels that represents either density or activity.

Execution
---------

Execution Command
+++++++++++++++++

 .. code-block:: bash

    $ ./core [Run Mode] [input file] [Events Number Per Thread]

[input file] : inputs commands file

[Events Number Per Thread] : for sequential execution, a computer is considered as one core (one thread). Whereas for Multi-Threaded or MPI computation modes, each rank simulate this number on thread or rank respectively.

Computation Modes
+++++++++++++++++

1. Sequential Execution Command

 .. code-block:: bash

    $ ./core B inp.mac 100000

Total number of events in simulation is 100000

2. Multi-threaded(MT) Execution Command

 .. code-block:: bash

    $ ./core B inp.mac 100000

100000 Events per Thread. The total number of events in the simulation will be 100000*ThreadNumber. ThreadNumber is set by command /RunAndScoreData/setNumberOfThreads

This mode is used when Geant4 is built in multi-threading.

3. MPI Execution Command

 .. code-block:: bash

    $ /home/../mpirun -np [Rank Number] core B inp.mac 100000

[Rank Number] : number of parallel simulations on the cluster, where each simulation runs on a cluster unit(i.e core). DoseCalcs uses different Events Data for each simulation in order to avoid the results repetition.

100000 Events per Rank. The total number of events in the simulation will be 100000*[Rank Number].

This mode is used when the DoseCalcs is built with WITH_G4MPI_USE=ON, and and setting the OpenMPI1.8.1 path.

Run Modes
+++++++++

[Run Mode] : can be : B (Batch "default if we don't set [Run Mode]), G (Graphical), T (Terminal) or Gen (Generation).

1. G Run Mode

G mode is used just for geometry visualization in Sequential mode (DoseCalcs built with -DWITH_G4MPI_USE set to OFF), then we can set the commands /SourceData/TestEventsInitialPositions and /SourceData/ShowSourceBox

 .. code-block:: bash

    $ ./core G [input file] [.]

[.] : can be : v or empty value which mean visualize geometry with Qt interface using OGL driver; d means download the geometry image in PS format, by specifying the image axis and and depth on this axis; visualization macros file to visualize your geometry with your own Geant4 visualization commands.

2. Gen Run Mode

Gen mode is used just for events data generation, this data which is used after in the simulation with B, G, or T run modes. The data to generate must be described in inputs commands file using a set of /SourceData/ commands.

In  Multi-threaded(MT) or Sequential execution modes
 .. code-block:: bash

    $ ./core Gen [input file]

The data units are generated progressively, begins with initial positions in source volume 1, then source volume 2, ..., energy 1, energy 2 ..., momentum direction.

In  MPI execution modes
 .. code-block:: bash

    $ /home/../mpirun -np [Rank Number] core Gen [input file]

[Rank Number] : must be equal to the data that will be generated, where each rank generates a data unit, for example, to generate initial positions in three source volumes, two energies, and one momentum direction, the [Rank Number] must be set to 6.

3. B Run Mode

B mode is used to simulate the events interactions and gives the dosimetric quantities outputs scored.

Results
-------

DoseCalcs Results are given in text format files, each thread calculation produce its own results, whereas the master thread produce SimData.txt file, which contains the geometry regions data and the needed user inputs in global results calculation and analysis. Note that the global results calculation produce ResultsData.txt, and the analysis produce graphs and histograms based on the data given in ResultsData.txt, SimData.txt and events data files.

SimData.txt
+++++++++++

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

How to get this results
+++++++++++++++++++++++

1. Which Computation mode!

Sequential : 

Multi-threading : 

MPI : 

2. merge executable and input file

Analysis
--------

prequisites to analysis 
+++++++++++++++++++++++

1. Setting ROOT install directory 

2. /AnalysisData/ commands

analysis executable and input file  
++++++++++++++++++++++++++++++++++

Analysis output directory and files  
+++++++++++++++++++++++++++++++++++

Build directory Final Structure
-------------------------------



