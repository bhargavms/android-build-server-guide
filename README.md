# android-build-server-guide
Guide to building a proper android build server, for continous integration using Gitlab CI.

#INSTALLING JDK
Depending on compileSdkVersion you can compile java classes using jdk 7 or 8.
Thought the procedure for installing jdk 7 and 8 is same, except you download different files (i recommend installing jdk7)

## The manual way

- Download JDK 7 from [oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

   The file names follows below format

        jdk-7u<version>-linux-x64.tar.gz

   Where <version> is the update number currently the latest is 79.

   So for 32bit version 79 linux OS it would be `jdk-7u79-linux-x86.tar.gz` and 64 bit it would be `jdk-7u79-linux-x64.tar.gz`

   So download the jdk depending on your linux os being 32bit or 64bit and the version of your choice.

- Unzip the downloaded tar.gz file

        $ tar zxvf jdk-7u<version>-linux-x64.tar.gz

   This command untar(uncompresses) the compressed folder and its contents

   I would recommend unzipping it to `/usr/local/java`

- For gradle you need to add `JAVA_HOME` and `JRE_HOME` environment variables
   
   Do `sudo gedit /etc/profile` or if you don't have a GUI but are running a terminal only version of linux OS, then do `sudo nano /etc/profile` I'm assuming you know how to use the nano editor if not google!

   You need to add these lines at the bottom of the file

        JAVA_HOME=/usr/local/java/jdk1.7.0_79
        JRE_HOME=$JAVA_HOME/jre
        PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
        export JAVA_HOME
        export JRE_HOME
        export PATH

   press `ctrl+o` to write out, press `ctrl+x` to exit the nano editor.

   reboot the OS.

   (Taken from http://askubuntu.com/a/55960)
*   Now run

    <pre>sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.7.0_79/bin/java" 1
    sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.7.0_79/bin/javac" 1
    sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.7.0_79/bin/javaws" 1
    </pre>

    This will assign Oracle JDK a priority of 1, which means that installing other JDKs will [replace it as the default](http://askubuntu.com/q/344059/23678). Be sure to use a higher priority if you want Oracle JDK to remain the default.

*   Correct the file ownership and the permissions of the executables:

    <pre>sudo chmod a+x /usr/bin/java
    sudo chmod a+x /usr/bin/javac
    sudo chmod a+x /usr/bin/javaws
    sudo chown -R root:root /usr/lib/jvm/jdk1.8.0
    </pre>

    N.B.: Remember - Java JDK has many more executables that you can similarly install as above. `java`, `javac`, `javaws` are probably the most frequently required. This [answer lists](http://askubuntu.com/a/68227/14356) the other executables available.

*   Run

    <pre>sudo update-alternatives --config java
    </pre>

    You will see output similar to the one below - choose the number of jdk1.8.0 - for example `3` in this list (unless you have have never installed Java installed in your computer in which case a sentence saying "There is nothing to configure" will appear):

        $ sudo update-alternatives --config java
        There are 3 choices for the alternative java (providing /usr/bin/java).

          Selection    Path                                            Priority   Status
        ------------------------------------------------------------
          0            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   1071      auto mode
          1            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   1071      manual mode
        * 2            /usr/local/java/jdk1.7.0_79/bin/java             1         manual mode
          3            /usr/lib/jvm/jdk1.8.0/bin/java                   1         manual mode

        Press enter to keep the current choice[*], or type selection number: 3
        update-alternatives: using /usr/lib/jvm/jdk1.8.0/bin/java to provide /usr/bin/java (java) in manual mode

    Repeat the above for:

        sudo update-alternatives --config javac
        sudo update-alternatives --config javaws

#INSTALLING GRADLE

*   [Download](http://gradle.org/gradle-download/) either the gradle binary-only/all distribution, dont download the gradle source code distribution as it would require you to build the source.

* Extract the zip file and add the environment variable GRADLE_HOME=your gradle installation home folder, in my case i unzipped it to /usr/local/gradle/grade-2.10

* Next add the path GRADLE_HOME/bin to the environment variable PATH by editing the /etc/profile file.

        sudo  nano /etc/profile

   Add the the below lines similarly like adding the java env variables to profile.

        GRADLE_HOME=/usr/local/gradle/gradle-2.10
   Edit the below line with PATh to look like this

        PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$GRADLE_HOME/bin

   Then press `ctrl+o` to write out (save) `ctrl+x` to exit nano editor.

Done you have successfully installed gradle.
