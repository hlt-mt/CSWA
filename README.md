# CSWA
Continuous Space Word Alignment Model


HOW TO INSTALL WITH CMAKE

- cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="/path/where/to/install"
- make
- make install


Note: If your g++ compiler does not support '-std=c++0x', please add parameter '-DCXX0:BOOL=OFF'
To check whether g++ complier does support '-std=c++0x', please run the following command:
$> echo | g++ -E -x c++ -std=c++0x -dM - >& /dev/null ; echo $?

