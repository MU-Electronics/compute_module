# QuePi Docs

This repo contains documentation for the QuePi project.

QuePi is a project started by the Electronics Section at The University of Manchester. 

QuePi was developed to provided a base high level embedded platform for development of prototypes to be used in teaching and research.

## Documentation Content

### Development PC setup guides

* Installing Cygwin

#### Cross compiling setups guides

* [Windows]()
* MacOs

### Raspbian setup for product products

* Boot to application guide
* Setting up Ethernet
* Setting up LCD
* Setting up SSH
* Installing WirePi for GPIO control



## Hardware Designs

The QuePi project uses the Raspberry Pi compute module 3 for the embedded processor and Qt as the software framework. Schematic and PCB designs have been created (using Altium Designer), these designs can be used as a starting point for all high level embedded projects; Download the design projects below

* Altium project (CAM1, DISP1, All GPIO, Input Regulation, HDMI, Ethernet, Single USB)
  * X axis expanding design finished
  * Y axis expanding design to follow


* Other configurations to follow

## Software 

One of the largest parts of developing new prototypes is time spent in software development. To speed up this process QuePi aims at supporting every aspect of your development.

### Boiler plate projects

There are two boiler plate project templates downloadable, 

* Terminal applications
* GUI applications 

These help by provided a recommend project folder and application structure.

### Vender

The vendor repo located here provided thirdparty and QuePi libraries that help with everything from reading ADCs to automatic program updater. 

### Qt

Compiled Qt binaries for the CM3 which allows you to jump straight into developing the program. The binaries are compressed for storage reasons, free programs such as 7zip etc can uncompressed the binaries stored as tar.gz. These can be downloaded below:

* Qt 5.9, with hardware acceleration << 2nd in queue to be provided
* Qt 5.9, no hardware acceleration << 3rd in queue to be provided
* Qt 5.8, with hardware acceleration  << 1st in queue to be provided
* [Qt 5.8, no hardware acceleration]()

Other binaries can be provided on required, submit a request through the issue manager in GitHub.

## Disclaimer

As and when jobs require features adding they are added but this project is also developed in the staffs free time! So please bear that in mind, this is not a commercial project and has no links to Qt or Raspberry Pi.

As with all open source and hardware projects there is no warranty provided and you take full responsibility for using any part of this project.

You must publish all modifications to the public domain.

Licensing is under GNU, special permissions on request.