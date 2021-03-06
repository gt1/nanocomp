nanocomp
========

Conversion of nano pore HDF5 to FastQ

Source
------

The nanocomp source code is hosted on github:

	git@github.com:gt1/nanocomp.git

Compilation of nanocomp
---------------------------

nanocomp needs libmaus [https://github.com/gt1/libmaus] and 
HDF5 [http://www.hdfgroup.org/downloads/index.html]. 
When libmaus is installed in ${LIBMAUSPREFIX} and HDF5 in ${HDF5PREFIX}
then nanocomp can be compiled and installed in ${HOME}/nanocomp using

	- autoreconf -i -f
	- ./configure --with-libmaus=${LIBMAUSPREFIX} \
		--with-hdf5=${HDF5PREFIX} --prefix=${HOME}/nanocomp
	- make install

Please note that HDF5 needs to be set up for multi threading, otherwise nanocomp will crash.

Running the program
-------------------

The program can be run using

	nanocomp directory_with_hdf5_files

A  list of options can be obtained by running

	nanocomp -h
