Applications
============

Geometry from GDML format file 
------------------------------

GDML file format (MIRDPhantomVol.gdml) 

Input File (inp.mac)
++++++++++++++++++++

 .. code-block::
    :linenos:
    :emphasize-lines: 1,4,9,16,21

    #######################################################  Geometry ##########################
    /GeometryData/createVolume GDML Scripts/MIRDPhantomVol.gdml
    
    #######################################################  Physics  ##########################
    /PhysicsData/setPhysicsData EMS3
    /PhysicsData/setCutsData 1. 1. mm keV
    /PhysicsData/generateCrossSectionFor gamma MeV 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1

    #######################################################  Source  ###########################
    /SourceData/setEventsParticleNameData gamma
    /SourceData/setEventsInitialPosData cm Volume Liver 15. 8. 8. Adrenal 10.5 0.5 4.96 Kidney 16. 2. 5.
    /SourceData/setEventsInitialEneData MeV Mono 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1
    /SourceData/setEventsInitialMomDirData degree Isotropic
    /SourceData/useDataGenerationFiles 40000000 yes yes yes

    #######################################################  Run and Score  ####################
    /RunAndScoreData/setRegionsToScore Liver Adrenal Kidney
    /RunAndScoreData/setQuantitiesToScore SAF
    /RunAndScoreData/setSimNumOnRanks m

    #######################################################  Analysis  #########################
    /AnalysisData/generateSelfCrossGraphs Reference_Result Self_Cross MIRD Results/ReferenceData.txt
    /AnalysisData/generateRelativeErrGraph
    /AnalysisData/generateRelativeSDevGraph
    /AnalysisData/generateVariableRegionGraph Mass
    /AnalysisData/generateEventsDataHisto
    /AnalysisData/setGraphsParameters yes no yes yes RightTop 0.15 0.23 .pdf

1. Simulation inputs 

The Simulation volume will be read from the Volume.gdml file, where all the volumes material and shapes are described by gdml tags to describe MIRD phantom regions, the physics used to track the particles is EMS3 (i.e. ElectromagneticOtandard_3), the cut in range was 1 mm and energy threshold was 1 keV.

For source data, first, the data generation has been done for initial positions in Liver, Adrenal and kidney. The initial Mono energies were 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1, and the isotropic distribution of momentum direction are is used to generate events momentum directions.  
 
In simulation of calculation mode, the scored quantity was Specific Absorbed Fraction (SAF), and it's scored in three regions whose are Liver, Kidney and Adrenal. Using the MPI computation mode, we set "m" value to indicate that each rank will simulate a couple of Particle-SourceRegion-Energy-MomentumDirection. Where the total number of ranks to be used should be 27. 

The activation of using data generation in files by the command /SourceData/useDataGenerationFiles is used, and the 40000000 initial data for events are generated.
 
Execution and Results
+++++++++++++++++++++

1. Execution Commands

The first run mode was the generation (Gen) mode, in order to accelerate the calculation mode while getting events data, the computation is done in MPI mode, and the used ranks was 13 ranks to generate initial positions in 3 source regions, 9 energies, and for 1 momentum direction distribution. With the command below:

 .. code-block:: bash

    $ /home/../mpirun -np 13 ./simulate Gen inp.mac 

The second run mode was calculation mode (B), in order to accelerate the calculation mode while getting events data, the computation is done in MPI mode, and the used ranks was 27 ranks to run 27 simulations. With the command below:

 .. code-block:: bash

    $ /home/../mpirun -np 27 ./simulate B inp.mac 

2. Results 

The results of calculation for energy 1 MeV and all simulated source regions are shown below in the following ResultsData file. 

 .. code-block::
    :linenos:
    :emphasize-lines: 1,7,13
    
    ****** SAF Kidney gamma 1 0.1mm 0.001MeV 40000000 433410544 Isotropic Mono MPI 14 3.04747e-07Sv StepLevel Trunk 0.00611567% Rib 99.7072% 132.151min 
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   1.310077e-02   1.092875e-13   0.021      24713684    1.807      1830.910       0.987          
    Kidney                  2.295299e-01   4.773080e-13   0.011      54947494    0.284      287.559        0.987          
    Adrenal                 3.756827e-02   5.787328e-11   0.124      803426      0.016      15.713         0.987          
    * ----------------------------------------------------------------------------------------------------------------------------------------------
    ****** SAF Adrenal gamma 1 0.1mm 0.001MeV 40000000 433800287 Isotropic Mono MPI 8 1.13608e-06Sv StepLevel Trunk 0.00587664% Rib 51.8396% 132.138min 
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   1.538729e-02   1.014808e-13   0.019      28524604    1.807      1830.910       0.987          
    Kidney                  3.777749e-02   1.009186e-12   0.029      11042596    0.284      287.559        0.987          
    Adrenal                 1.330778e+00   1.164446e-11   0.021      23431533    0.016      15.713         0.987          
    * ----------------------------------------------------------------------------------------------------------------------------------------------
    ****** SAF Liver gamma 1 0.1mm 0.001MeV 40000000 443598449 Isotropic Mono MPI 2 1.31757e-07Sv StepLevel Trunk 0.00670617% Rib 87.4197% 98.404min 
    # Volume                SAF            SDev           Rel SDev % Values Num  Mass (Kg)  Volume (cm3)   Density (g/cm3)
    Liver                   8.089826e-02   5.281887e-14   0.008      117752835   1.807      1830.910       0.987          
    Kidney                  1.304899e-02   1.598432e-12   0.052      4235494     0.284      287.559        0.987          
    Adrenal                 1.542536e-02   8.798033e-11   0.198      346666      0.016      15.713         0.987          
    * ----------------------------------------------------------------------------------------------------------------------------------------------

The calculation mode generate SimData file that contains all simulated region's data, and other simulation data such as source and analysis data to be used for ROOT graphs and tables generation  

 .. code-block::
    :linenos:
    :emphasize-lines: 1,36
    
    >> Regions Data

    # Region Name                     Mass(Kg)            Density(g/cm3)      Volume(cm3)         PosX(mm)            PosY(mm)            PosZ(mm)            
    Adrenal                           0.0154882           0.9869              15.6938             -45                 -80                 345.5               
    Clavicle                          0.0815661           1.4862              54.8823             -3.94949e-14        -3.94949e-14        638                 
    Heart                             0.596889            0.9869              604.812             -1.50876e-14        30                  468.7               
    Liver                             1.80469             0.9869              1828.64             0                   0                   315.5               
    Lung                              1.00054             0.2958              3382.49             73.3                -1.83575e-14        392.1               
    Kidney                            0.284473            0.9869              288.249             51.8                -73                 293                 
    Pelvis                            0.326348            1.4862              219.585             3.00957e-14         29.4                99.15               
    Spleen                            0.173426            0.9869              175.728             -107.9              -29.4               333.5               
    Stomach                           0.396212            0.9869              401.471             -69                 39.2                315.5               
    Thymus                            0.0248035           0.9869              25.1327             -20                 60                  570.5               
    Pancreas                          0.0601849           0.9869              60.9838             50                  -2.44929e-16        267.5               
    Ovary                             0.00828098          0.9869              8.3909              51.8                1.57367e-14         135.2               
    SmallIntestine                    1.00603             0.9869              1019.39             1.9178e-14          26.6                185.5               
    UpperLargeIntestine               0.327407            0.9869              331.753             73.3                23.1                173.3               
    UrinaryBladder                    0.24492             0.9869              248.171             3.52086e-14         44.1                72.1                
    MiddleLowerSpine                  0.852692            1.4862              573.74              -2.60789e-14        -53.9               474.55              
    LowerLargeIntestine               0.260717            0.9869              264.178             -78.6               23.1                147.6               
    ArmBone                           1.41974             1.4862              955.285             158.7               -1.94351e-14        315.5               
    Brain                             1.45141             0.9869              1470.67             -1.01033e-14        -1.01033e-14        795                 
    Thyroid                           0.0190944           0.9869              19.3479             1.20444e-14         39.1                653.25              
    Skull                             0.948356            1.4862              638.108             -3.52086e-15        -3.52086e-15        741.25              
    UpperSpine                        0.197897            1.4862              133.156             -2.62074e-15        -53.9               680                 
    LegBone                           4.14748             1.4862              2790.66             -70                 0                   -390                
    Head                              5.00037             0.9869              5066.75             0                   0                   712.5               
    Trunk                             42.0142             0.970874            43274.6             0                   0                   315.5               
    Legs                              21.7346             0.9869              22023.1             -70                 0                   -390                
    Skin                              2.77696             0.9869              2813.82             0                   0                   315.5               
    Rib                               1.03078             1.4862              693.566             0                   0                   620                 
    MaleGenitalia                     0                   0.9869              0                   0                   50                  -14                 
    Teste                             0.0370863           0.9869              37.5786             14                  30                  -34                 
    World                             1.0648e-21          1e-25               1.0648e+07          0                   0                   0                   

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
    RefFilePath                      Results/ReferenceData.txt
    RefName                          MIRD
    GenerateRegionsVariableGraph     yes
    RegionVariableName               Mass
    GenerateRelativeSDevGraph        yes
    GenerateRelativeErrGraph         yes
    GenerateCrossSectionGraph        yes
    PositionDataFile                 EventsData/Pos_Liver_Volume_40000000.bin
    EnergyDataFile                   EventsData/Ene_Mono_1_40000000.bin
    MomDirDataFile                   EventsData/MomDir_Isotropic_40000000.bin
    EventsDataHistograms             yes
    UseLogE                          yes
    UseLogVariable                   no
    UseGridXY                        yes
    PrintTitle                       yes
    LegendPos                        RightTop
    LegendXWidth                     0.15
    LegendYHeight                    0.23
    GraphsExt                        .pdf
    MPISimulationNum                 m
    QuantitiesToScore                SAF      
    OrgansNamesToScoreString         Liver Kidney Adrenal 
    ExecutionMode                    MPI
    EventNumberForRankThread         40000000
    NumberOfRanks                    27

Using the command below which use as an input file the SimData generated in the calculation mode, the different graphs types were generated.  

 .. code-block:: bash

    $ ./analysis 

An example of the generated graphs is shown below 

.. list-table:: 

    * - .. figure:: /images/Cross_Result_SAF_Liver_gamma.png

           Results Cross Irradiation Graph

      - .. figure:: /images/Cross_ReferenceResult_SAF_gamma_Adrenal_Kidney_DoseCalcs_vs_MIRD.png

           Results and Reference Cross Irradiation Graph
           
    * - .. figure:: /images/Self_Result_SAF_gamma.png

           Results Self Absorption Graph 

      - .. figure:: /images/Self_ReferenceResult_SAF_gamma_Liver_DoseCalcs_vs_MIRD.png

           Results and Reference Self Irradiation Graph
           
    * - .. figure:: /images/RelativeSDv_SelfSAF.png

           Self Relative SDv Graph

      - .. figure:: /images/RelativeSDv_CrossSAF_Liver.png

           Cross Relative SDv Graph
           
    * - .. figure:: /images/RelativeError_Self_SAF_DoseCalcs_vs_MIRD.png

           Self Relative error Graph

      - .. figure:: /images/RelativeError_Cross_SAF_Liver_DoseCalcs_vs_MIRD.png

           Cross Relative error Graph
           
    * - .. figure:: /images/Mass_SAFForAllEnergies_inSelfAbsorption.png

           Mass SAF in Self Absorption Graph 

      - .. figure:: /images/Macroscopic_Cross_Section_for_gamma_in_material_SoftTissue.png

           Macroscopic Cross Section Graph
                  
Geometry from TEXT format file 
------------------------------

The simulation inputs are the same as for GDML phantom geometry, the difference is that the geometry was read from a TEXT format file called TEXTPhantomVol.geom. Where the command below should be changed to:  

.. code-block::
    :linenos:
    :emphasize-lines: 1

    #######################################################  Geometry ##########################
    /GeometryData/createVolume TEXT Scripts/TEXTPhantomVol.geom
    

Geometry from commands and STL files
------------------------------------

Input File
++++++++++

 .. code-block::
    :linenos:
    :emphasize-lines: 1,14,17,63,68,75,81

    #######################################################  Materials  ###########################
    /MaterialData/createElement 1 1.01 Hydrogen
    /MaterialData/createElement 6 12. Carbon
    /MaterialData/createElement 8 16.02 Oxygen

    /MaterialData/createMaterial CO2 0 2 1.03 g/cm3 nimb
    /MaterialData/addElements Carbon 1 Oxygen 2

    /MaterialData/createMaterial H2O 1 2 1.03 g/cm3 numb
    /MaterialData/addElements Hydrogen 2 Oxygen 1

    /MaterialData/setNistMaterialNameAndID G4_AIR 2

    #######################################################  world volume ########################
    /GeometryData/createWorld World G4_AIR yes 200. 200. 200. cm

    #######################################################  Volumes  ############################
    /GeometryData/createSolid STL solid1 Scripts/solid1.stl
    /GeometryData/createVolume STLVol1 solid1 G4_AIR World 80. 80. 80. 30. 10. 130. cm degree

    /GeometryData/createSolid STL solid2 Scripts/solid2.stl
    /GeometryData/createVolume STLVol2 solid2 H2O World 12. -34. -19. 0. 0. 0. cm degree

    /GeometryData/createSolid Box BoxSol 50. 40. 60. cm
    /GeometryData/createVolume BoxVol BoxSol CO2 World 80. 80. 80. 30. 10. 130. cm degree

    /GeometryData/createSolid Box BoxSol2 10. 20. 15 cm radian
    /GeometryData/createVolume BoxVol2 BoxSol2 CO2 BoxVol 0 0 0 99. 30. 34. cm degree

    /GeometryData/createSolid Tube tubeSol 10. 40. 50 3.14 3.14 cm radian
    /GeometryData/createVolume tubeVol tubeSol H2O World 99. 130. 111. 0. 0. 0. cm degree

    /GeometryData/createSolid Cone ConeSol 10. 40. 20. 60. 50 3.14 3.14 cm radian
    /GeometryData/createVolume ConeVol ConeSol H2O World 0. 8. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Para ParaSol 20. 40. 60. 3.14 3.14 3.14 cm radian
    /GeometryData/createVolume ParaVol ParaSol H2O World 0. 12. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Trd TrdSol 30. 10. 40. 15. 60. cm radian
    /GeometryData/createVolume TrdVol TrdSol H2O World 0. 16 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Sphere SphereSol 20. 40. 0. 180 0. 180 cm degree
    /GeometryData/createVolume SphereVol SphereSol H2O World 5. 0. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Orb OrbSol 10. cm radian
    /GeometryData/createVolume OrbVol OrbSol H2O World 120. 0. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Torus TorusSol 10. 20. 90. 0. 1.57 cm radian
    /GeometryData/createVolume TorusVol TorusSol H2O World 18 0. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Ellipsoid EllipsoidSol 10. 20. 50. cm radian
    /GeometryData/createVolume EllipsoidVol EllipsoidSol H2O World 24. 0. 0. 0. 0. 0. cm degree

    /GeometryData/createSolid Union UnionSol EllipsoidSol BoxSol 10. 0. 23. 0. 0. 0. cm radian
    /GeometryData/createVolume UnionVol UnionSol H2O World 0. 0. 20. 0. 0. 0. cm degree

    /GeometryData/createSolid Intersection IntersectionSol EllipsoidSol BoxSol 10. 0. 23. 0. 0. 0. cm radian
    /GeometryData/createVolume IntersectionVol IntersectionSol H2O World 0. 10. 20. 0. 0. 0. cm degree

    /GeometryData/createSolid Subtraction SubtractionSol EllipsoidSol BoxSol 10. 0. 23. 0. 0. 0. cm radian
    /GeometryData/createVolume SubtractionVol SubtractionSol H2O World 0. 20. 20. 0. 0. 0. cm degree

    #######################################################  Physics  ##########################
    /PhysicsData/setPhysicsData EMS2
    /PhysicsData/setCutsData 1. 1. mm keV
    /PhysicsData/generateCrossSectionFor gamma MeV 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1

    #######################################################  Source  ###########################
    /SourceData/setEventsParticleNameData gamma
    /SourceData/setEventsInitialPosData cm Volume STLVol1 2. 2. 2. BoxVol 1.2 1.2 1.2 EllipsoidVol 2.5 1.2 4.
    /SourceData/setEventsInitialEneData MeV Mono 1
    /SourceData/setEventsInitialMomDirData degree Isotropic
    # /SourceData/setSourceGenerationData 1000000 yes yes yes

    #######################################################  Run and Score  ####################
    /RunAndScoreData/setRegionsToScore all
    /RunAndScoreData/setQuantitiesToScore all
    /RunAndScoreData/setNumberOfThreads 1
    /RunAndScoreData/setSimNumOnRanks m

    #######################################################  Analysis  #########################
    /AnalysisData/generateSelfCrossGraphs Reference_Result Self_Cross MIRD /home/tarik/Desktop/WorkSpace/Projects/DoseCalcsCore/Results/ReferenceData.txt
    /AnalysisData/generateRelativeErrGraph
    /AnalysisData/generateRelativeSDevGraph
    /AnalysisData/generateVariableRegionGraph Mass
    /AnalysisData/generateEventsDataHisto
    /AnalysisData/setGraphsParameters yes no yes yes RightTop 0.15 0.23 .pdf

In addition to the solids constructed by commands, the mentionned STL files (solid1.stl , solid2.stl) should be present in the indicated path.

.. ICRP Voxelized Phantom 
.. ----------------------

.. DICOM Phantom
.. -------------







