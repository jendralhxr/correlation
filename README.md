README

This application performs digital image correlation based on the optical flow method by Lucas and Kanade. A small video 
that illustrates how this program works is at https://youtu.be/Ak4puzdqWDM

![gui](/gui.png)

What is this repository for?

It consists of a Qt GUI that calls a multi-threading computational engine, running on the CPU and a CUDA engine to perform the computations in the GPU. User can choose which one to use.

A set of images can be selected in the GUI, which will display the first two images of the set. Selection of the domain to be correlated can be done using rectangular domains, annular domains and "blob" domain, which are defined by a continuous trace of the mouse over the image. Domains can also be adjusted numerically and via mouse clicks.

The correlation images are transformed into image pyramids (https://en.wikipedia.org/wiki/Pyramid_(image_processing)) This method correlatess first coarser representations, the resulting parameters of which are used for the next pyramid. The user can select the pyramid architechture via controls on the gui.

Initial guess for the correlation algorithm can be automatic, null or user-selected. In the last mode, the user can adjust the position of the domain in the first deformed image via mouse clicks. After the initial guess is defined for the first two images, the motion is assumed to be at constant velocity, so past deformations are extrapolated to produce the next initial guesses.

Correlation can be performed using the models U, UV, UVQ and UVUxUyVxVy. The first model, U, computes only a displacement in the horizontal direction, Last model, UVUxUyVxVy computes displacements in both directions, and their 2D gradients. The model UVQ is a hybrid representation, quasi-rigid body, with a small angle approximation.

The user can also select how the interpolation is done on the deformed images. Choices are: "nearest", "bilinear" and "bicubic" interpolation. 

User can also choose the reference image for the correlation, the previous image or the first image.

Reports can be saved with the resulting correlation parameters, number of iterations and chi.

How do I get set up?

Summary of set up

The project is currenly set up as a Qt Creator project in Ubuntu 16.04. It uses the openCV libraries, CUDA, openmp and Eigen. Of course also uses Qt. Also a visual studio 2015 is available, look for the .sln file.

Configuration

Gui allows for defining the number of thread used in the CPU, which I currnetly have at 20. When I run the code in my laptop (smaller chip) I change that to 4-8. Also in gui one can choose if you want to use CUDA and the GPU. Since I don't have a CUDA compatible laptop, I set CUDA_ENABLED to false in "defines.h". The maximum number of GPUs is set to the unrealistic number of 32. I just use that variable to statically define a small array for the cuda class, so there is not that much down side. The code detects the number of available GPUs. At the moment, since the implementation of the GPU image pyramids, I set the number of GPUs = 1 always, cause I have not propagated the pyramid arcitecture and multi-GPU architecture together.

I use -std=c++14, -fopenmp and -use_fast_math in anticipation of using some of the abreviated function in the future.

Dependencies

Project needs to link to

openCV: I use version 3.3. Other earlier version will probably work too, since I am not using functionality other than the very basic, such us opening images. Used libraries are -lopencv_highgui -lopencv_core -lopencv_imgproc -lopencv_imgcodecs

CUDA: I used a somehow more complex version of CUDA than what I need at the moment, in anticipation of playing around with the dynamic parallelism. For this I do separate compilation with relocatable device code, which requires the separate compilation ( and CUDA linking with -lcudadevrt ). Other than that, I use -lcudart -lcuda -lcusolver -lcublas. I use the CUDA version 8, with the 8.61.2 patch.

Eigen: This is a easy one to setup. Just needs the header files.

Qt: I use core, gui, widgets and printsupport. 5.3, 5.9 and 5.11 work.

openmp: parallelization of some loops.

I also link to the nvToolsExt from nVidia to make CPU side debug traces in the nvvp profiler.

Who do I talk to?

Send me an email at namascar@gmail.com with comments/suggestions
