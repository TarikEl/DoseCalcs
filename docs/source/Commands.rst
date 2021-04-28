DoseCalcs Commands
==================

To Build Materials
------------------

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

/GeometryData/createWorld command must be used firstly.

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

This command does not need the /GeometryData/createSolid or any built material

.. 4. Voxelized Geometry

.. .. code-block::

..    /GeometryData/createVolume VOXEL

.. This command does not need the /GeometryData/createSolid but uses the built material, and some additionnal commads as :


To Define Source
----------------

Generation Initial Positions
++++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setEventsInitialPosData [Length Unit] [Generate Type] [Parameters Case 1] [Parameters Case 2] ...

2. Volume Generate Type

 .. code-block::

    /SourceData/setEventsInitialPosData [Length Unit] Volume [VolumeName1 hx hy hz(surrends box half sizes)] [VolumeName1 hx hy hz(surrends box half sizes)] ...

ex : /SourceData/setEventsInitialPosData cm Volume Vol1 4 2 5 Vol2 6 6 10

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

Setting Events Data Number
++++++++++++++++++++++++++

1. General Command

 .. code-block::

    /SourceData/setSourceGenerationData [Events Data number] [Force Positions Generation] [Force Energies Generation] [Force MomDirs Generation]

ex : /SourceData/useDataGenerationFiles 10000 yes yes yes

Source Data Visualization
++++++++++++++++++++++++++

1. To visualize the Box that surrend the source region

 .. code-block::

    /SourceData/showSourceBox

 .. image:: /images/BoxSurrendVolume.png

2. To Visualize generated initial positions

 .. code-block::

    /SourceData/testEventsInitialPositions

 .. image:: /images/BoxPointsTestVisualization.png

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

 .. code-block::

    /PhysicsData/setCutsData [Cut in Range] [Energy Threshold] [Length Unit] [Energy Unit]

ex : /PhysicsData/setCutsData 0.1 1 mm keV

Generating Cross Section Data
+++++++++++++++++++++++++++++

 .. code-block::

    /PhysicsData/generateCrossSectionFor [Particle Name] [Energy Unit] [Energy1] [Energy2] [Energy3] ...

ex : /PhysicsData/generateCrossSectionFor gamma MeV 0.01 0.015 0.02 0.03 0.05 0.1 0.2 0.5 1

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

    /AnalysisData/setGraphsParameters [Use Log for E axis] [Use Log for Variable axis] [Use Grid XY] [Print Title] [Legend Position] [Graphs Extension]

[Use Log for E axis] can be : yes, no.

[Use Log for Variable axis] can be : yes, no.

[Use Grid XY] can be : yes, no.

[Print Title] can be : yes, no.

[Legend Position] can be : RightBottom, LeftBottom, RightTop, LeftTop, MiddleBottom, MiddleTop .

[Graphs Extension] can be : .root , .pdf , .ps , .png , .jpeg .

ex : /AnalysisData/setGraphsParameters yes no yes yes RightTop .pdf



