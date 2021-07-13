
Welcome to CoolProp 
===================

CoolProp is a thermophysical property database and wrappers for a selection of programming environments. 
It offers similar functionality to REFPROP, but CoolProp is open-source and free. 
It was originally developed by Ian Bell, at the time a post-doc at the University of Liege, in Liege, Belgium.

* CoolProp has flexible licensing terms: Commercial - ok! Academic? - ok! |ghlicense|

* For Python, get the latest release via ``pip install coolprop`` |pypidownloads| |pypiversion| 

* ... other binaries are available from `SourceForge <http://sourceforge.net/projects/coolprop/files>`_  |sfdownloads| |ghversion|

* There is also a bleeding edge nightly build of the `development version <http://sourceforge.net/projects/coolprop/files/CoolProp/nightly>`_ .

* The documentation is available for the `latest release <http://www.coolprop.org>`_ and the `development version <http://www.coolprop.org/dev>`_  

* For any kind of question regarding CoolProp and its usage, you can ask the `CoolProp user group <https://goo.gl/Pa7FBT>`_ 

* ... you might also find answers in our `FAQ <https://github.com/CoolProp/CoolProp/blob/master/FAQ.md>`_ 

* If you found a bug or have an issue that requires the developers to become active, please file an issue in our `issue tracker <https://github.com/CoolProp/CoolProp/issues>`_ 

* `Contributions <https://github.com/CoolProp/CoolProp/blob/master/.github/CONTRIBUTING.md>`_ to this project are welcomed and encouraged!  If you wish to `contribute <https://github.com/CoolProp/CoolProp/blob/master/.github/CONTRIBUTING.md>`_ bug fixes, patches, or new features, wrappers, or material properties, please submit a Pull Request with your code.

* If you are new to Git and Github, please see the `CoolProp Wiki <https://github.com/CoolProp/CoolProp/wiki>`_ for guidance on becoming a contributor to the project.

* Accelerate development of things you really need implemented by posting at `Bountysource <https://www.bountysource.com/teams/coolprop>`_ 

* Check Travis CI for build failures |travisbuilds| and have a look at the coverity stats |coveritystatus|

.. 
   Downloads and other stats
   -------------------------
   
   ===============  ==============================
   Binary release:  |sfdownloads| |ghversion| 
   PyPI release:    |pypidownloads| |pypiversion|
   ===============  ==============================




.. |ghversion| image:: https://img.shields.io/github/release/CoolProp/CoolProp.svg?label=SF-binaries
    :alt: CoolProp version tag

.. |sfdownloads| image:: https://img.shields.io/sourceforge/dm/CoolProp.svg?label=SF-downloads
    :target: http://sourceforge.net/projects/coolprop/files
    :alt: sourceforge downloads

.. |pypidownloads| image:: https://img.shields.io/pypi/dm/CoolProp.svg?label=PyPI-downloads
    :target: http://pypi.python.org/pypi/CoolProp/
    :alt: PyPI downloads

.. |pypiversion| image:: https://img.shields.io/pypi/v/coolprop.svg?label=PyPI-binaries
    :target: http://pypi.python.org/pypi/CoolProp/
    :alt: PyPI version

.. |ghlicense| image:: https://img.shields.io/github/license/CoolProp/CoolProp.svg
    :target: https://github.com/CoolProp/CoolProp/blob/master/LICENSE
    :alt: license

.. |travisbuilds| image:: https://travis-ci.org/CoolProp/CoolProp.svg?branch=master
    :target: https://travis-ci.org/CoolProp/CoolProp
    :alt: Travis CI builds

.. |coveritystatus| image:: https://scan.coverity.com/projects/4375/badge.svg
    :target: https://scan.coverity.com/projects/coolprop
    :alt: Coverity Scan Build Status

.. 
   image:: https://www.bountysource.com/badge/team?team_id=14160&style=raised
    
.. |bounties| image:: https://img.shields.io/bountysource/team/coolprop/activity.svg
   :alt: Post a bounty at https://www.bountysource.com/teams/coolprop
   :target: https://www.bountysource.com/teams/coolprop?utm_source=CoolProp&utm_medium=shield&utm_campaign=raised

.. 
   image:: https://badges.gitter.im/Join%20Chat.svg
   :alt: Join the chat at https://gitter.im/CoolProp/CoolProp
   :target: https://gitter.im/CoolProp/CoolProp?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
   
   
SWIG Coolprop wrapping for Golang 
===================

The workflow was written in `CoolProp issue 1871 <https://github.com/CoolProp/CoolProp/issues/1871#issuecomment-582898288>`_  by   `Gautier Rouaze <https://github.com/GautierR>`_ , here only add more details to the process.

Apply to: **Linux OS** and **WSL for windows user**, here we are doing to **Ubuntu** users.
*if you use another distro, well we assume you can handle with the differences  ;)*

**Prerequisites**

- ``sudo apt update && sudo apt upgrade``
- ``sudo apt install build-essential``
- ``sudo apt install python-is-python3``
- ``sudo apt install cmake``
- ``sudo apt install swig``
- `Install Go <https://golang.org/doc/install>`_

**Workflow**

Clone the ``golang_module`` branch in our `imhotep-nb CoolProp fork <https://github.com/imhotep-nb/coolprop/tree/golang_module>`_ .

Use the ``--recursive`` argument if you don't want to suffer manually cloning all the  `CoolProp/externals/ <https://github.com/imhotep-nb/coolprop/tree/golang_module/externals>`_ repositories

- ``git clone -b golang_module git@github.com:imhotep-nb/coolprop.git CoolProp --recursive``

- ``cd CoolProp``

- ``mkdir build && cd build``

- ``cmake .. -DCOOLPROP_GOLANG_MODULE=ON``

- ``cmake --build .``

At this point if you will get an error like ``fatal error: gitrevision.h: No such file or directory``, please follow the process in `e7c5493 <https://github.com/imhotep-nb/coolprop/commit/e7c54933825f3da379c490ef241d9a428716f9a2>`_.

Move the ``CoolProp.go`` and ``CoolProp.so`` files generates in ``/CoolProp_go/build/`` inside a new go package (under ``/go/src/CoolProp``).

- ``mkdir $GOPATH/src/CoolProp``
- ``scp CoolProp.go $GOPATH/src/CoolProp/CoolProp.go``
- ``scp CoolProp.so $GOPATH/src/CoolProp/CoolProp.so``

Move to this directory and update the ``cgo`` part of ``CoolProp.go`` file with the LDFLAGS (linking the shared library).

- ``cd $GOPATH/src/CoolProp``
- ``vim CoolProp.go``

Add the following line just before the import "C" statement (inside all the commented part).

- ``#cgo LDFLAGS: -L${SRCDIR} -l:CoolProp.so -ldl``

I mean here::

   package CoolProp
   /*
   #define intgo swig_intgo
   typedef void *swig_voidp;
   #cgo LDFLAGS: -L${SRCDIR} -l:CoolProp.so -ldl
   #include <stdint.h>

Try to build the package:

- ``go install``

Export the shared library to the LD_LIBRARY_PATH

- ``export LD_LIBRARY_PATH=$GOPATH/src/CoolProp``

Remember that the export only work in your active terminal session, if you want it permanently please read `How to set LD_LIBRARY_PATH permanently? <https://askubuntu.com/questions/950313/how-to-set-ld-library-path-permanently#950315>`_.

Then you can use the go package inside another program ``test_coolprop.go`` ::

   package main
   import "CoolProp"
   import "fmt"
   func main() {
          waterTCrit := CoolProp.Props1SI("Tcrit", "water")
          fmt.Printf("Water TCrit : %v degC \n", waterTCrit - 273.16)
   }

Run it and Go!

- ``go run test_coolprop.go``::

   Water TCrit : 373.936 degC


 
