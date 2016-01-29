# android-build-server-guide
Guide to building a proper android build server, for continous integration using Gitlab CI.

#INSTALLING JDK
(Taken from http://askubuntu.com/a/55960)

##Install Java JDK

## The manual way

*   [Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html) the 32-bit or 64-bit Linux "compressed binary file" - it has a ".tar.gz" file extension.

*   Uncompress it

    `tar -xvf jdk-8-linux-i586.tar.gz` (32-bit)

    `tar -xvf jdk-8-linux-x64.tar.gz` (64-bit)

    The JDK 8 package is extracted into `./jdk1.8.0` directory. N.B.: Check carefully this folder name since Oracle seem to change this occasionally with each update.

*   Now move the JDK 8 directory to `/usr/lib`

    <pre>sudo mkdir -p /usr/lib/jvm
    sudo mv ./jdk1.8.0 /usr/lib/jvm/
    </pre>

*   Now run

    <pre>sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0/bin/java" 1
    sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0/bin/javac" 1
    sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0/bin/javaws" 1
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
        * 2            /usr/lib/jvm/jdk1.7.0/bin/java                   1         manual mode
          3            /usr/lib/jvm/jdk1.8.0/bin/java                   1         manual mode

        Press enter to keep the current choice[*], or type selection number: 3
        update-alternatives: using /usr/lib/jvm/jdk1.8.0/bin/java to provide /usr/bin/java (java) in manual mode

    Repeat the above for:

        sudo update-alternatives --config javac
        sudo update-alternatives --config javaws

#INSTALLING GRADLE

*   [Download](http://gradle.org/gradle-download/) either the gradle binary-only/all distribution, dont download the gradle source code distribution as it would require you to build the source.

* Extract the zip file and add the environment variable GRADLE_HOME=your gradle installation home folder

* Next add the path GRADLE_HOME/bin to the environment variable PATH by editing the /etc/profile file.