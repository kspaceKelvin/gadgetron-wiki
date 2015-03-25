RHEL 6 Derivatives
==================

Installation instruction for Red Hat Enterprise Linux and its derivatives (CentOS, Scientific Linux, etc).  These notes have been tested on a clean install of CentOS 6.3 with the "Software Development Workstation" package option.

CUDA, CULA and GPU Driver (Optional)
------------------------------------
**The steps below must be executed as a super user.**

* Install CUDA (5.5) toolkit and SDK
* Install CULA
* Install nvidia GPU Driver

Enabling some none-default repositories
---------------------------------------
There are a few dependencies not in the default repositories, so we add the EPEL, ACE, and Boost repositories.

**The steps below must be executed as a super user.**

* Add EPEL to the list of know repositories:
  - `wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`
  - `yum install epel-release-6-8.noarch.rpm`
* Add the ACE development repository from http://download.opensuse.org/repositories/devel:/libraries:/ACE:/minor/, e.g.
  - `cd /etc/yum.repos.d/`
  - `wget http://download.opensuse.org/repositories/devel:/libraries:/ACE:/minor/CentOS_CentOS-6/devel:libraries:ACE:minor.repo`
* Add the Boost >=1.55 repo:
  - `wget http://repo.enetres.net/enetres.repo -O /etc/yum.repos.d/enetres.repo`
  - `yum install boost-devel`

Gadgetron dependencies
----------------------
**The steps below must be executed as a super user.**

* `yum install qt-devel fftw-devel freeglut-devel hdf5-devel glew-devel lapack-devel xerces-c-devel xsd`
* `yum install docbook-utils-pdf docbook5-schemas docbook5-style-xsl`
* `yum install cmake28`
* `yum install ace-devel`
* `yum install boost-devel doxygen git libxml2-devel libxslt-devel openblas-devel armadillo-devel gtest`

  Note: Libraries below can be updated from third party repos:
  
  - `fftw-devel 3.2.2`: atrpms
  - `libglew-devel 1.7.0`: linuxtech-release

Python Options
--------------
* CentOS 6.x ships with python 2.6.6 and some somewhat outdated python packages. It is recommended to install Python 2.7 for speed and compatibility. Then, one can set up a virtualenv and use pip to install the necessary bits.  Alternatively, you could stay on python 2.6.6 and just install/upgrade the packages you want. One can install newer version of the numpy by installing via the PUIAS Computational repo, e.g:
    - `yum update http://puias.math.ias.edu/data/puias/computational/6/x86_64/numpy-1.6.2-0.1.puias6.x86_64.rpm`
    - `yum update http://puias.math.ias.edu/data/puias/computational/6/x86_64/numpy-f2py-1.6.2-0.1.puias6.x86_64.rpm`

* Then, one can install pip to update the python packages, e.g.:
    - `yum install python-pip`
    - `pip-python install --upgrade scipy matplotlib psutil twisted`

ISMRMRD and Gadgetron
---------------------
**The steps may be executed as a regular user.**

* Create install directory
    - `mkdir ~/local`
* ISMRMRD
    - `git clone https://github.com/ismrmrd/ismrmrd.git`
    - `cd ismrmrd`
    - `mkdir build; cd build`
    - `cmake28 -D CMAKE_INSTALL_PREFIX=~/local -D CMAKE_PREFIX_PATH=~/local/ ../`
    - `make;make install`
* Gadgetron
    - `git clone https://github.com/gadgetron/gadgetron.git`
    - `cd gadgetron`
    - `git checkout development`
    - `mkdir build; cd build`
    - `cmake28 -D CMAKE_INSTALL_PREFIX=~/local -D CMAKE_PREFIX_PATH=~/local/ ../`
    - `make; make install`

Environment Variables
---------------------
The final step is to add/modify a few environment variables in your `~/.bashrc` file:

    export GADGETRON_HOME=/usr/local/gadgetron
    export ISMRMRD_HOME=/usr/local
    export PATH=$PATH:$GADGETRON_HOME/bin:$ISMRMRD_HOME/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$GADGETRON_HOME/lib:$ISMRMRD_HOME/lib

Rename the example configuration file `$GADGETRON_HOME/config/gadgetron.xml.example` to `$GADGETRON_HOME/config/gadgetron.xml`.