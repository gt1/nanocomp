nanocomp
========

Conversion of nano pore HDF5 to FastQ

Source
------

The alternatives source code is hosted on github:

	git@github.com:gt1/nanocomp.git

Compilation of alternatives
---------------------------

nanocomp needs libmaus [https://github.com/gt1/libmaus] . When libmaus
is installed in ${LIBMAUSPREFIX} then alternatives can be compiled and
installed in ${HOME}/alternatives using

	- autoreconf -i -f
	- ./configure --with-libmaus=${LIBMAUSPREFIX} \
		--prefix=${HOME}/nanocomp
	- make install

Running the program
-------------------


