+++
date = "2013-10-23T15:35:00-05:00"
title = "3rd party libraries and Android Studio Gradle issues"
categories = ["Android", "Android Studio", "Software Development"]
tags = ["Android", "Android Studio", "Software Development"]
slug = "2013/3rd-party-libraries-and-android-studio-gradle-issues"
+++

I want to start this off by saying that this was for Android Studio 0.2 and at the time of this post that is already outdated. However, I feel this might help others.

The project I was working on was currently using an older version of the Android Gradle plugin (0.4.2) with and older version of Android Studio. I had version 0.2 which requires the Android Gradle plugin minimum of 0.5.

I was getting gradle build errors, and Android Studio import errors. I needed to update my project `build.gradle` file as well as all the libraries. This meant I needed to go and edit the `build.gradle` file manually ([0.3 fixes this issue](http://tools.android.com/recent/androidstudio030released)), and change:

    dependencies {
      classpath 'com.android.tools.build:gradle:0.4.2'
    }

to

    dependencies {
      classpath 'com.android.tools.build:gradle:0.5'
    }

Doing this for the main app was not an issue, however some of the libraries I did not have access to like [ActionBarSherlock](https://github.com/JakeWharton/ActionBarSherlock). I had to fork the repo and update the gradle version.

For other projects like [SlidingMenu](https://github.com/jfeinstein10/SlidingMenu) I needed to fork it and setup a `build.gradle` file manually. This is my default [build.gradle](https://gist.github.com/joekr/7128972) file

Lastly, for [Volley](https://android.googlesource.com/platform/frameworks/volley/) I had to create a new repo and add the followling:

    buildscript {
      repositories {
          mavenCentral()
      }
      dependencies {
          classpath 'com.android.tools.build:gradle:0.5+'
      }
    }


I'm not sure if doing this is the best way, but it allowed my project to build and seems to be working. I have seen others doing the same thing, like [Path's ActionBarSherlock](https://github.com/path/ActionBarSherlock).

If I find a better way to do this you better believe I will post an update. This just seems gross to me.
