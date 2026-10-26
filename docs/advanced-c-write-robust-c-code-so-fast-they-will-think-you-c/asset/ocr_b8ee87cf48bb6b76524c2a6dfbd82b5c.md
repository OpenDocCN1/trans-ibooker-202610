```
s/CxxTemplate.dir/link.txt --verbose=1
/usr/bin/c++ -g CMakeFiles/CxxTemplate.dir/src/CxxTemplate.cpp.o CMakeFiles/CxxTemplate.dir/src/ANewClass.cpp.o CMakeFiles/CxxTemplate.dir/src/SumFunc.cpp.o CMakeFiles/CxxTemplate.dir/src/LinearMotion1D.cpp.o -o CxxTemplate -lpthread
make[2]: Leaving directory '/home/gazihan/CxxTemplate/build/Debug'
[ 45%] Built target CxxTemplate
make -f tests/CMakeFiles/tests.dir/build.make tests/CMakeFiles/tests.dir/depend
make[2]: Entering directory '/home/gazihan/CxxTemplate/build/Debug'
cd /home/gazihan/CxxTemplate/build/Debug && /home/gazihan/cmake-3.15.1-Linux-x86_64/bin/cmake -E cmake_depends "Unix Makefiles" /home/gazihan/CxxTemplate /home/gazihan/CxxTemplate/tests /home/gazihan/CxxTemplate/build/Debug /home/gazihan/CxxTemplate/build/Debug/tests /home/gazihan/CxxTemplate/build/Debug/tests/CMakeFiles/tests.dir/DependInfo.cmake --color=
make[2]: Leaving directory '/home/gazihan/CxxTemplate/build/Debug'
make -f tests/CMakeFiles/tests.dir/build.make tests/CMakeFiles/tests.dir/build
make[2]: Entering directory '/home/gazihan/CxxTemplate/build/Debug'
[ 54%] Building CXX object tests/CMakeFiles/tests.dir/CanTest.cpp.o
cd /home/gazihan/CxxTemplate/build/Debug/tests && /usr/bin/c++ -g -std=gnu++17 -o CMakeFiles/tests.dir/CanTest.cpp.o -c /home/gazihan/CxxTemplate/tests/CanTest.cpp
[ 63%] Building CXX object tests/CMakeFiles/tests.dir/SumFuncTest.cpp.o
cd /home/gazihan/CxxTemplate/build/Debug/tests && /usr/bin/c++ -g -std=gnu++17 -o CMakeFiles/tests.dir/SumFuncTest.cpp.o -c /home/gazihan/CxxTemplate/tests/SumFuncTest.cpp
[ 72%] Building CXX object tests/CMakeFiles/tests.dir/__/src/SumFunc.cpp.o
cd /home/gazihan/CxxTemplate/build/Debug/tests && /usr/bin/c++ -g -std=gnu++17 -o CMakeFiles/tests.dir/__/src/SumFunc.cpp.o -c /home/gazihan/CxxTemplate/src/SumFunc.cpp
[ 81%] Building CXX object tests/CMakeFiles/tests.dir/LinearMotion1DTest.cpp.o
cd /home/gazihan/CxxTemplate/build/Debug/tests && /usr/bin/c++ -g -std=gnu++17 -o CMakeFiles/tests.dir/LinearMotion1DTest.cpp.o -c /home/gazihan/CxxTemplate/tests/LinearMotion1DTest.cpp
[ 90%] Building CXX object tests/CMakeFiles/tests.dir/__/src/LinearMotion1D.cpp.o
cd /home/gazihan/CxxTemplate/build/Debug/tests && /usr/bin/c++ -g -std=gnu+
```

Figure 1.36: The build process part 2