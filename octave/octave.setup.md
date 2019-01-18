# Set Up Octave

### Install Octave
* Reference: http://wiki.octave.org/Octave_for_Microsoft_Windows
* Download octave from: https://www.gnu.org/software/octave/download.html
* Pick up the 7z format
* Run octave by running the octave.vbs file

### Packages

* you need first to rebuild the package list on your local machine.
pkg rebuild
* you can confirm the package list by typing the command below at the Octave command prompt:
pkg list
* Packages can be updated by running
pkg update
* Other packages can be installed by running
pkg install -forge <package name>