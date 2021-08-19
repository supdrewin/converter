UUP converter
-------------

### Description
A basic UUP converter aimed at Linux and macOS users who don't have access to any
Windows machine, but want or need to create an ISO image for latest Windows
Insider version downloaded from UUP dump.

**For obvious reasons this script will never support integration of Cumulative
Updates to created images.**

### Usage
``` shell
./convert.sh [compression] [uups_directory] [create_virtual_editions]
```

###### compression options:
 * wim - standard wim compression (`/Compress:max` in DISM) (default)
 * esd - solid esd compression (`/Compress:recovery` in DISM)

###### create_virtual_editions options:
 * 0 - do not create virtual editions (default)
 * 1 - create virtual edtitions

### Usage examples
 * `./convert.sh` - starts the conversion using files from `UUPs` directory and
   creates an ISO image with `install.wim`

 * `./convert.sh esd` - starts the conversion using files from `UUPs` directory
   and creates an ISO image with `install.esd`

 * `./convert.sh wim MyUUP` - starts the conversion using files from `MyUUP`
   directory and creates an ISO image with `install.wim`

 * `./convert.sh wim MyUUP 1` - starts the conversion using files from `MyUUP`
   directory, creates virtual editions and creates an ISO image with
   `install.wim`

### Virtual editions
Since version 0.5.0 this script supports creation of virtual editions.
To run creation of all virtial editions simply use create_virtual_editions
switch in command line. If you want to customize which editions will be created
when this switch is set, please use VIRTUAL_EDITIONS_LIST in configuration file.

Virtual editions creation can be only done when convert_ve_plugin is present in
the same directory as converter.

Thanks to abbodi1406 for providing informations which helped with creating this
option.

### Configuration file
Configuration of advanced script options can be modified using
the file `convert_config_linux` (on Linux) or `convert_config_macos` (on macOS).

###### Configuration options
```
VIRTUAL_EDITIONS_LIST='space delimited editions sequence'
```

###### Configuration options explanation
 * VIRTUAL_EDITIONS_LIST - configures which editions will be created when
   create_virtual_editions is enabled.

### Requirements
This script uses the following commands to do its work:
 * cabextract - to extract cabs
 * wimlib-imagex - to export files from metadata ESD
 * chntpw - to modify registry of first index of boot.wim
 * genisoimage or mkisofs - to create ISO image

###### Linux
If you use Debian or Ubuntu based distribution you can quickly install these
using the following command:

``` shell
sudo apt install cabextract wimtools chntpw genisoimage
```

For Arch Linux, requested dependencies can be installed by following command:

``` shell
sudo pacman -Sy --needed wimlib cabextract cdrtools chntpw
``` 

If you use any other distribution, then you will need to check its repository
for packages needed to run this script.

###### macOS
macOS requires [Homebrew](https://brew.sh) to install the prerequisite software.
After Homebrew was installed, you can install the requirements using:

``` shell
brew tap sidneys/homebrew
brew install cabextract wimlib cdrtools sidneys/homebrew/chntpw
```
