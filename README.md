# ese-5190_Lab_setupguide_Sushrut

In this guide we have used windows subsystem for linux to execute our code on the RP2040 microcontroller Adafruit QT board. This approach uses linux based commands. The details of the laptop used are – 
Laptop - 	Lenovo Legion 5 2022
Operating system – Windows 11 
Ram and processor – 32gb ram AMD Ryzen 7 

The windows subsystem for linux (WSL) is used to run a linux system with all its functionality directly on a windows machine. The linux terminal helps us to easily navigate the directories, make edit files, build files and do many more advanced stuff. For editing the code Visual studio code is used. Putty is used as a terminal to display the serial output. 
1. The first step will be to open the terminal in Windows i.e Windows powershell and run it as administrator then type in the wsl –install command. 
![Screenshot (16)](https://user-images.githubusercontent.com/114092860/195965269-e58f0800-996c-44fb-aa67-635f9c128396.png)

This command will start installing Windows subsystem for linux and ubuntu. After installation you will be prompted to reboot your system. Then after restarting the ubuntu terminal will automatically pop-up. It will prompt you to enter a username and password.  After this is successfully done your terminal will look like this –  
![Screenshot (17)](https://user-images.githubusercontent.com/114092860/195965287-3c627e1f-b7da-4fae-8a64-875b5192b6a0.png)

2. Download Visual studio code from https://code.visualstudio.com/download . After successfully installing it you will need certain extensions inside VS code. Navigate to the extensions options then in search type in WSL and C/C++ for visual studio code and install both extensions. This completes the setup for visual studio code.  

3. Download the putty terminal from https://www.putty.org/. This will be used later on for serial interfacing.
The above part completes the installation of all programs we need for the setup. The Now we need the Raspberry Pi Pico SDK (Software development Kit) on our machine which includes all the header files, libraries and build system which is essential to write programs, compile and flash them on the RP2040 Adafruit QtPy board. We are using the Adafruit QtPy board which is slightly different than the Pico board which we are using as a reference for our codes and libraries. It is important to keep in mind the differences by going through both the datasheets and making the changes accordingly. The main pico reference been used in this lab is - https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf. 


Now the below step by step guide is used to print Hello, world on the serial terminal putty. 
1. Open the Windows subsystem for linux Ubuntu terminal. Make sure windows is up to date with all the latest drivers and updates. 
2. Now some linux tools need to be installed using the following commands. – 
sudo apt install linux-tools-5.4.0-77-generic hwdata                                              sudo update-alternatives --install /usr/local/bin/usbip    
/usr/lib/linux-tools/5.4.0-77-generic/usbip 20 
![Screenshot (3)](https://user-images.githubusercontent.com/114092860/195965384-28a5a99d-f64e-4e84-be7a-1eb5926cb085.png)


3. Now to install the sdk and other files we need to create a pico directory. 
$ cd ~/
$ mkdir pico
$ cd pico


4. Now we need to clone the pico- sdk and pico-examples git repositories. This includes a number of example codes which can be modified and written on the RP2040 QtPy board using the pico-sdk. 
$ git clone -b master https://github.com/raspberrypi/pico-sdk.git 
$ cd pico-sdk 
$ git submodule update --init 
$ cd .. 
$ git clone -b master https://github.com/raspberrypi/pico-examples.git

![Screenshot (4)](https://user-images.githubusercontent.com/114092860/195965455-a926999e-3ca2-4309-8e33-f27a0f2c1278.png)

![Screenshot (5)](https://user-images.githubusercontent.com/114092860/195965490-30f1d070-3c9a-4acf-8ae3-48689cd8ce3b.png)

5. In order to build applications in pico-examples we require extra tools which are CMake and GNU embedded toolchain for Arm. Cmake is an open-source, cross-platform tool that uses compiler and platform independent configuration files to generate native build tool files specific to any compiler and platform that we are using. The GNU Arm emebedded toolchain is a ready-to-use, open-source suite of tools for C, C++ and assembly programming. All this can be done from the Ubuntu terminal itself by using the following commands.
$ sudo apt update 
$ sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi        build-essential libstdc++-arm-none-eabi-newlib

![Screenshot (6)](https://user-images.githubusercontent.com/114092860/195965498-108ad906-8a41-46c0-be82-26c2fef81136.png)

6. In order to update your sdk to a new version the following steps are followed 
$ cd pico-sdk 
$ git pull 
$ git submodule update

7. In order to compile and run RP2040 code execute the following commands. We need to set the path PICO_SDK_PATH=~/pico/pico-sdk after cloning and prepare cmake file by running the command cmake –
$ cd ~/
$ cd pico/pico-examples
$ mkdir build 
$ cd build
$ export PICO_SDK_PATH=~/pico/pico-sdk
$ cmake ..

![Screenshot (9)](https://user-images.githubusercontent.com/114092860/195965543-33e2030b-62f4-425f-96d7-901bc4c9f184.png)

8. After we edit the code fro the examples folder we need to build the the target using the following commands – 
$ cd ~/
$ cd pico/pico-examples/build/hello_world
$ make -j4
 ![Screenshot (11)](https://user-images.githubusercontent.com/114092860/195965600-a6ab5686-2260-441b-8035-2c2323e6061c.png)


9. It takes a few seconds to build the target file. After a target has been built it produces a hello_usb.uf2 file which is stored inside the build folder of the pico-examples ubuntu folder. Now to retrive that folder from ubuntu to windows we need to execute the following commands. 
$ cd ~/
$ cd pico/pico-examples/build/hello_world/usb
$ cp hello_usb.uf2 /mnt/c/Users/98sus/Downloads
![Screenshot (14)](https://user-images.githubusercontent.com/114092860/195965698-f05800dd-fb8a-44a8-b43f-839665696953.png)


10. Now that we have the file we need to run it on the  RP2040 microcontroller. In order to that we first need to reset the RP2040 board after plugging it in the laptop by holding the boot and rst button for a few seconds and then releasing the boot button. Then in the file explorer you will see the drive RPI-RP2. Now in Putty we need to change the com port and change the baud rate to 115200. Type in the name of the session and click on load and open. Then drag and drop the hello_usb.uf2 file in the RPI-RP2 drive. The putty’s terminal will display Hello, world!
![Screenshot (2)](https://user-images.githubusercontent.com/114092860/195965755-e2b45461-acf2-463b-8be7-e0eb895ce4cd.png)





 
