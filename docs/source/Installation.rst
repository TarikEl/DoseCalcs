Installations
=============


Package Requirements
--------------------

 * build-essential 
 
 .. code-block:: bash
   
   $ sudo apt-get install build-essential
 
 * cmake
 
 .. code-block:: bash
 
   $ wget http://www.cmake.org/files/v3.5/cmake-3.5.0-rc2.tar.gz
   $ tar xf cmake-3.5.0-rc2.tar.gz
   $ cd cmake-3.5.0-rc2
   $ ./configure
   $ make
   $ sudo make install
 
 * X11 Xmu library and/or headers
 
 .. code-block:: bash

   $ sudo apt-get install libxaw7-dev libxaw7
   
   
 * Qt User Interface and Visualization
 
 .. code-block:: bash
  
   $ sudo apt-get install qt5-default
 
 * MesaGL headers and libraries
 
 .. code-block:: bash

   $ sudo apt-get install mesa-common-dev
   $ sudo apt-get install libglu1-mesa-dev -y 
 
 * Xerces-C++ headers and libraries
 
 .. code-block:: bash

   $ wget http://www.eu.apache.org/dist//xerces/c/3/sources/xerces-c-3.1.2.tar.gz
   $ tar xf xerces-c-3.1.2.tar.gz
   $ cd xerces-c-3.1.2
   $ ./configure
   $ make -j4
   $ make install
   
Or by: 

 .. code-block:: bash
 
   $ cmake -DCMAKE_INSTALL_PREFIX=/home/User/xercesc_install  /home/User/xerces-c-3.2.0
 
 * Geant4
 
Download geant4 from:  http://geant4.web.cern.ch/geant4/support/download.html

Unpack the Geant4 source package geant4.XX.XX.tar.gz to a location of your choice. For illustration only, this guide will assume it's been unpacked in a directory named for example at /home/User/Desktop/geant4 , so that the Geant4 source package sits in a subdirectory . (geant4.XX.XX means the latest Release of Geant4). the unpacked source now is in : /home/User/Desktop/geant4/geant4.XX.XX

 .. code-block:: bash

   $ cd /home/User/Desktop/geant4
   $ mkdir geant4_build
   $ mkdir geant4_install
   $ cd /geant4_build
   $ cmake -DCMAKE_INSTALL_PREFIX=/home/User/Desktop/geant4/geant4_install -DGEANT4_BUILD_MULTITHREADED=ON -DGEANT4_USE_QT=ON -DGEANT4_USE_OPENGL_X11=ON -DGEANT4_INSTALL_DATA=ON -DGEANT4_USE_GDML=ON -DXERCESC_ROOT_DIR="/home/..../xerces-c-3.2.1" /home/tarik/Desktop/geant4/geant4.XX.XX

(if fails, rm -rf * in /geant4_build before rerunning cmake). cmake -D and Tab, will show you all the cmake variables possibilities to build geant4 as you need.
   
 .. code-block:: bash
  
   $ make -j4
   $ make install

-DXERCESC_ROOT_DIR=<path_to_xercesc> : pointing to the path where the XercesC XML parser package is installed in your system.
 
 * OpenMPI1.8.1
 
 .. code-block:: bash

   $ mpigcc   #it will show you the 3 MPI libraries, choose one to install. in our case, we choose OpenMPI1.8.1 by :
   $ sudo apt-get install libopenmpi-dev
   $ or by : 
   $ downloading the OpenMPI1.8.1 package, unpack it under /home/User/Desktop/openmpi1.8.1
   $ mkdir  /home/User/Desktop/openmpi1.8.1/install
   $ cd /home/User/Desktop/openmpi1.8.1
   $ ./configure --prefix= /home/User/Desktop/openmpi1.8.1/install 
   $ make all install
 
The related variables used in DoseCalcs building should be set to :  
-DWITH_G4MPI_USE=ON, 
-DCMAKE_CXX_COMPILER=/home/User/Desktop/openmpi1.8.1/install/bin/mpicxx,
-DCMAKE_C_COMPILER=home/User/Desktop/openmpi1.8.1/install/bin/mpicc.

 * ROOT Analysis System

Download Root from: https://root.cern/downloading-root 

Unpack the Root source package root-X.XX.XX.tar.gz to a location of your choice. For illustration only, this guide will assume it's been unpacked in /home/User/Desktop , so that the root source package sits in a subdirectory . (/root-X.XX.XX , X is the version). the unpacked source now is in : /home/User/Desktop/root-X.XX.XX
 
 .. code-block:: bash

   $ mkdir root_build
   $ cd root_build
   $ cmake /home/User/Desktop/root-X.XX.XX
   $ make -j4 
   
   Setup and run ROOT
   
 .. code-block:: bash
    
    $ source /home/User/Desktop/root_build/bin/thisroot.sh
    $ root
    
.. * DCMTK
 
.. Unpack the DCMTK source package dcmtk.tar.gz to a location of your choice. For illustration only, this guide will assume it's been unpacked in /home/User/Desktop , so that the root source package sits in a subdirectory. the unpacked source now is in : /home/User/Desktop/DCMTK
 
.. .. code-block:: bash

..   $ cd /home/User/Desktop/DCMTK
..   $ mkdir build
..   $ mkdir install
..   $ cd build
..   $ cmake -DCMAKE_INSTALL_PREFIX= /home/User/Desktop/DCMTK/install /home/User/Desktop/DCMTK
..   $ make -j4
..   $ make install

.. The related variable used in DoseCalcs building should be set to : -DDCMTK_DIR=/home/User/Desktop/DCMTK/install/usr/local/lib/cmake/dcmtk


DoseCalcs building
------------------

 .. image:: /images/SourceDir.png

 .. image:: /images/DoseCalcsBuilding.png
 
 .. image:: /images/BuildDir.png
  
1. Building Command

Considering that the application source directory is in /home/User/Desktop/DoseCalcs . in terminal:

 .. code-block:: bash

   $ cd /home/User/Desktop
   $ mkdir DoseCalcs_build
   $ cd /DoseCalcs_build
   $ cmake -DCMAKE_BUILD_TYPE=Debug  -DGeant4_DIR=/home/User/Desktop/geant4/geant4_install/lib/Geant4-XX.X.X -DWITH_GEANT4_UIVIS=ON -DWITH_GDML_USE=ON­ -DWITH_MT_USE=ON  -DWITH_ANALYSIS_USE=ON  -DROOT_DIR=/usr/local  -DWITH_G4MPI_USE=OFF  -DCMAKE_CXX_COMPILER=/home/User/Desktop/openmpi1.8.1/install/bin/mpicxx  /home/User/Desktop/DoseCalcs
   $ make -j4
   $ make install

..  -DWITH_VOX_USE=ON  -DWITH_DCMTK_USE=ON  -DDCMTK_DIR=/home/User/Desktop/DCMTK/install/usr/local/lib/cmake/dcmtk  

2. Building Variables

-DWITH_GDML_USE : Use GDML geometry in the simulation

.. -DWITH_VOX_USE : Use Voxelized geometry (Voxel, VoxIDs, DICOM) in the simulation

.. -DWITH_DCMTK_USE : Used with DICOM voxelized geometry, this allow to read DICOM files using DCMTK Packages

.. -DDCMTK_DIR : Used If we set -DWITH_VOX_USE and -DWITH_DCMTK_USE to ON, the value of this variable is the installation path of DCMTK

-DWITH_ANALYSIS_USE : Used If we want to Generate Graphs using ROOT ANALYSIS SYSTEM, cmake generate an executable called [analysis]

-DWITH_G4MPI_USE : Used in MPI DoseCalcs running, to run the application on cluster containing Multiple Computers, each computer has a number of cores. cmake generate an executable called [merge], this used to merge the result of a MPI simulations in cluster nodes, and create a master result on master node.

The use of ROOT and G4MPI requires the installation path of these libraries to be given with -DROOT_DIR and -DMPI_DIR variables while building the code. 

-DROOT_DIR : Used if we set -DWITH_ANALYSIS_USE to ON , the value of this variable is the installation path of ROOT

-DCMAKE_C_COMPILER : Used if we set -DWITH_G4MPI_USE to ON, the value of this variable is the path of mpicc

-DCMAKE_CXX_COMPILER : Used if we set -DWITH_G4MPI_USE to ON, the value of this variable is the path of mpicxx

..  The variables -DWITH_DCMTK_USE, -DWITH_G4MPI_USE and -DWITH_ANALYSIS_USE are needed if the correspondent libraries to be used.
    
..  If -DWITH_DCMTK_USE and -DWITH_ANALYSIS_USE are set to ON, we need to set the correspondent -DDCMTK_DIR, -DROOT_DIR. 

If -DWITH_ANALYSIS_USE is set OFF, or it doesn’t present in the cmake command, the setting of -DROOT_DIR is unneeded. 

.. The same for -DWITH_DCMTK_USE.

3. Building Example

 .. code-block:: bash
  
   $ cmake -DCMAKE_BUILD_TYPE=Debug  -DWITH_GDML_USE=ON  -DWITH_ANALYSIS_USE=ON  -DROOT_DIR=/usr/local/lib  -DWITH_MT_USE=OFF  -DWITH_G4MPI_USE=ON  -DCMAKE_CXX_COMPILER=$OpenMPIInstallDir/bin/mpicxx  -DCMAKE_C_COMPILER=$OpenMPIInstallDir/bin/mpicc $ApplicationSourcePath 

.. -DWITH_VOX_USE=ON -DWITH_DCMTK_USE=ON  -DDCMTK_DIR=/home/tarik/Desktop/WorkSpace/geant4/dcmtk-3.6.5/install/usr/local/lib/cmake/dcmtk 

After DoseCalcs building, the three directories EventsData, Scripts and Results in the main application directory structure shown in figure \ref{SrcDir}, are copied to the build directory. Where the scripts directory contains the commands and geometry macros, TissueRadiationFactors.mac file is used in calculation of the equivalent dose (H) and the effective dose (E). The directory EventsData, will contain the generated data files. Finally, Results directory will contain all the results of the simulation, text files and ROOT generated graphs and histograms. Besides these directories, the simulate.cc file serves to generate [simulate] executable file, the same as analysis.cc and merge.cc, that serve to generate [analysis] and [merge] executable files respectively as shown in the figure \ref{BuildDir}. For the visualization purposes macros commands file, openGLVis.mac is called when the user run [simulate] executable in graphical mode.




