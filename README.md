# AV Survivability fork of Veins – Release 5.2 
Veins - The open source vehicular network simulation framework.

See the Veins website <http://veins.car2x.org/> for a tutorial, documentation,
and publications.

Veins is composed of many parts. See the version control log for a full list of
contributors and modifications. Each part is protected by its own, individual
copyright(s), but can be redistributed and/or modified under an open source
license. License terms are available at the top of each file. Parts that do not
explicitly include license text shall be assumed to be governed by the "GNU
General Public License" as published by the Free Software Foundation -- either
version 2 of the License, or (at your option) any later version
(SPDX-License-Identifier: GPL-2.0-or-later). Parts that are not source code and
do not include license text shall be assumed to allow the Creative Commons
"Attribution-ShareAlike 4.0 International License" as an additional option
(SPDX-License-Identifier: GPL-2.0-or-later OR CC-BY-SA-4.0). Full license texts
are available with the source distribution.
 
 
 ## Overview
 
 1 - Installation Process: Instructions taken from the official Veins site `https://veins.car2x.org/tutorial/` to install Veins + related software (Sumo, OMNeT++). With slight modifications/acknowledgement of potential errors. 
 
 2 - Launching Veins: How to run a scenario in Veins, and how to launch Veins after the initial installation/launch process.
 
 3 - Potential GCC Isssue: Build issue resolution for problem with incorrect GCC installation.
 
 4 - Definitions: Some definitions for the many file types and explanations on some of the terminology used in the Veins project.  
 
 5 - Repo Structure
 
 6 - Useful Links
 
 
## 1 - Installation Process 
Full installation instructions can be found here but assumes the user is on Windows 7: https://veins.car2x.org/tutorial/. These instructions assume Ubuntu 22.04.1

Veins also requires:
-	SUMO 1.8.0
-	OMNeT++ 6.0 
These versions were chosen for our specific setup, see the website for all compatible versions of SUMO and OMNeT++. 

### Prerequisites
For building on Ubuntu, the following packages may be required.
Run: 
`sudo apt-get install build-essential gcc g++ bison flex perl tcl-dev tk-dev blt libxml2-dev zlib1g-dev default-jre doxygen graphviz libwebkitgtk-1.0-0 openmpi-bin libopenmpi-dev libpcap-dev autoconf automake libtool libproj-dev libgdal1-dev libfox-1.6-dev libgdal-dev libxerces-c-dev qt4-dev-tools`


#### Libwebkitgtk-1.0-0 Issue
If Libwebkitgtk-1.0-0 causes an error, do the following: 
- Add the following to `/etc/apt/sources.list`: 
  - `deb [trusted=yes] http://mirrors.kernel.org/ubuntu bionic main universe`
- Run `sudo apt-get update`
- Run `sudo apt-get install libwebkitgtk-1.0-0` 

#### Libgdal1-dev Issue
If Libgdal1-dev causes an error, it can be ignored. 

### Install SUMO
Download the SUMO 1.8.0 binaries at: https://sourceforge.net/projects/sumo/files/sumo/version%201.8.0/

Unpack in desired location as: `<path_to_location>\sumo-1.8.0`

You should find the executable: `<path_to_location>\sumo-1.8.0\bin\sumo.exe`

Build SUMO following these instructions: https://sumo.dlr.de/docs/Installing/index.html

### Install OMNeT++
Download the Linux distribution here: https://omnetpp.org/download/old
Unpack to: `<path_to_location>/omnetpp-6.0`

*Make sure the folder name contains no spaces* 

To build, run the following: 
-	`./configure`
-	`make`

You should find: `<path_to_location>/omnetpp-6.0/bin/omnetpp`

Run: 
-	`source setenv `
-	`omnetpp`

to launch the OMNeT++ IDE. Choose `<path_to_location>\omnetpp-6.0\samples\<optional_sub_folder>` as your workspace when prompted 

### Install Veins
Download Veins 5.2: https://veins.car2x.org/download/

Unpack it to: `<path_to_location>\veins-5.2`

In the OMNeT++ IDE, import the Veins project as follows:
-	Click `File > Import > General: Existing Projects into Workspace`
-	Select the `veins-5.2` directory 
-	Select `Project > Build All` to build

## 2 - Launching Veins

Run a scenario under `examples` (eg; *SingleAttackerV2VScenario.ned*) by right-clicking on a .ned file and selecting `Run As > OMNeT++ Simulation`. This will launch a Qtenv window. Click the green play button in the top left corner to run the scenario. After the initial setup, the following steps can be used each time Veins/OMNeT++ needs to be launched. 

Run: `export PATH=$PATH:<path_to_sumo>/sumo-1.8.0/bin`
-	This can be permanently set in the .profile 

Run: `<path_to_veins>/veins-5.2/bin/veins_launchd -vv -c <path_to_sumo>/sumo-1.8.0/bin/sumo.exe`

In the OMNeT++ IDE, right-click: `veins-5.2/examples/veins/omnetpp.ini`

And `Run As > OMNeT++ simulation`. 

**Note: If the Veins project is hiding, click `Window > Show View > Project Explorer`. You should see a veins folder and find the .ini file.** 

## 3 - Potential GCC Version Issue
At some point, our enviornment decided to start using GCC version 12. Veins uses version 11 and would generate errors such as `<memory> file not found` when attempting to build the Veins project in OMNeT. To chech which version of GCC is being used, run the following: 

`dpkg -l | grep -e cpp -e c++ -e clang; clang -v; which clang`

Which will output (among other things) the following line:

`Found candidate GCC installation: /usr/bin../lib/gcc/x86_64-linux-gnu/<version>`

If the GCC installation is not the correct version, either uninstall the selected version, or navigate to `/usr/bin/../lib/gcc/x86_64-linux-gnu` and rename the current version so that it's forced to pick up the desired GCC installation. 

## 4 - Definitions

### .NED Files
Network definition file, created by OmNeT. Stores logical network structure and information on how classes are tied together. Is accompanied by a C++ source file (.cc, .cpp and headers [.h]). You will need to create both a **NED file** and a **C++ behavior file** to simulate a network. NED files can be edited graphically (in a drag-and-drop manner) or from Source (text editing) in the OMNeT++ IDE. 
New scenarios can generally inherit from the base `Scenario.ned` file. 

### Scenarios
Scenarios are basically represented by .NED files. They can be extensions of the standard VEINS scenario (./src/veins/nodes/Scenario.ned), but users are not required to use `Scenario.ned` as a base. 

### .ini Files
Define network and simulation parameters. An .ini file can contain many networks, so this is where you define which networks to use and what parameters to pass to each one.

### Qtenv
Main OmNeT++ runtime simulation GUI

### erlangen.rou.xml
This generates the different types of traffic that SUMO + TraCI spawn/manage.

## 5 - Repo Structure
### Scenarios: 
To view the code for scenarios created for this project, navigate to `examples/veins`. Each scenario is in its own folder with its own README. 

### Veins source code:
Navigate to `src` to view Veins code. 


## 6 - Useful Links
In no particular order, links that were useful resources of information at some point.

**General VEINS Usage Notes** - https://github.com/namnatulco/veins/blob/ldm/usagenotes.md
Good resource for understanding file structure in VEINS and file types 

**OMNeT++ Documentation** - https://omnetpp.org/documentation/
Recommended by VEINS developers to read if creating new scenarios

**User Guide** - https://doc.omnetpp.org/omnetpp/UserGuide.pdf

**Tic Toc Tutorial for OMNeT** - https://docs.omnetpp.org/tutorials/tictoc/part1/
Good starting place



