Apple2BuildPipeline
===================

A build pipeline for making 8-bit Apple II software on OS X using cc65, Applesoft BASIC or Merlin32.

Features:
=========

This project was built based on the one created by [Quinn Dunki](http://quinndunki.com/blondihacks/?p=1904) but it has been expanded to add the following features:

   * You can create Applesoft BASIC, ca65 assembly, cc65 C and Merlin32 assembly projects.
   * Attempts to hide all of the infrastructure which you don't need to modify in a make directory.
   * On Mac OS X, Virtual II will start when you do a "build and run" and the emulator will execute your program.  This works for all supported Apple configurations with cc65 based projects.  It also works with Applesoft and Merlin32 based projects.  Also, it uses a hardware configuration which is stored in the make directory in your project.  That way, you can modify the hardware configuration to be exactly the config you want to test under.
   * The Mac OS X deliverable is now an installer which will create the Xcode project templates.  So, to start a new Apple II project in Xcode, select File->New->Project.  Under the "Cross-platform" tab, you will see an "Apple II" section with the different project types.
   * Code completion works now in Xcode.  You can even complete functions from the standard library in cc65 based projects and jump to those function definitions in the headers.  Code completion also works in Applesoft and Merlin32 projects.
   * Syntax colouring is supported for all project types including assembly files and Applesoft BASIC files.
   * The Makefile template provides a gen phase which executes before all compiles.  If you want to generate code or other data files in your project, this is supported.
   * You can specify one or more directories in your project which contain files you want to copy to the disk image at the end of the build.  So, if you have files beyond the executable itself which need to be on the disk image, you can use this feature to do so.

With cc65 based projects, the following features are supported:
   * Supports linking together multiple C and assembly files.  To add a new file to the project, just create a new *.c or *.s file in the project directory.
   * If you change a header file, the right source files will rebuild automatically.  Header file dependencies are generated during the build.
   * Supports all cc65 Apple configurations.  So, if you want to target enhanced Apple IIe's running DOS 3.3, knock yourself out.  Just set the configuration you want in the Makefile and the build will do the right thing to create a disk image for that configuration.
   * Build errors from the compiler, assembler or linker will be displayed inline in your source code next to the problematic line.
   * You can easily add drivers from the cc65 project by uncommenting a single line in the template Makefile.  Drivers for lores graphics, hires graphics, aux memory, joysticks, mouse and serial devices are supported.


Mac OS X Installation:
----------------------

In order to use this infrastructure from Mac OS X, follow these instructions:
   1. Install [Xcode from Apple](https://developer.apple.com/xcode/).  Xcode is generally the most popular app in the Mac App Store in the "Developer Tools" category.  Xcode is free and you do not need to be a registered Apple developer to download and use it, especially if you are building Apple II programs.  I am generally running the latest version of Xcode so if you are trying to use these templates with an old version, you may run into problems.
   2. Install the [Apple II project templates](https://github.com/jeremysrand/Apple2BuildPipeline/releases/download/2.7.4/AppleXcodeTemplate.pkg).
   3. Install and setup [Virtual II](http://www.virtualii.com).
   4. If you plan to use cc65 based projects, install the [cc65 v2.19 package](https://github.com/jeremysrand/Apple2BuildPipeline/releases/download/2.7.4/cc65-2.19.pkg).  Both versions should work with the latest project templates but all of my testing from now on will be with the newer version.  I haven't tested without cc65 installed but in theory, if you only plan to use Applesoft or Merlin32 based projects, you don't need to install cc65.
   5. If you plan to use Merlin32 based projects, install the [Merlin32 binaries and libraries](https://www.brutaldeluxe.fr/products/crossdevtools/merlin/).  By default, the build system assumes you have installed the Merlin32 binary in /usr/local/bin and put the library macro files in /usr/local/lib/Merlin.  If you installed them somewhere else, you can override these location in your projects.

Note that on your first launch of Xcode after installing the project templates, you may be asked if it is OK to load a cc65 plug-in bundle.  You should allow this to load.  If you do not do this, syntax highlighting and code completion will not work in assembly or Applesoft files.


Your First cc65 Based Project:
------------------------------

To create a new cc65 based project for the Apple II in Xcode:
   1. Start Xcode and create a new project by using File->New->Project...
   2. In the dialog, you will see an "Apple II" option below the Cross-platform section.  Select "Apple II cc65 Project" for a C based project.  Or select "Apple II ca65 Project" for an assembly project.  Click "Next".
   3. A dialog box with a few text fields will appear.  In product name, put in the name of the Apple II executable you want to build.  Organization Name and Organization Identifier can be anything you want it to be.  Leave Build Tool set to "/usr/bin/make".  Click "Next".
   4. Xcode now prompts you where you want to save your project.  The name of the project will be the product name you already gave.  Pick a good directory for your project.  Your Documents folder is a reasonable option.  Click "Create".
   5. Your project is now ready for you.  If you select Product->Build, it will build your code.  If you select Product->Run, it will build and execute the template code in Virtual II.  On your first build, you may be prompted by Mac OS X to install a Java runtime.  Java is required so if you get this prompt, you should install it.  Once Java is installed, go back to Xcode and select Product->Clean and then Product->Run.  Virtual II should launch this time.
   6. At this point everything should work and you should see "HELLO, WORLD!" in the emulator.  Press any key in the emulator to quit the executable and go back to the BASIC prompt.
   7. Review the Makefile and set any options you want.  The file has lots of comments to help you understand the configuration options.
   8. Change main.c (or main.s if you created an assembly project) and write more code in new C or assembly files until you have the program you always wanted to build.  To add new files, select File->New->File.  In the dialog, you will see an Apple II section in the macOS tab.  In there, you will see options to create a new "C File" or a new "ca65 Assembly File".  Select the one you want to add the file to your project.  Put the new file in the same directory as Makefile.  You can add assembly files in a C project or add C files in an assembly project.  The only difference between them is the type of the default source file in the project template.
   9. Do not add a "Merlin Assembly File" to your project.  You cannot link Merlin files in a cc65 based project.
   10. You can add a "Basic File" to your project.  Any Applesoft files added to the project will be tokenized during the build and will be copied to the disk image.  But when you build and run, the cc65 binary will be executed.


Your First Applesoft BASIC Project:
-----------------------------------

To create a new Applesoft BASIC project for the Apple II in Xcode:
   1. Start Xcode and create a new project by using File->New->Project...
   2. In the dialog, you will see an "Apple II" option below the Cross-platform section.  Select "Apple II Basic Project" for an Applesoft project.  Click "Next".
   3. A dialog box with a few text fields will appear.  In product name, put in the name of the Apple II program you want to build.  Organization Name and Organization Identifier can be anything you want it to be.  Leave Build Tool set to "/usr/bin/make".  Click "Next".
   4. Xcode now prompts you where you want to save your project.  The name of the project will be the product name you already gave.  Pick a good directory for your project.  Your Documents folder is a reasonable option.  Click "Create".
   5. Your project is now ready for you.  If you select Product->Build, it will tokenize your BASIC code.  If you select Product->Run, it will tokenize and execute the template code in Virtual II.  On your first build, you may be prompted by Mac OS X to install a Java runtime.  Java is required so if you get this prompt, you should install it.  Once Java is installed, go back to Xcode and select Product->Clean and then Product->Run.  Virtual II should launch this time.
   6. At this point everything should work and you should see "HELLO, WORLD!" in the emulator.
   7. Review the Makefile and set any options you want.  The file has lots of comments to help you understand the configuration options.  Definitely check out the optimization option for BASIC programs.
   8. Change the file containting the Applesoft code until you have the program you always wanted to build.
   9. You can add other C and assembly files to your projects but they will be ignored.  Today, there is no way to add a binary built by cc65 or Merlin32 and include that in a BASIC project (let me know if you wan this).  You can add BASIC files to your cc65 and Merlin32 based projects though.
   10. You can add a second, third, etc "Basic File" to your project.  Any Applesoft files added to the project will be tokenized during the build and will be copied to the disk image.  But when you build and run, only the original BASIC file will be executed.


Your First Merlin32 Based Project:
----------------------------------

To create a new Merlin32 based project for the Apple II in Xcode:
   1. Start Xcode and create a new project by using File->New->Project...
   2. In the dialog, you will see an "Apple II" option below the Cross-platform section.  Select "Apple II Merlin Project".  Click "Next".
   3. A dialog box with a few text fields will appear.  In product name, put in the name of the Apple II executable you want to build.  Organization Name and Organization Identifier can be anything you want it to be.  Leave Build Tool set to "/usr/bin/make".  Click "Next".
   4. Xcode now prompts you where you want to save your project.  The name of the project will be the product name you already gave.  Pick a good directory for your project.  Your Documents folder is a reasonable option.  Click "Create".
   5. Your project is now ready for you.  If you select Product->Build, it will build your code.  If you select Product->Run, it will build and execute the template code in Virtual II.  On your first build, you may be prompted by Mac OS X to install a Java runtime.  Java is required so if you get this prompt, you should install it.  Once Java is installed, go back to Xcode and select Product->Clean and then Product->Run.  Virtual II should launch this time.
   6. At this point everything should work and you should see "HELLO, WORLD!" in the emulator.  Press any key in the emulator to quit the executable and go back to the BASIC prompt.
   7. Review the Makefile and the linkscript.s file and set any options you want.  Remember that this is a project for 8-bit Apple II's so don't enable features which produce Apple IIgs binaries.
   8. Change main.s and write more code in new assembly files until you have the program you always wanted to build.  To add new files, select File->New->File.  In the dialog, you will see an Apple II section in the macOS tab.  In there, you will see options to create a new "Merlin Assembly File".  Put the new file in the same directory as Makefile.  You also need to add the new file to the linkscript.s file to ensure that Merlin assembles its contents.
   9. Note that the Merlin assembly files contain "]XCODESTART" and "]XCODEEND" variables defined at the beginning and the end of every file.  If you remove those, your build will still work.  These markers are here to convince Xcode that the code is the body of a function.  I have told Xcode that a function in Merlin assembly start and end with these tokens.  With this in place, we get better indentation handling.  If you remove them, things will work but the editor may drive you crazy.  If I can find a way to solve this without this Xcode cruft in your files, I will implement that in the future.
   10. Do not add a "C File" or "ca65 Assembly File" to your project.  You cannot link cc65 files in a Merlin based project.
   11. You can add a "Basic File" to your project.  Any Applesoft files added to the project will be tokenized during the build and will be copied to the disk image.  But when you build and run, the binary produced by Merlin32 will be executed.


Common Problems:
----------------

There are some known issues which can crop up:
   * If you aren't seeing the "ca65 Assembly" option under Editor->Syntax Coloring, you may be having an Xcode compatibility problem.  These syntax colouring files are provided as part of an Xcode plugin and plugins must advertize their compatibility.  During install, the plugin is set to be compatible with the version of Xcode you have.  If you upgrade Xcode, the plugin will be assumed to be incompatible and will not be loaded.  To workaround this, we force the plugin to be marked as compatible on every build of a Apple II target.  So, you should do a build and then quit and re-launch Xcode.  You should see a warning asking if you want to load the plugin and if you allow the plugin to load, you should see the ca65 syntax colouring option.

If these suggestions do not help or you are having some other problem, please contact me and I will try to help you out.


Upgrading Existing Projects:
----------------------------

As these templates evolve, you may have projects you started under older versions of the project templates which you will want to upgrade.  The best method for doing this is:
   1. Create a brand new project with the same name in Xcode and select the appropriate project type.
   2. Review your changes to the Makefile in the old project.  Apply the appropriate changes to the Makefile in the new project to pick up those customizations.
   3. Copy all of the code from your old project into your new project.  You should be able to drag it into the Xcode sidebar.  Be sure to copy main.c or main.s also.
   4. Test the project.  It should build and run at this point if you have duplicated everything.

It is possible to upgrade projects in other ways like copying the make directory from a new project into the old project.  The contents of the make directory is where much of the functionality is located so this picks up most things but not necessarily everything.  The preferred approach is to create a new project and migrate your code into that project.


Acknowledgements:
-----------------

Thanks to Quinn Dunki and Carrington Vanston who were instrumental in getting this working.  [Carrington's presentation](http://monsterfeet.com/kfest/) at KansasFest 2014 is where the basic concepts of a better Apple II build environment were demonstrated.  [Quinn built on that](http://quinndunki.com/blondihacks/?p=1904) and included support for automatically starting the program in Virtual II using Applescript.  Without their work, I wouldn't have been able to create this.  Thanks!

Kris Kennaway provided some patches to the build pipeline providing cc65 v2.17 support.  This was instrumental in getting v2.6 of the build pipeline released.  I appreciate the support.
