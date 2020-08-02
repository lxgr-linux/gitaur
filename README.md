# Buildaur -- An AUR helper
by LXGR <lxgr@protonmail.com>

# Introduction

An AUR helper? what's that? And what's the AUR in general?
The AUR is an software repository for archlinux users and their self created or packaged software which is not in an official repository. There the packages are not stored traditionaly as packages, like in a normal software repository. They are stored as 'PKGBUILD's which are used in a makeprocess to create and install the package itself.
An AUR helper is an program which does this job for you, just like a normal package manager like pacman.
Buildaur is one of those.
https://wiki.archlinux.org/index.php/Arch_User_Repository

# Installation

First the 'PKHBUILD' should be cloned from the AUR

$ git clone https://aur.archlinux.org/buildaur.git

Then the package has to be build and installed

$ cd ./buildaur
$ makepkg -si

Then the installation will be finished.

# Usage

Like most other shell programs buildaur has some options which can be used to specify what the program should exactly do:

Usage: /usr/bin/buildaur "<option> <string>"<br>
   General options:<br>
      -S                : Installs a package<br>
      -R                : Removes a package<br>
      -Q                : Lists installed packages or searches for ones in the AUR<br>
      -Qs               : Search the AUR<br>
      -Syu              : Updates all AUR packages<br>
      -url              : Installs a package from a given git-repository<br>
      -asp              : Builds a package from source using asp (usefull for archlinux arm)<br>
      --clear           : Cleanes build dir<br>
      -v|--version      : Displays the version of this program<br>
      -l|--license      : Displays the license of this program<br>
      --make-chroot     : Creates a chroot dir which can be used for building packages<br>

   Additional options for -S,-R,-Syu,-asp:<br>
      n                 : Doesn't ask questions<br>
      spgp              : Skips pgp checks of sourcecode<br>
      ch                : Builds the package in a clean chroot (you may run into some problems using this on archlinux arm!)<br>
      di                : Just builds the package<br>
      co                : Toggles colored output on and off<br>

   Hookoptions:<br>
      --listhooks       : Lists all available and installed hooks<br>
      --hook-activate   : Activates a hook<br>
      --hook-deactivate : Deactivates a hook<br>

   Help options:<br>
      -h|--help         : Displays this help-dialog<br>
      --help-hooks      : Displays help-dialog for hooks<br>

Examples:<br>
    buildaur -S cava    : Installes the package 'cava'<br>
    buildaur -Sn cava   : Installes 'cava' without asking any questions<br>
    buildaur -Syu       : Updates all packages which were installed from the AUR<br>

# Special options

-asp:
  The '-asp' option has in the first case nothing to do with the AUR itself, it's rather a fuction which is very important for Archlinux ARM.
  It builds a specified package, which may or may not be in the official Archlinux ARM repository, but is in the one for x86_64, completly from the source and installes it. This is usefull because some packages are not yet ported to ARM but may work.

hooks:
  Pre- and post-hooks are skripts that run before and after the package building.
  They are made to, for example modify the PKGBUILD.
  Pre- and post-run-hooks are skripts that run before the installation itself to fix dependency issues etc.
  All hooks are stored in /etc/buildaur/hooks.
  In /etc/buildaur/prehooks are the hooks stored wich run before the package building
  and in /etc/buildaur/posthooks those which run after the packagebuild.
  A hook always contains a 'type' (pre, post, prerun or postrun) and a 'desc' variable.
  It also contains an if-function which contains the modifications for the PKGBUILD etc.
  See /etc/buildaur/prehooks/1-arm-archfix as an example.

  Hookoptions:<br>
     --listhooks       : Lists all available and installed hooks<br>
     --hook-activate   : Activates a hook<br>
     --hook-deactivate : Deactivates a hook<br>

     When 'all' is given as argument for --hook-activate and --hook-deactivate the action applies to all hooks.

# Config file

The config file for buildaur is ' /etc/buildaur/buildaur.conf'. It can be used to set variables which appear in buildaur, like 'ask', 'pgp' and 'buildchroot'. It can also contain code that should be integrated into buildaur.

Available variables are:<br>
editor : Text-editor that will be used to edit the PKGBUILD. Default is 'nano'<br>
showPKGBUILD : Print out PKGBUILD before asking to edit it. Default is '1'<br>
sudoreset : Reset sudo before running PKGBUILD. Default is '1'. This may cause typing in your password more often, but is also more secure.<br>
ask_warn_inst : Ask you if you want to update packages wich higher local versions than aur versions.<br>
layout : Specifies the look of the program at updates. Options are 'old' and 'new'.<br>
color : Turn on and off colored output.<br>

# Blacklist

The blacklist is stored in /usr/share/buildaur/blacklist contains package names which are excluded in updates.
