them in the browser. This includes games like Tetris, Pong, and Pacman. Rather than attempt to recreate what he has already done masterfully, I am going to focus on a relatively simple and clean project.

I spent some time looking for good candidate code. I wanted something meaty but not overwhelming. Eventually I found Arash Partow’s collection of elegant, clean, suitably-licensed and useful C++ code at his website https://www.partow.net. If you go there you will find quite a lot of interesting material. I was originally going to use the computational geometry library but decided that the Bitmap library was better-suited to a book like this.

To start off, fetch the code from http://www.partow.net/programming/bitmap/index.html. Once you download the Zip file and uncompress it, you will see three files. A Makefile is an old-school Unix build file that has directions to assemble the software in question. We will explore that process momentarily. The bitmap_image.hpp file is the main library and the bitmap_test.cpp is a comprehensive collection of tests that generate a bunch of interesting Windows Bitmap images. This code does not require any platform-specific libraries.

```
brian@tweezer ~/g/w/s/c/bitmap> ls -alF
total 536
drwxr-xr-x@  5 brian  staff       160 Dec 31  1999 ./
drwxr-xr-x  11 brian  staff       352 Mar  6 13:56 ../
-rw-r--r--@  1 brian  staff       770 Dec 31  1999 Makefile
-rw-r--r--@  1 brian  staff    247721 Dec 31  1999 bitmap_image.hpp
-rw-r--r--@  1 brian  staff     20479 Dec 31  1999 bitmap_test.cpp
```

I have removed some of the comments and license details from Example 6-6 for space. What remains is the structure of the rules for building the test app, bitmap_test. A Makefile works by establishing a target followed by the dependencies and rules to build the target. As a convention, there is often an all rule which specifies the target file name mentioned above. It is dependent upon the .cpp and .hpp files. If either of those are modified, our executable needs to be rebuilt. To do that, the make tool will execute the file