## Empowering your projectsNow that you can save your code on Github in a breeze, you can take advantage of servicesto automate actions. For example, to execute tests using [Travis](http://www.travis-ci.com)\) or [AppVeyor](https://www.appveyor.com) for Windows.### Adding Travis integrationBy adding two simple files, you can have the tests of your project automatically run after each commit with Travis. You need to enable Travis in your Github repository. Since Travis is changing its policy and work tightly in relation with Github you might have a better time checking on their respective websites.You should also add the two following files: `.travis.yml` and `.smalltalk.ston` in the top level of your repository.`.travis.yml````language: smalltalk
sudo: false
os:
  - linux
smalltalk:
  - Pharo-7.0````.smalltalk.ston````SmalltalkCISpec {
  #loading : [
    SCIMetacelloLoadSpec {
      #baseline : 'MyCoolProjectWithPharo',
      #directory : src',
      #platforms : [ #pharo ]
    }
  ]
}```If you have done everything right, Travis will pick up the changes and will start testing and building your project… and you are done, congratulations!### On windowsIf you want to make sure that your code runs on windows, you should use the Appveyor service and add the `appveyor.yml` file.```environment:
  CYG_ROOT: C:\cygwin
  CYG_BASH: C:\cygwin\bin\bash
  CYG_CACHE: C:\cygwin\var\cache\setup
  CYG_EXE: C:\cygwin\setup-x86.exe
  CYG_MIRROR: http://cygwin.mirror.constant.com
  SCI_RUN: /cygdrive/c/smalltalkCI-master/run.sh
  matrix:
    - SMALLTALK: Pharo-6.1
    - SMALLTALK: Pharo-7.0

platform:
  - x86

install:
  - '%CYG_EXE% -dgnqNO -R "%CYG_ROOT%" -s "%CYG_MIRROR%" -l "%CYG_CACHE%" -P unzip'
  - ps: Start-FileDownload "https://Github.com/hpi-swa/smalltalkCI/archive/master.zip" "C:\smalltalkCI.zip"
  - 7z x C:\smalltalkCI.zip -oC:\ -y > NULL

build: false

test_script:
  - '%CYG_BASH% -lc "cd $APPVEYOR_BUILD_FOLDER; exec 0</dev/null; $SCI_RUN"'```### Adding badgesWith CI happily running, you can add a badge to your `README` that will show the current status of your project.Here is the badge of the Containers-Stack project where we also enabled the `coveralls.io` test coverage service.```# Containers-Stack
A dead stupid stack implementation, but one fully working :)

[![Build Status](https://travis-ci.com/Ducasse/Containers-Stack.svg?branch=master)]
(https://travis-ci.com/Ducasse/Containers-Stack)
[![Coverage Status](https://coveralls.io/repos/Github//Ducasse/Containers-Stack/badge.svg?branch=master)]
(https://coveralls.io/Github//Ducasse/Containers-Stack?branch=master)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()
[![Pharo version](https://img.shields.io/badge/Pharo-7.0-%23aac9ff.svg)]
(https://pharo.org/download)
[![Pharo version](https://img.shields.io/badge/Pharo-8.0-%23aac9ff.svg)]
(https://pharo.org/download)

## Installation
The following script installs Containers-Stack in Pharo.

```smalltalk
Metacello new
  baseline: 'ContainersStack';
  repository: 'Github://Ducasse/Containers-Stack/src';
  load.
``````To obtain the necessary link, click on the badge in your Travis project overview and select one of the options. You can insert the markdown code directly into your README.md.### ConclusionWith a continuous integration setup, you make sure that you can run your software on different platforms.In addition you make sure that you will be able to smoothly load your code and that any addition or modifications you just did is not disrupting the whole project. Other services such as coveralls are available for Pharo.