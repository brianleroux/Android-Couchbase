## Mobile Couchbase for Android

Apache CouchDB on Android provides a simple way to sync your application data across devices and provide cloud backup of user data. Unlike other cloud solutions, the data is hosted on the device by Couchbase Mobile, so even when the network is down or slow (airplane, subway, backyard) the application is responsive to users.

What this means for you:

* You can embed the rock solid distributed database, Mobile Couchbase, on your Android device.
* Your Android apps can use Apache CouchDB's well-proven synchronization technology.
* If you <3 CouchApps, you can deploy them as Android apps.

### Beta Release

If you just want to get started, jump to **Getting Started**.

The biggest thing we need help with is size optimization - currently a Release build adds about 7 MB to your application. We are targeting 5 MB for our initial round of optimizations. It can definitely go lower but that work might take longer.

## Join us

There is a [Google Group here for Mobile Couchbase](https://groups.google.com/group/mobile-couchbase). Let's talk about how to optimize the Erlang build, what the best Java APIs are for CouchDB, how to take advantage of replication on mobile devices. It'll be fun.


## Getting Started

If you have questions or get stuck or just want to say hi, email <mobile@couchbase.com> and let us know that you're interested in Couchbase on mobile.

These instructions require you to have installed Eclipse and the Android SDK, the [Android developer website](http://developer.android.com/sdk/installing.html) has instructions.

## Running the Demo app

### Get the main repository

    git clone --recursive git://github.com/couchbaselabs/Android-Couchbase.git

Import both the libcouch-android and Android-Demo projects into your workbench with File -> Import -> General -> Existing Projects into Workspace and browse to the location you checked out Android-Couchbase into.

You will need to do a clean build, Project -> Clean and choose both libcouch-android CouchApp

You should now be able to run CouchApp by right clicking on the CouchApp project and choosing Run As -> Android Application

## Building a new application

Start a new Android project, right click your new project -> Properties -> Android and on the lower libraries panel press "Add" and select the LibCouch project

back in the properties dialog pick Java Build Path -> Libraries and Add JARS, navigate to LibCouch/lib and choose both commons-compress-1.0.jar and commons-io.2.0.1.jar

Inside your AndroidManifest.xml you will need the following definitions inside the <manifest> tag
    <uses-permission android:name="android.permission.INTERNET"></uses-permission>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
The following code is what is needed in your application to start Couchbase as a service

as well as

        <service android:name="com.couchbase.libcouch.CouchService"
                 android:enabled="true"
                 android:exported="false"></service>

inside your <application> tag, then the following code is used inside your application to start Couchbase

    private final ICouchClient mCallback = new ICouchClient.Stub() {
      @Override
      public void couchStarted(String host, int port) {}

      @Override
      public void installing(int completed, int total) {}

      @Override
      public void downloading(int completed, int total) {}

      @Override
      public void exit(String error) {}
    };

   String url = "https://github.com/downloads/couchbaselabs/Android-Couchbase/";
   String release = "release-0.1";
   couchServiceConnection = CouchDB.getService(getBaseContext(), url, release, mCallback);

for example of all of these you can check inside the Android-Demo folder

## Build information

The current build of Android Couchbase downloads the binaries from github during the installation. There is information on how to build these binaries on the [build-android-couch](https://github.com/couchbaselabs/build-android-couch) project.

## License

Portions under Apache, Erlang, and other licenses.

The overall package is released under the Apache license, 2.0.

Copyright 2011, Couchbase, Inc.
