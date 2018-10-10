# Atlantis model configuration

<hr>
<p align="center">
    <img alt="Atlantis" src="./img/logo.jpg" height="200" width="200">
</p>
<hr>
___________
This page contains the basic instructions for running an Atlantis model on your
computer. For more detailed explanation you should visit the [Wiki](https://confluence.csiro.au/display/Atlantis/Atlantis+Ecosystem+Model+Home+Page)

 **Attention!**
>If you want a copy of the Atlantis main code (which you will need to start using the
model) you will need to go first to this page [Licence](https://confluence.csiro.au/display/Atlantis/CSIRO+licence+and+repository+request) and send an email to the
developers, who will give you access to the code.
__________
<hr>

Atlantis model installation and running
===============
### Index
* [Preparing your machine](#preparing-your-machine)
   * [Linux](#linux)
   * [Windows](#windows)
   * [MacOS](#macos)
* [Check out the code](#Check-out-the-code)
* [Building Atlantis](#Building Atlantis)
   * [Linux](#linux)
   * [Windows](#windows)
   * [MacOS](#macos)
* [Input files](#input-files)
* [Output Files](#output-files)
* [Supporting software](#Supporting-software)


<hr>

# Preparing your machine
---
## Linux

####  Checking libraries and packages

```
$ dpkg -l | grep build-essential	# Essential packages to build Debian
$ dpkg -l | grep autoconf      	    # Automatic configure script builder
$ dpkg -l | grep subversion      	# Version Control System (like GITHUB)
$ dpkg -l | grep gawk          	    # GNU version de Awk
$ dpkg -l | grep proj           	# Program Proj.4 Cartographic projection
$ dpkg -l | grep libxml2-dev 	    # Library for XML language
$ dpkg -l | grep libnetcdf-dev	    # Library of development kit for NetCDF
$ dpkg -l | grep flip              	# convert text file line endings between Unix and DOS
```

#### Installing libraries and packages
In the case that you do not have any of the libraries or packages of the previous
point installed, this is the way to install them on your machine.

```
$ sudo apt-get install build-essential
$ sudo apt-get install autoconf
$ sudo apt-get install subversion
$ sudo apt-get install libxml2-dev
$ sudo apt-get install libnetcdf-dev
$ sudo apt-get install gawk
install proj.4 following the instruction from developers [webpage](https://proj4.org/)
```
<hr>


---
## Windows



---
## MacOS

# Checking out the Atlantis code.

The source code can be accessed through the following URL's:
 * CSIRO users: https://svnserv.csiro.au/svn/atlantis/Atlantis/trunk/atlantis
 * External Partners: https://svnserv.csiro.au/svn/ext/atlantis/Atlantis/trunk/atlantis
If you need information on how to check out the code from SVN have a look at this
tutorial - http://vegastrike.sourceforge.net/wiki/HowTo:Checkout_SVN.

*Example*
```
svn co https://svnserv.csiro.au/svn/ext/atlantis/Atlantis/trunk/atlantis
```
> Remember that if you want a copy of the Atlantis main code you will need to go
first to this page
[Licence](https://confluence.csiro.au/display/Atlantis/CSIRO+licence+and+repository+request)
and send an email to the developers, who will give you access to the code.

# Building Atlantis
## Linux
To build Atlantis under LinuxI need t compile and run using aototools

```
$ cd  /to/your/atlantis/trunk/folder/
$ aclocal
$ autoconf
$ automake -a               # I have some problems with this section
$ autoreconf  -fvi          # you can use this instead of the other ones
$ sudo chmod +x configure   # Change the permissions to the configure script
$ ./configure
$ make
$ sudo make install
```

## Windows
## MacOs
# Input files

# Output files
## NetCDF output Files
Atlantis generates a number of netCDF output files that contain spatial information
such as the biomass of functional groups in boxes and layers.  The netCDF files do
not follow the normal gridded netCDF structure that people are used to as the
Atlantis model is a box model, not a grid model. The normal packages such as
[ncview](http://meteora.ucsd.edu/~pierce/ncview_home_page.html) will not work with
Atlantis output netCDF files. To read these outputs you can use your own code or one
of the tools developed for these type of outputs [Tools](Supporting-software).
*  **biol.nc**
This 3D (time, box, layer) contains snapshots of the tracers in the model at given time frequencies in each box and layer.
*  **biolTOT.nc**
This 2D (time, box) output file contains a sum of the tracer values in each box.
*  **biolPROD.nc**
This output file is useful during the model tuning process and contains 2D data (time, box) for each box. It contains the tracers for production and grazing for invertebrate groups, growth and consumption for each age class for each vertebrate group, and a number of indices such as the diversity index.
*  **CATCH.nc**
This output file contains cumulative values of:
   * Catch per species per age class (cohort) in numbers
   * Discards per species per age class (cohort) in numbers
   * Catch per species per fishery (in tonnes) - that is the total tonnes taken from that box
   * Discards per species per fishery (in tonnes) - that is the total tonnes taken from that box
> To convert the numbers to biomass you have to multiple by individual size-at-age
(form biol.nc) and then * X_CN * mg_2_tonne where X_CN is the Redfield ratio
specified in the biol.prm file (typically 5.7) and mg_2_tonne is 0.00000002
*  **TOTCATCH.nc**
This output files also contains cumulative values in tonnes. All values are zeroed after they are written out. Tracers include:
   * Total catch per species
   * Total recreational catch per species
   * Total discards per species.
*  **ANNAGEBIO.nc**
This output provides abundance in each annual age class (Numbers at age per species)
> so mapped from Atlantis "age class" which can contain multiple years to true annual
age classes).

*  **ANNAGECATCH.nc**
This output provides numbers at annual age class in the catch and discards (summed
over all fleets). Tracers provided are:
   * Numbers at age per species in the catch
   * Numbers at age per species in the discards

## Plain text files
these files can contains a simplified or aggregate version of the information
contained in the NetCDF Files or and specific output.
#### Biologically relevant output:
*  **BiomIndx.txt**
Biomass in tonnes of each species across the entire model domain
*  **DietCheck.txt**
Indication of diet pressure
*  **DetailedDietCheck.txt**
This file is only produced if flagdietcheck is set to 1. The file produced provides the total biomass consumed of each prey species per age class of each predator species per box and layer. 
*  **YOY.txt**
This is the biomass in tonnes per spawning event summed over the total model domain.
#### Catch and fisheries relevant output
*  **BrokenStick.txt**
The output file that has been created to debug the broken stick management strategy.
*  **Catch.txt**
The total landings per species (in tonnes) across the entire model domain (summed over fisheries)
*  **CatchPerFishery.txt**
The catch of each species per fishery (in tonnes) across the entire model domain (summed over fisheries)
*  **Discards.txt**
The total discards per species across the entire model domain (summed over fisheries)
*  **DiscardsPerFishery.txt**
The discards of each species per fishery across the entire model domain (summed over fisheries)

<hr>
