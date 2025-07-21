# Setup CLion on Windows

> Originally published: 15 November 2016

## Installation

* [Install CLion](https://www.jetbrains.com/clion/download/#section=windows-version).
* [Install Cygwin](https://cygwin.com/install.html).

---

## Preparing Cygwin

1. Run the Cygwin `setup-*.exe` file you downloaded.

2. If an Administration pop-up window appears. Click <kbd>Yes</kbd>.

3. Once the Cygwin Setup window appears. Click <kbd>Next</kbd>.

4. Select ***Install from Internet***. Click <kbd>Next</kbd>.

5. Select your Root Directory, I recommend using the root directory of a particular drive (for example `D:\cygwin64`)

6. Select Install for ***All Users***. Click <kbd>Next</kbd>.

7. Select your local package directory, I recommend creating a folder in your home folder or in your Downloads folder
   for this. Click <kbd>Next</kbd>.

8. Select ***Direct Connection***. Click <kbd>Next</kbd>.

9. Select your chosen mirror. I use `http://www.mirrorservice.org`. Click <kbd>Next</kbd>.

10. You will then be presented with a screen to select your packages to install. To select a packge for install, simply
	click on where it says "Skip" for that respective package.  
	You ***need***:  
	`cmake`, `cmake-gui`, `make`, `gcc-core`, `gcc-g++`, `gdb` (all from the *Devel* category).  
	I *recommend* also getting:  
	`vim`, `curl`, `wget`, `ssh`, `ncurses`.  
	Once done, click <kbd>Next</kbd>.

11. Deselect ***Create icon on Desktop***. Click <kbd>Finish</kbd>.

---

## Setting up CLion

You should already have installed CLion, now find the `CLion` program.

1. Select your theme. CLick <kbd>Next: Toolchains</kbd>.

2. Under ***Environment***, ensure the ***Cygwin*** option is selected.

3. Under ***CMake executable***, select ***Custom***, and type in the location of your `cmake.exe` inside the `bin`
   folder inside where you installed Cygwin. For example, if you installed Cygwin under `D:\cygwin64`, then you should
   use `D:\cygwin64\bin\cmake.exe`.

4. All of the tools at the bottom should load with tick symbols at the start. If any are red circles with an `i` in
   them, then you need to ensure that you installed all the correct Cygwin packages back in the Cygwin preparation
   stage. If all are ticks, Click <kbd>Next: Default Plugins</kbd>

5. Disable any plugins you want, I didn't disable any. Click <kbd>Next: Featured Plugins</kbd>

6. Install any featured plugins you want, I installed Lua, Markdown and Go as I may be using them in my development time
   using CLion. <kbd>Start using CLion</kbd>.

---

And you're done! Enjoy programming in C/C++ on your Windows machine!
