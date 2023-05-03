Download Link: https://assignmentchef.com/product/solved-cs2110-homework9-graphics-library
<br>
For this assignment, you will write in the C language a graphics library that is capable of drawing a variety of shapes, as well as a number of filters that can be applied on the colors of the pixels of an image.

<h1><a name="_Toc6895"></a>1           Instructions</h1>

<h2><a name="_Toc6896"></a>1.1         Setting Up</h2>

It is expected that you work on this assignment and run your code on a computer or VM running Ubuntu

16.04. Run the following commands to install dependencies used to build and test your code:

sudo apt update

sudo apt install check build-essential pkg-config valgrind gdb

<h2><a name="_Toc6897"></a>1.2         Getting Started</h2>

<strong>Read through the </strong><em>geometry.h </em><strong>file. The comments here will explain each of the structs so that you know what you’re doing when you start writing your functions. </strong>This file is thoroughly commented in order to make sure you are able to understand the structs here.

<strong>Now take a look at the </strong><em>graphics.h </em><strong>file. </strong>This file contains the headers and details of each of the functions you need to implement.

<strong>You will write all of your code in the </strong><em>graphics.c </em><strong>file. If you edit other files, the tester will use their originals and thus your code might not compile. </strong>This file is prepopulated with all of the functions you will have to write. If you accidentally delete any, you can still find the prototypes in the <em>graphics.h </em>file. Note that for this assignment, some C standard library headers are included, and you can feel free to use any functions included in those headers.

The file is distributed by default with the UNUSED macro in each function that keeps gcc from complaining about unused variables. Without those, you wouldn’t be able to compile the file before fully writing all of the functions. So as you are writing each function, <strong>be sure to remove the UNUSED macro calls from its body first</strong>, so that the compiler can accurately warn you about unused variables.

<h2><a name="_Toc6898"></a>1.3         Understanding How Pixel Arrays and 16-bit Colors Work</h2>

<h3><a name="_Toc6899"></a>1.3.1         How does a 1-d pixel array work for displaying colors on a screen?</h3>

At first it might seem weird to represent a 2-d object (the screen) with a 1-d array of pixels, but it’s actually pretty straightforward. Think about a text document. If you’re typing on one line, eventually it will wrap around to the start of the next line, even though you were just typing one character after the next. The pixel array for the screen works the same way: the end of each row wraps around to the start of the next row. So if the screen is 240×160, for example, then pixels 0-239 are the first row, pixels 240-479 are the second row, continuing for all 160 rows. This convention for storing an array is referred to as “row major” order and is used by most languages, including C, for storing two-dimensional arrays in a one-dimensional memory.

Figure 1: Visual Explanation of Pixel Layout

<h3><a name="_Toc6900"></a>1.3.2         How are the individual pixels represented as integers?</h3>

We represent each pixel as 16 <strong>unsigned </strong>bits (2 bytes) in a BGR format. With 5 bits to represent the amount of blue, 5 bits for the amount of green, and 5 bits for the amount of red. However, this only adds up to 15 bits, so the most significant bit is unused. The reason it’s a BGR format instead of the more commonly known RGB format is because the 5 bits for each color are organized (from most significant bits to least significant bits) blue, green, then red. This is also the layout of pixels on the GBA that we’ll use later on.

Figure 2: The layout of bits in a 16-bit BGR format

<h2><a name="_Toc6901"></a>1.4         Drawing Geometry</h2>

You need to implement a number of functions in <em>graphics.c </em>that will be used to draw the shapes in <em>geometry.h</em>. Prototypes for each function are provided in <em>graphics.h </em>in the order that we recommend that you implement them. The documentation above each function explains in detail the expected behavior for that function.

<h2><a name="_Toc6902"></a>1.5         Color Filters</h2>

The filters that you are expected to implement are very simple functions of single pixels that produce a new pixel based on the input. For example, the Greyscale filter will produce a greyscale version of the given pixel, the Red Only filter will only let the red channel of the pixel pass through, and the provided No Filter filter is the identity function on the pixel, so that you can use the drawImage function without implementing the filters first. These functions are also in the <em>graphics.h </em>and <em>graphics.c </em>files for you to work on.

<h1><a name="_Toc6903"></a>2           Testing Your Work</h1>

You are provided with two different pieces of software to test your work with: the sandbox and the tester.

<h2><a name="_Toc6904"></a>2.1         Sandbox</h2>

The sandbox is provided in the assignment directory. In the <em>sandbox.c </em>file there is a bunch of code that sets up a test image and provides you with an environment to test your code with. You can change the code between the comments indicating the area you can change. Once you save the file and run the make run-sandbox command from the command line, the resulting image will be output to the <em>output.bmp </em>file which you can view using an image viewer.

To help you see cases where you might be writing outside the image bounds, the image will be drawn such that your actual drawing will appear between two horizontal black lines. If you cannot see these lines or if they are incomplete, it means your functions are drawing outside the bounds of the pixel array, which is something you should correct.

The sandbox is shipped with sample images (azul, tvtester, austinbear) that you can use to test your <em>drawImage </em>function, too.

The sandbox will not be used for grading, its purpose is to simply show you what image your code draws on a hypothetical screen, in order to make your code easier to debug.

If you need to debug code you wrote in the sandbox, you can run the make run-sandbox-gdb command to launch gdb for the sandbox.

<h2><a name="_Toc6905"></a>2.2         Tester</h2>

The tester is provided in the <em>tester </em>directory. It contains unit tests that call your functions with certain parameters and compare the output with the TA solution’s outputs (the source code for the tests is in <em>graphics suite.c</em>.

<strong>The more important feature of the tester is that it shows you as an image file both the output your code produced and the expected output for the tests. </strong>Your code’s results are found in the <em>tester/actual/ </em>directory, while the expected images are found in the <em>tester/expected/ </em>directory, so that you can compare them using any image viewer. Moreover, the <em>tester/diff/ </em>directory contains images that are diffs of the expected and the actual: pixels that match on the two are marked green and pixels that don’t match are marked red.

<strong>This is your best bet at debugging your code, and at understanding what’s expected of each function! The tester will have the expected image and the image your code produced for all of the test cases. Comparing those should be able to tell you what’s wrong with your code.</strong>

<strong>To run the tester, run the following command (or any of the other ones listed below in the Makefile section: </strong>make run-tests

Note the tests are unweighted when you run them like this, but they are weighted on Gradescope.

<h2><a name="_Toc6906"></a>2.3         Makefile</h2>

We have provided a Makefile for this assignment that will build your project. Here are the commands you should be using with this Makefile:

<ol>

 <li>To clean your working directory (use this command instead of manually deleting the .o files): make clean</li>

 <li>To run the sandbox: make run-sandbox</li>

 <li>To run the sandbox with gdb: make run-sandbox-gdb (gdb instructions are available below, in the HW8 doc, and on Piazza)</li>

 <li>To run the tests without valgrind or gdb: make run-tests</li>

 <li>To run your tests with valgrind: make run-valgrind</li>

 <li>To debug a specific test with valgrind: make TEST=test name run-valgrind</li>

 <li>To debug a specific test using gdb: make TEST=test name run-gdb Then, at the (gdb) prompt:

  <ul>

   <li>Set some breakpoints (if you need to — for stepping through your code you would, but you wouldn’t if you just want to see where your code is segfaulting) with b tester/graphics c:420, or b graphics.c:69, or wherever you want to set a breakpoint</li>

   <li>Run the test with run</li>

   <li>If you set breakpoints: you can step line-by-line (including into function calls) with s or step over function calls with n</li>

   <li>If your code segfaults, you can run bt to see a stack trace</li>

  </ul></li>

</ol>

For more information on gdb, please see one of the many tutorials linked above.

To get an individual test name, you can look at the output produced by the tester. For example, the following failed test is test list size empty:

suites/list_suite.c:906:F:test_list_size_empty:test_list_size_empty:0: Assertion failed… ^^^^^^^^^^^^^^^^^^^^

Beware that segfaulting tests will show the line number of the last test assertion made before the segfault, not the segfaulting line number itself. This is a limitation of the testing library we use. To see what line in your code (or in the tests) is segfaulting, follow the “To debug a specific test using gdb” instructions above.