DoseCalcs Commands
==================

To Build Materials
------------------

Four commands are dedicated to the materials building. The materials are created setting the elements and material data. A second way consists of using the NIST material database. 
In Fact, the material building is essential to construct world volume in STL and Geant4 predefined solids. Except TEXT and GDML geometry types that use all geometry and material data including world, from TEXT or GDML file.

 .. code-block::

    /MaterialData/createElement [Number Z] [Number A] [Element Name]

ex : /MaterialData/createElement 8 16.02 Oxygen

 .. code-block::

    /MaterialData/createMaterial [Material Name] [Material ID] [Number Of Elements] [Density] [Density Unit] [Element Accumulation by fraction (frac) or Number (Numb)]

ex : /MaterialData/createMaterial CO2 1 2 1.0 g/cm3 numb

 .. code-block::

    /MaterialData/AddElements [Element Name1]  [fraction(or number)] [Element Name2] [fraction(or number)] ...

ex : /MaterialData/AddElements Carbon 1 Oxygen 2

 .. code-block::

    /MaterialData/setNistMaterialNameAndID [NIST Material Name] [Material ID]

ex : /MaterialData/setNistMaterialNameAndID G4_AIR 1

To Construct Volumes
--------------------

The Geometry construction in DoseCalcs(except for GDML and TEXT formats) pass through construction of world volume, solids, and volumes. Materials should be created before. DoseCalcs groups these steps in three /GeometryData commands.

Creating World Volume
+++++++++++++++++++++

 .. code-block::

    /GeometryData/createWorld [World Volume Name] [Material Name] [is Sensitive Detector! (yes or no)] [X Y Z half size] [Length Unit]

ex : /GeometryData/createWorld World G4_AIR yes 200. 200. 200. cm

Creating Solids
+++++++++++++++

1. General Command :

 .. code-block::

    /GeometryData/createSolid [Solid Type] [Solid Name] [Solid Parameters] [Length Unit] [Angle Unit]

The next solid types require firstly, the World volume construction. which is done by /GeometryData/createWorld command.

2. Box

 .. code-block::

    /GeometryData/createSolid [Solid Type] [Solid Name] [X Y Z half size] [Length Unit] [Angle Unit]

3. Tube

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmin Rmax Dz SPhi DPhi] [Length Unit] [Angle Unit]

4. CutTube :

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmin Rmax Dz SPhi DPhi] [Length Unit] [Angle Unit]

5. Cone

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmin1 Rmax1 Rmin2 Rmax2 Dz SPhi DPhi] [Length Unit] [Angle Unit]

6. Para

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Dx Dy DZ Alpha Theta0 Phi0] [Length Unit] [Angle Unit]

7. Trd

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Dx1 Dx2 Dy1 Dy2 Dz] [Length Unit] [Angle Unit]

8. Sphere

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmin Rmax SPhi DPhi STheta DTheta] [Length Unit] [Angle Unit]

9. Orb

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmax] [Length Unit] [Angle Unit]

10. Torus

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Rmin Rmax Rtor SPhi DPhi] [Length Unit] [Angle Unit]

11. Ellipsoid

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [xSemiAxis ySemiAxis zSemiAxis] [Length Unit] [Angle Unit]

12. Union

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [First Solid Name] [Second Solid Name] [Solid Relative Translation] [Solid Relative Rotation] [Length Unit] [Angle Unit]

13. Intersection

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [First Solid Name] [Second Solid Name] [Solid Relative Translation] [Solid Relative Rotation] [Length Unit] [Angle Unit]

14. Subtraction

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [First Solid Name] [Second Solid Name] [Solid Relative Translation] [Solid Relative Rotation] [Length Unit] [Angle Unit]

For the last three types, the user needs two solids to create the third which can be union, or intersection or subtraction of the two solids. 

15. STL

 .. code-block::

     /GeometryData/createSolid [Solid Type] [Solid Name] [Solid File Path]

Creating Volumes
++++++++++++++++

1. General Command

 .. code-block::

    /GeometryData/createVolume [Volume Name Or Geometry Type] [Volume Solid Name] [Volume Material Name] [Volume Mother Name] [X Y Z Position] [X Y Z Rotation] [Length Unit] [Angle Unit]

This parameters must be set when the user create solids by /GeometryData/createSolid command. And the materials should be built. 

2. Geometry from GDML File

 .. code-block::

    /GeometryData/createVolume GDML [GDML Geometry File Path]

This command does not need the /GeometryData/createSolid or any built material

3. Geometry from TEXT File

 .. code-block::

    /GeometryData/createVolume TEXT [TEXT Geometry File Path]

This command does not need the /GeometryData/createSolid or any built material. The TEXT or GDML format description of a volume consists of the description of : shape, rotation, position, and material data. All these parameters are implemented in the TEXT or GDML format and one file must contain all volumes as well as the world volume, which means that DoseCalcs takes the entire geometry from this file, without need to construct world by command. Each created volume will be assigned to the sensitive detector to track the particles through it.

.. 4. Voxelized Geometry

.. .. code-block::

..    /GeometryData/createVolume VOXEL

.. This command does not need the /GeometryData/createSolid but uses the built material, and some additionnal commads as :


To Define Source
----------------

DoseCalcs source is constructed by five principal /SourceData/ commands that hold source parameters, such as events particle names, initial positions, initial energies, initial momentum directions and number of data to generate.


Generation Initial Positions
++++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setEventsInitialPosData [Length Unit] [Generate Type] [Parameters Case 1] [Parameters Case 2] ...

2. Volume Generate Type

 .. code-block::

    /SourceData/setEventsInitialPosData [Length Unit] Volume [VolumeName1 hx hy hz(surrends box half sizes)] [VolumeName1 hx hy hz(surrends box half sizes)] ...

ex : /SourceData/setEventsInitialPosData cm Volume Vol1 4 2 5 Vol2 6 6 10

The passed parameters such as region name, box half dimensions hx, hy and hz. The command takes the first parameter length unit, then "Volume" word, which indicates that the volumes where we want to generate data is a volume with non-uniform shapes, followed by source volume name and the corresponding hx, hy and hz. Additionally, to generate initial positions in more than one volume name, the command support more than one source volume data by adding the second source volume name followed by the corresponding hx, hy and hz, and so on... This makes initial positions generation or simulation in multiple sources easy and simple with one command.   

Generation Initial Energies
++++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setEventsInitialEneData [Energy Unit] [Energy Distribution] [Parameter1] [Parameter2] ...

2. Mono Distribution

 .. code-block::

    /SourceData/setEventsInitialEneData [Energy Unit] Mono [Mone Energy 1] [Mone Energy 2] ...

ex : /SourceData/setEventsInitialEneData MeV Mono 0.01 0.02 0.015 0.03 0.05 0.1 0.2 0.5 1

3. Gauss Distribution

 .. code-block::

    /SourceData/setEventsInitialEneData [Energy Unit] Gauss [Gauss SDev] [Gauss Mean 1] [Gauss Mean 2] ...

ex : /SourceData/setEventsInitialEneData MeV Gauss 0.01 1 2 3 4 5 10

4. Rayleigh Distribution

 .. code-block::

    /SourceData/setEventsInitialEneData [Energy Unit] Rayleigh [Rayleigh Max Energy 1] [Rayleigh Max Energy 2] ...

ex : /SourceData/setEventsInitialEneData MeV Rayleigh 0.5 0.7 0.6

5. Uniform Distribution

 .. code-block::

    /SourceData/setEventsInitialEneData [Energy Unit] Uniform [Min Energy] [Max Energy 1] [Max Energy 2] ...

ex : /SourceData/setEventsInitialEneData MeV Uniform 6 6.1 6.2 6.3

Generation Initial Momentum Directions(MomDir)
++++++++++++++++++++++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setEventsInitialMomDirData [Angle Unit] [MomDir Distribution] [Parameter1] [Parameter2] ...

This command takes as a first argument the angle unit, followed by distribution name and distribution corresponding parameters if exist.

2. Isotropic Distribution

 .. code-block::

    /SourceData/setEventsInitialMomDirData [Angle Unit] Isotropic

ex : /SourceData/setEventsInitialMomDirData degree Isotropic

3. Uniform Distribution

 .. code-block::

    /SourceData/setEventsInitialMomDirData [Angle Unit] Uniform

ex : /SourceData/setEventsInitialMomDirData degree Uniform

4. Directed Distribution

 .. code-block::

    /SourceData/setEventsInitialMomDirData [Angle Unit] Directed [Theta] [Phi]

ex : /SourceData/setEventsInitialMomDirData degree Directed 145 30

Setting Events Particle Names
+++++++++++++++++++++++++++++

 .. code-block::

    /SourceData/setEventsParticleNameData [Particle1] [Particle2] ...

ex : /SourceData/setEventsParticleNameData gamma e- e+

Setting Events Data Number, Activating Data Files Generation
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setSourceGenerationData [Events Data number] [Force Positions Generation] [Force Energies Generation] [Force MomDirs Generation]

ex : /SourceData/useDataGenerationFiles 10000 yes yes yes

The data files nomenclature used by DoseCalcs is based on four principal inputs such as data type, distribution name, a value related to this distribution and events number to generate. As a consequence, the file name is constructed in the same manner either for creation during data generation or for reading during simulation process. This nomenclature is used to know clearly which data file to use for the simulation, as well as avoiding the simulation of the same data with the same name. 

Setting this command means that the data generation is activated, then for generation run mode the data will be generated and saved to data files, and for calculation the events initial data will be read from the data files where the name of file is constructed by the same manner when its created. The direct events data simulation without needs to data files can be done just by unsetting this commands.

During simulation, each thread or rank read the data files and fills the position, energy and momentum arrays of size equal to the events number entrusted to this thread or rank. Then it reads the appropriate lines from the data files, in order to avoid the recurrence of an event's simulation with the same initial data. In consequence, it is recommended that the total number of simulated events by all threads must be equal or less than the number of lines in the data files. Note that each line in a data file corresponds to an event position, energy or momentum direction.

Source Data Visualization
++++++++++++++++++++++++++

1. To visualize the Box that surrend the source region

 .. code-block::

    /SourceData/showSourceBox

 .. image:: /images/BoxSurrendVolume.png

In the case of the initial positions generation, the box dimensions must be such as the box surrounds the source region as exactly as possible, in order to reduce the generation CPU time. Therefore, the user can use /SourceData/showSourceBox command to visualize the enclosing box in the entire geometry, then fine tuning of the box dimensions must be done before generation tasks. 

2. To Visualize generated initial positions

 .. code-block::

    /SourceData/testEventsInitialPositions

 .. image:: /images/BoxPointsTestVisualization.png

Moreover, if the user wants to visualize the initial positions, by setting the command /SourceData/testEventsInitialPositions and launching the simulation process. This command deactivates the transport process in all the body and only the starting source points are shown.

These two commands are useful to verify that the initial positions are well generated in the desired Region volume. 

To Define Physics
-----------------

Setting Physics
+++++++++++++++

1. General Command

 .. code-block::

    /PhysicsData/setPhysicsData [Physics Constructor] [Constructor Parameters]

2. Electromagnetic Constructors

 .. code-block::

    /PhysicsData/setPhysicsData [Electromagnetic Constructor]

ex : /PhysicsData/setPhysicsData EMS3

[Electromagnetic Constructor] parameter can be : EMS, EMO1, EMO2, EMO3, EMO4, Livermore, Penelope.

2. Construct Electromagnetic Physics

 .. code-block::

    /PhysicsData/setPhysicsData Construct [PhotoElectricEffect Model] [ComptonScattering Model] [GammaConversion Model] [RayleighScattering Model] [ElectronIonisation Model] [ElectronBrem Model] [HadronIonisation Model] [IonIonisation Model]

[PhotoElectricEffect Model] can be : 1 for G4PEEffectFluoModel, 2 for G4LivermorePhotoElectricModel, 3 for G4LivermorePolarizedPhotoElectricModel, 4 for G4PenelopePhotoElectricModel.

[ComptonScattering Model] can be : 1 for G4KleinNishinaCompton, 2 for G4KleinNishinaModel, 3 for G4LowEPComptonModel, 4 for G4LivermoreComptonModel, 5 for G4LivermoreComptonModifiedModel, 6 for G4LivermorePolarizedComptonModel, 7 for G4PenelopeComptonModel, 8 for G4TKleinNishinaCompton.

[GammaConversion Model] can be : 1 for G4BetheHeitlerModel, 2 for G4BetheHeitler5DModel, 3 for G4PairProductionRelModel, 4 for G4LivermoreGammaConversionModel, 5 for G4BoldyshevTripletModel, 6 for G4LivermoreNuclearGammaConversionModel, 7 for G4LivermorePolarizedGammaConversionModel, 8 for G4PenelopeGammaConversionModel.

[RayleighScattering Model] can be : 1 for G4LivermoreRayleighModel, 2 for G4LivermorePolarizedRayleighModel, 3 for G4PenelopeRayleighModel.

[ElectronIonisation Model] can be : 1 for G4MollerBhabhaModel, 2 for G4LivermoreIonisationModel.

[ElectronBrem Model] can be : 1 for G4SeltzerBergerModel, 2 for G4eBremsstrahlungRelModel, 3 for G4LivermoreBremsstrahlungModel, 4 for G4PenelopeBremsstrahlungModel.

[HadronIonisation Model] can be : 1 for G4BetheBlochModel, 2 for G4BetheBlochIonGasModel, 3 for G4BraggIonModel, 4 for G4BraggIonGasModel, 5 for G4IonParametrisedLossModel, 6 for G4AtimaEnergyLossModel, 7 for G4LindhardSorensenIonModel.

[IonIonisation Model] can be : 1 for G4BetheBlochModel, 2 for G4BraggModel, 3 for G4ICRU73QOModel.

ex : /PhysicsData/setPhysicsData Construct 1 2 1 2 1 1 1 1

Setting Cuts Data
+++++++++++++++++

The cuts are defined for electrons, positrons, gamma and protons. To determine the cuts values for the simulated particles. If the distance cut is not passed, the energy cut value is set to the default minimal energy. If both cuts are not passed, the default range cut value for electrons and photons is 1 mm, and according to material specification, it is converted to the energy threshold. In the current version of DoseCalcs code.

Below a threshold energy given by setting a cut in range or in kinetic energy, the secondary particles are simulated as continuous energy loss by the incident particle, this has no significant effect on the simulation results. Above this threshold, the secondary particles are explicitly generated and followed.

The performance of any Monte Carlo simulation will be poor if all the secondary particles are simulated and tracked. In order to reduce the simulation time, we use cut in range (distance cut ) or energy threshold (energy cut). In Geant4, the energy threshold can be 1 keV or larger.

 .. code-block::

    /PhysicsData/setCutsData [Cut in Range] [Energy Threshold] [Length Unit] [Energy Unit]

ex : /PhysicsData/setCutsData 0.1 1 mm keV

Generating Cross Section Data
+++++++++++++++++++++++++++++

 .. code-block::

    /PhysicsData/generateCrossSectionFor [Particle Name] [Energy Unit] [Energy1] [Energy2] [Energy3] ...

ex : /PhysicsData/generateCrossSectionFor gamma MeV 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1

Using this command, for each created material in the geometry, a table of macroscopic cross-sections will be created. It contains columns of all processes to be simulated for particle name and for all set energies, and written to the CrossSectionData file.

To Set Run and Score Parameters
-------------------------------

Setting Volumes To Score
++++++++++++++++++++++++

 .. code-block::

    /RunAndScoreData/setRegionsToScore [Volume1] [Volume2] [Volume3] ...

ex : /RunAndScoreData/setRegionsToScore LiverVol LungsVol SpleenVol PancreasVol

Setting Quantities To Score
+++++++++++++++++++++++++++

 .. code-block::

        /RunAndScoreData/setQuantitiesToScore [Quantity1] [Quantity2] [Quantity3] ...

ex : /RunAndScoreData/setQuantitiesToScore SAF AE S E

Note that the passed quantities can be : AE for absorbed energy, AF for absorbed fraction, SAF for specific absorbed fraction, AD for absorbed dose, S for S values, H for equivalent dose and E for effective dose

Setting Number Of Simulation Per Ranks
++++++++++++++++++++++++++++++++++++++

 .. code-block::

    /RunAndScoreData/setSimNumOnRanks [Simulation Number]
    
[Simulation Number] can be : one(o) , multi(m)

ex : /RunAndScoreData/setSimNumOnRanks o

Setting Number Of threads
+++++++++++++++++++++++++

 .. code-block::

    /RunAndScoreData/setNumberOfThreads [Number Of Threads]

ex : /RunAndScoreData/setNumberOfThreads 4

To Analysis with ROOT
---------------------

Generate Self and Cross Graphs
++++++++++++++++++++++++++++++

 .. code-block::

    /AnalysisData/generateSelfCrossGraphs [Graphs Data] [Compare Type] [Reference Name] [Reference File Path]

ex : /AnalysisData/generateSelfCrossGraphs Reference_Result Self_Cross .pdf MIRD /home/User/DoseCalcs/Results/ReferenceData.txt

[Graphs Data] can be : Result , Reference , Reference_Result.

[Compare Type] can be : Self, Cross, Self_Cross .

Generate Relative Error Graph
++++++++++++++++++++++++++++++

 .. code-block::

    /AnalysisData/generateRelativeErrGraph

Generate Relative Standard Deviation Graph
++++++++++++++++++++++++++++++++++++++++++

 .. code-block::

    /AnalysisData/generateRelativeSDevGraph

Generate Variable-Region Graph
++++++++++++++++++++++++++++++

In the context of data analysis for internal dosimetry purpose, the user can generate a graph of a quantity i.e. SAF, in function of either volume, mass, density, and distance between the source and target, by using /AnalysisData/generateVariableRegionGraph command, which takes the variable name (i.e. Mass, Volume, Density and Distance) as an argument. This will show clearly how a dosimetry quantity value changes between targets and sources, and give insight about the parameter that affects this scored quantity.

 .. code-block::

    /AnalysisData/generateVariableRegionGraph [Parameter Name]

ex : /AnalysisData/generateVariableRegionGraph Volume

[Parameter Name] can be : Volume, Mass, Density, Distance.

Generate Events Data Histograms
+++++++++++++++++++++++++++++++

 .. code-block::

    /AnalysisData/generateEventsDataHisto

Setting Graphs Parameters
+++++++++++++++++++++++++

 .. code-block::

    /AnalysisData/setGraphsParameters [Use Log for E axis] [Use Log for Variable axis] [Use Grid XY] [Print Title] [Legend Position] [Legend X Width] [Legend Y Height] [Graphs Extension]

[Use Log for E axis] can be : yes, no.

[Use Log for Variable axis] can be : yes, no.

[Use Grid XY] can be : yes, no.

[Print Title] can be : yes, no.

[Legend Position] can be : RightBottom, LeftBottom, RightTop, LeftTop, MiddleBottom, MiddleTop .

[Legend X Width] : double Value

[Legend Y Height] : double Value

[Graphs Extension] can be : .root , .pdf , .ps , .png , .jpeg .

ex : /AnalysisData/setGraphsParameters yes no yes yes RightTop 0.15 0.23 .pdf



