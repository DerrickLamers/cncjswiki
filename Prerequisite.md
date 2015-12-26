You will need an Arduino UNO/Nano board based on the ATmega328P. Download the latest Grbl firmware from the [Grbl  repository](https://github.com/grbl/grbl), and [flash Grbl to an Arduino](https://github.com/grbl/grbl/wiki/Flashing-Grbl-to-an-Arduino).

## Running without using Arduino board
If you don't have an Arduino, check out [grbl-sim](https://github.com/grbl/grbl-sim) and follow the instructions below to compile Grbl into an executable for your computer:

1. Clone this repository into the directory containing the Grbl source code (i.e. `<repo>/grbl/`), like so:

  ```bash
  $ git clone git@github.com:grbl/grbl.git
  $ cd grbl/grbl
  $ git clone git@github.com:grbl/grbl-sim.git
  $ cd grbl-sim
  ```
2. Edit the Grbl-sim Makefile to select the correct `PLATFORM =` line.
3. Run `make new` to compile the Grbl sim. It will create an executable file named `grbl_sim.exe`. See below:

  ![grbl-sim](https://raw.githubusercontent.com/cheton/cnc.js/master/media/grbl-sim.png).
4. On Linux, run the updated version of [simport.sh](https://github.com/cheton/cnc.js/blob/master/examples/grbl-sim/simport.sh) (`examples/grbl-sim/simport.sh`) to create a fake serial port (`/dev/ttyFAKE`), and use it to test your Grbl interface software.
5. Copy [.cncrc](https://github.com/cheton/cnc.js/blob/master/examples/grbl-sim/.cncrc) from [examples/grbl-sim](https://github.com/cheton/cnc.js/tree/master/examples/grbl-sim) to the home directory, and run `cnc -c ~/.cncrc` to start the server. The configuration file should look like below:

  ```json
  {
      "ports": [
          {
              "comName": "/dev/ttyFAKE",
              "manufacturer": "grbl-sim"
          }
      ]
  }
  ```
6. Open `/dev/ttyFAKE` from the Connection widget to interact with the Grbl simulator as if connected to an Arduino with Grbl.

  ![ttyFAKE](https://raw.githubusercontent.com/cheton/cnc.js/master/media/ttyFAKE.png)