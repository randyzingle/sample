# Simple adventure game

## phase1a - Initial setup 

Note, this README and all subsequent ones will be written using [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

### Download and install the tools that you'll need (Linux or Mac box only!)
1. [Eclipse Oxygen Release (4.7.0)](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/oxygenr)
2. [Gradle](https://gradle.org/install/)
3. Use the Eclipse Marketplace (under Help) to install the Buildship gradle plugin
4. [Git](https://git-scm.com/downloads)

### Create a new project from scratch
First we will act as if we are creating a project from scratch so that we can see how its done.

Log onto you Linux or Mac box, create a working directory under your home directory (something like work/projects/adventure) then cd into that directory and run `gradle init --type java-library`: 

```shell
anduril$ pwd
/home/rzingle/work/projects/adventure
anduril$ gradle init --type java-library
:wrapper
:init

BUILD SUCCESSFUL

Total time: 0.543 secs
anduril$ ls
build.gradle  gradle  gradlew  gradlew.bat  settings.gradle  src

```
As you can see this has created a few files and directories. 

#### src directory
It's created a src directory that has the standard structure for Java projects, a `src/main/java` folder for java code, and a `src/main/resources` folder for resources (text files, images, properties, etc). It's also created a sample Java class and test file. 

```shell
anduril$ ls -r src
test  main
anduril$ ls -R src
src:
main  test

src/main:
java

src/main/java:
Library.java

src/test:
java

src/test/java:
LibraryTest.java
```

#### build.gradle and gradlew
`build.gradle` is the build file that describes all of the libraries that your project will use. The `gradlew` file is a script that runs gradle and builds whatever the build.gradle file tells it to build. Below is the raw file:

```shell
anduril$ cat build.gradle
/*
 * This build file was generated by the Gradle 'init' task.
 *
 * This generated file contains a sample Java Library project to get you started.
 * For more details take a look at the Java Libraries chapter in the Gradle
 * user guide available at https://docs.gradle.org/3.4/userguide/java_library_plugin.html
 */

// Apply the java-library plugin to add support for Java Library
apply plugin: 'java-library'

// In this section you declare where to find the dependencies of your project
repositories {
    // Use jcenter for resolving your dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
}

dependencies {
    // This dependency is exported to consumers, that is to say found on their compile classpath.
    api 'org.apache.commons:commons-math3:3.6.1'

    // This dependency is used internally, and not exposed to consumers on their own compile classpath.
    implementation 'com.google.guava:guava:20.0'

    // Use JUnit test framework
    testImplementation 'junit:junit:4.12'
}
```

If you run `gradlew tasks` you will see the build tasks that are available to you.

We're going to remove the default dependencies and add the **eclipse** plugin which will allow us to easily import our project into Eclipse:

```shell
anduril$ cat build.gradle
/*
 * This build file was generated by the Gradle 'init' task.
 *
 * This generated file contains a sample Java Library project to get you started.
 * For more details take a look at the Java Libraries chapter in the Gradle
 * user guide available at https://docs.gradle.org/3.4/userguide/java_library_plugin.html
 */

// Apply the java-library plugin to add support for Java Library
apply plugin: 'java-library'
apply plugin: 'eclipse'

// In this section you declare where to find the dependencies of your project
repositories {
    // Use jcenter for resolving your dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
}

dependencies {
    // Use JUnit test framework
    testImplementation 'junit:junit:4.12'
}
```

If you run `gradlew tasks` again then you'll see the eclipse-specific tasks:

```shell
...
IDE tasks
---------
cleanEclipse - Cleans all Eclipse files.
eclipse - Generates all Eclipse files.
...
```

Let's generate the eclipse project files now by running `gradlew eclipse`

```shell
anduril$ ls -al
total 40
drwxrwxr-x  5 rzingle rzingle 4096 Aug  6 15:41 .
drwxrwxr-x 10 rzingle rzingle 4096 Aug  6 15:21 ..
-rw-rw-r--  1 rzingle rzingle  678 Aug  6 15:56 build.gradle
drwxrwxr-x  3 rzingle rzingle 4096 Aug  6 15:41 gradle
drwxrwxr-x  4 rzingle rzingle 4096 Aug  6 15:32 .gradle
-rwxrwxr-x  1 rzingle rzingle 5299 Aug  6 15:41 gradlew
-rw-rw-r--  1 rzingle rzingle 2260 Aug  6 15:41 gradlew.bat
-rw-rw-r--  1 rzingle rzingle  582 Aug  6 15:41 settings.gradle
drwxrwxr-x  4 rzingle rzingle 4096 Aug  6 15:41 src

anduril$ gradlew eclipse
:eclipseClasspath
:eclipseJdt
:eclipseProject
:eclipse

BUILD SUCCESSFUL

Total time: 0.696 secs

anduril$ ls -al
total 52
drwxrwxr-x  6 rzingle rzingle 4096 Aug  6 15:59 .
drwxrwxr-x 10 rzingle rzingle 4096 Aug  6 15:21 ..
-rw-rw-r--  1 rzingle rzingle  678 Aug  6 15:56 build.gradle
-rw-rw-r--  1 rzingle rzingle  357 Aug  6 15:59 .classpath
drwxrwxr-x  3 rzingle rzingle 4096 Aug  6 15:41 gradle
drwxrwxr-x  4 rzingle rzingle 4096 Aug  6 15:32 .gradle
-rwxrwxr-x  1 rzingle rzingle 5299 Aug  6 15:41 gradlew
-rw-rw-r--  1 rzingle rzingle 2260 Aug  6 15:41 gradlew.bat
-rw-rw-r--  1 rzingle rzingle  361 Aug  6 15:59 .project
drwxrwxr-x  2 rzingle rzingle 4096 Aug  6 15:59 .settings
-rw-rw-r--  1 rzingle rzingle  582 Aug  6 15:41 settings.gradle
drwxrwxr-x  4 rzingle rzingle 4096 Aug  6 15:41 src
```

Now we have the files Eclipse needs to import our project (.classpath, .project, and the .settings directory). 

From Eclipse do the following:

File -> Import -> General -> Existing Projects into Workspace

Select root directory (for me it's /home/rzingle/work/projects/adventure)
Check Options -> Search for nested projects

Right Click on the Project Name (adventure)
Configure -> Add Gradle Nature

We should have the project imported but the test is not compiling because it can't find the JUnit library

Right Click on the Project Name (adventure)
Gradle -> Refresh Gradle Project

This will download the Jar files that we listed in our dependencies section (junit-*.jar and hamcrest-core-*.jar)
Our compilation errors should be gone now.

Let's also add some Gradle specific Views to our Eclipse window. 

Window -> Show View -> Other -> Gradle -> (both) Gradle Executions & Gradle Tasks

Right Click on the Project Name (adventure)
Gradle -> Refresh Gradle Dependencies


#### Git

Create a **.gitignore** file which will tell git which files we don't want stored in our repository.

```shell
anduril$ pwd
/home/rzingle/work/projects/courses/adventure
anduril$ cat .gitignore 
node_modules/
*.class
*~
.DS_Store
.metadata/
.settings/
RemoteSystemsTempFiles/
.gradle/
build/
libs/
.recommenders
bin/
.classpath
.project
```

Now make our folder **git-enabled**. The full instructions for this are here:
[git initialization](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)

First set up a new repository in github and then use the repo name below replacing **REPLACE WITH YOUR GIT REPO** with the
repo name. For instance, for the below I replace it with **randyzingle/sample.git**

```shell
anduril$ pwd
/home/rzingle/work/projects/courses/adventure
anduril$ git init
Initialized empty Git repository in /home/rzingle/work/projects/courses/adventure/.git/
anduril$ git add .
anduril$ git commit -m 'first commit'
[master (root-commit) b651893] first commit
 8 files changed, 539 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100644 build.gradle
 create mode 100755 gradlew
 create mode 100644 gradlew.bat
 create mode 100644 settings.gradle
 create mode 100644 src/main/java/Library.java
 create mode 100644 src/test/java/LibraryTest.java
anduril$ git remote add origin git@github.com:REPLACE WITH YOUR GIT REPO
anduril$ git remote -v
origin	git@github.com:randyzingle/sample.git (fetch)
origin	git@github.com:randyzingle/sample.git (push)
anduril$ git branch --set-upstream-to=origin/master master
anduril$ git push origin master 
Counting objects: 15, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (15/15), 7.34 KiB | 0 bytes/s, done.
Total 15 (delta 0), reused 0 (delta 0)
To https://github.com/randyzingle/sample.git
 * [new branch]      master -> master
```

Note if you added the remote as https://github.com/randyzingle/sample.git vs git@github.com:randyzingle/sample.git you will **continually** get asked for your username and password every time to try to push code to the remote repository. That's no fun so make sure you use **ssh** vs **https**. This only works if you added your ssh key to the repo, if you haven't yet follow the instructions here: [add our ssh key to the repo] (https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).

Now pushing and pulling from the remote should be easy:

```shell
anduril$ git add .
anduril$ git commit -m 'first commit'
[master 40b4d43] first commit
 1 file changed, 6 insertions(+), 9 deletions(-)
anduril$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 634 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:randyzingle/sample.git
   34f5d18..40b4d43  master -> master
```

### Share or Use the new project from git

Create a new empty directory and clone the project into it:

```shell
anduril$ mkdir temp
anduril$ cd temp
anduril$ pwd
/home/rzingle/work/projects/temp
anduril$ git clone git@github.com:randyzingle/sample.git
anduril$ ls
sample
anduril$ cd sample
anduril$ ls
build.gradle  gradle  gradlew  gradlew.bat  README.md  settings.gradle  src
anduril$ ls -al
total 56
drwxrwxr-x 5 rzingle rzingle 4096 Aug  6 17:16 .
drwxrwxr-x 3 rzingle rzingle 4096 Aug  6 17:16 ..
-rw-rw-r-- 1 rzingle rzingle  750 Aug  6 17:16 build.gradle
drwxrwxr-x 8 rzingle rzingle 4096 Aug  6 17:16 .git
-rw-rw-r-- 1 rzingle rzingle  142 Aug  6 17:16 .gitignore
drwxrwxr-x 3 rzingle rzingle 4096 Aug  6 17:16 gradle
-rwxrwxr-x 1 rzingle rzingle 5299 Aug  6 17:16 gradlew
-rw-rw-r-- 1 rzingle rzingle 2260 Aug  6 17:16 gradlew.bat
-rw-rw-r-- 1 rzingle rzingle 9441 Aug  6 17:16 README.md
-rw-rw-r-- 1 rzingle rzingle  582 Aug  6 17:16 settings.gradle
drwxrwxr-x 4 rzingle rzingle 4096 Aug  6 17:16 src
```

Note the Eclipse `.classpath` and `.project` files aren't there yet. Let's build them then import the project into Eclipse:

```shell
anduril$ gradlew eclipse
:eclipseClasspath
:eclipseJdt
:eclipseProject
:eclipse

BUILD SUCCESSFUL

Total time: 0.5 secs
``` 

And once again you have a project that you can import into eclipse.





