AOSPE Source
=============

Getting Started
---------------
To get started with the AOSPE sources, you'll need to get
familiar with [Git and Repo](http://source.android.com/source/version-control.html).

Setup Build Environment - http://forum.xda-developers.com/showthread.php?t=2464683


Create the Directories
----------------------

You will need to set up some directories in your build environment.

To create them run:

    mkdir -p ~/bin
    mkdir -p ~/aospe
    
    
Install the Repository
----------------------

Enter the following to download the "repo" binary and make it executable:

curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo

You may need to reboot for these changes to take effect. 
Now enter the following to initialize the repository:

    cd ~/aospe
    

Repositories:
---------------
To initialize your local repository using the AOSPE trees, use a command like this:

    repo init -u git://github.com/DracoROM/android.git -b kitkat && repo sync
  
  
Building the System
---------------
Initialize the environment with the envsetup.sh script. Note that replacing "source" with a single dot saves a few characters, and the short form is more commonly used in documentation.

    . build/envsetup.sh
    lunch
    
Enter the number of the build you want to start and press enter

Build the Code:
  make -j4
  
To make a flashable zip, use:
  ./rom-build.sh <Device>
