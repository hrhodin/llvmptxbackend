PTX backand for LLVM
============

Backend code that emerged from Helge Rhodin's bachelor thesis 'A PTX code generator for LLVM'.

Instructions
============

Basically, the way Tom set this up is to allow all of us to use our own versions
of LLVM in a local repository (on the machine where we have checked out LLVM) and
still be able use each other's "passes" using shared libraries. This also gives a 
way of cleanly separating our passes from LLVM-core passes at the cost of some
additional command line options to opt. Here is one way to set up and start adding
new passes as shared libraries and use other passes too.

(1) First, make sure some environment variables are set up:

Assuming you already have the following environment variables defined:

LLVM_SRC_ROOT       (eg, /home/pprabhu/llvm/llvm/)
LLVM_OBJ_DIR        (eg, /home/pprabhu/llvm/llvm-objects/)
LLVM_INSTALL_DIR    (eg, /home/pprabhu/llvm/llvm-install/)

set up any include directory (using the absolute path):

export INCLUDE_DIR=/home/pprabhu/llvmptxbackend/include

(2) Configure the build system:

~/llvmptxbackend$ ./configure --with-llvmsrc=$LLVM_SRC_ROOT \
                              --with-llvmobj=$LLVM_OBJ_DIR \
                              --prefix=$LLVM_INSTALL_DIR \
                              --exec-prefix=$LLVM_INSTALL_DIR \
                              --includedir=$INCLUDE_DIR

(3) Run make to build all the shared objects:

~/llvmptxbackend$ make

(4) To test the backend try:

~/llvmptxbackend$ echo "int main() { return 42; }" | \
                  clang -x c - -emit-llvm -c -o - | \
                  llc -load ./Debug/lib/libPTXBackend.so -march=ptx

(5) Create a new directory for any new set of passes you want to add within the lib
directory, create .cpp files within that directory while the include directory has 
Liberty-specific .h files required by the .cpp files

![ImageForAnalytics](https://www.google-analytics.com/__utm.gif?v=1&cid=5555&t=pageview&tid=UA-102526720-1)
