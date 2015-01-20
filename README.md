**Purpose**:

   The goal of this repository is to help you compile and run SPEC. This will
   NOT verify the output of SPEC.

**Requirements**:

   - you must have your own copy of SPEC CPU2006 v1.2. 
   - you must have built the tools in SPEC CPU2006 v1.2 (see below for help). 

**Details**:

   We will compile the binaries "in vivo", calling into the actual SPEC CPU2006
   directory. Once completed, the binaries are copied into this directory (./build). 
   
   The reasoning is that compiling the benchmarks is complicated and difficult (so
   why redo that effort?), but we want better control over executing the binaries.  Of
   course, we are forgoing the validation and results building infrastructure of
   SPEC. 
   
**Setup**:

   - set the $SPEC_DIR variable in your environment to point to your copy of CPU2006-1.2.
   - modify Speckle/riscv.cfg as desired. It will get copied over to
     $SPEC_DIR/configs when compiling the benchmarks. 
   - modify the BENCHMARKS variable in gen_binaries.sh as required to set which
     benchmarks you would like to compile and run.
   - modify the RUN variable in gen_binaries.sh as required to set how you
     would like to run the binaries (e.g., RUN="spike pk" to run on the Spike
     ISA simulator).
   
**To compile binaries**:

        ./gen_binaries.sh --compile

   You only need to compile SPEC once for a given SPEC input ("test", "train",
   "ref"). It should take about a minute. 
    
**To run binaries**:

        ./gen_binaries.sh --run
    
**TODO**
   
   - specify the SPEC input mode so we can properly symlink to the input datasets.


**Building SPEC Tools**

   These are the instructions that I had to follow to build the CPU2006 v1.2
   tools from scratch on Intel amd64 machines running Ubuntu.

   First, you can try:

        cd $SPEC_DIR/
        ./install.sh

   Hopefully that works. 
   
   Otherwise, you can also try installing the tools from scratch.
   Begin by creating a script (my_setup.sh) in cpu2006-1.2/tools/src with the
   following code:

        #!/bin/bash
        PERLFLAGS=-Uplibpth=
        for i in `gcc -print-search-dirs | grep libraries | cut -f2- -d= | tr ':' '\n' | grep -v /gcc`; do
            PERLFLAGS="$PERLFLAGS -Aplibpth=$i"
        done
        export PERLFLAGS
        echo $PERLFLAGS
        export PATH=$PATH:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin

   Then:

        cd cpu2006-1.2/tools/src
        source my_setup.sh
        ./buildtools


