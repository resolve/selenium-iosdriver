# Selenium iOS Driver

Currently to build the iOS web driver for selenium you must download all the
selenium source code
(see https://code.google.com/p/selenium/wiki/IPhoneDriver).

This means a ~450MB download just to compile and run the iOS driver. For the
purposes of simplified distribution this project allows distribution of just
the essential files to compile the app (closer the 20MB total).

Under the `driver/` directory is the latest version distributed in this repo, to
update it there is a rake task that will pull down the full SVN repo into a
hidden directory (`.svn-copy/`) run a build task (`./go iphone_atoms`) and then
copy the necessary files into the `driver/` directory. To run this call:
```
$ rake update
```

If you want to update to a particular tag in the Selenium SVN repository:
```
$ rake update TAG=selenium-2.25.0
```

Output from these commands will be something like:
```
$ rake updateChecking out the latest revision at http://selenium.googlecode.com/svn/trunk/.
If this is the first time it may take several minutes.
Successfully checked out revision '17708' from SVN.
Successfully generated atoms.h for iOS.
Successfully copied files into repo.

Update complete.
```

Once the update is complete the `driver/` directory is managed by GIT so you
can review the changes and commit these.
Our approach has been to mention the SVN version in the commits also tagged these commits like so:
 * [selenium-2.25.0](https://github.com/resolve/selenium-iosdriver/tree/selenium-2.25.0)
 * [selenium-r17708](https://github.com/resolve/selenium-iosdriver/tree/selenium-r17708)
This makes it easier to track which SVN version is committed.