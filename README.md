This repository consists of the following Java Maven projects:
* ***perfecto-mobile-browser*** - A standalone Java application, implementing a web browser which allows interacting with Perfecto Mobile's "Mobile Cloud" web application views - "Perfecto Lab" and "Perfecto Dashboard". The application also receives the  generated **executionId** after logging-in to the Perfecto app, stores it in memorey and serves it to other applications via a TCP socket.
* ***jsystem-perfecto-plugin*** - Adding this plugin to JSystem allows adding Perfecto Mobile's tab inside the JSystem runner GUI. The plugin allows to configure the Perfecto cloud properties (host, username, password), and launching the **perfecto-mobile-browser** application.
* ***jsystem-perfecto-infra*** - A JSystem SO project, containing the needed dependencies and classes for interacting with the Perfecto cloud and with the **perfecto-mobile-browser** application, to read the current **executionId**.
* ***jsystem-perfecto-tests*** - A sample JSystem-Perfecto tests project. The project depends on the **jsystem-perfecto-infra** project, and contains the `JSystemPerfectoTest` class, responsible for intializing the communication with the Perfecto cloud.

#perfecto-mobile-browser
![Perfecto Mobile "Lab" and "Dashboard" browsers](https://github.com/ronyb/jsystem-perfecto-plugin/blob/master/images/perfecto_browsers.png)

###Description and use

This is a standalone Java GUI application, based on the [SWT library](https://www.eclipse.org/swt/). The application can be used on its own, or it could be launched from Perfecto's tab inside JSystem runner.

To launch the application, double click on the `perfecto-mobile-browser-<version>.jar` file, or launch it from the command line by typing: `java -jar perfecto-mobile-browser-<version>.jar`.

When launching the application without providing command line arguments, two browser windows are opened: "Perfecto Lab" and "Perfecto Dashboard" - each opens a different URL on the Perfecto Mobile Cloud.

It is possible to launch just one browser - "Perfecto Lab" or "Perfecto Dashboard" by adding a single command line argument:

For "Perfecto Lab": `java -jar perfecto-mobile-browser-<version>.jar lab`<br/>
For "Perfecto Dashboard": `java -jar perfecto-mobile-browser-<version>.jar dashboard`

####perfecto.properties
The application depends on the `perfecto.properties` file which should be located inside the same directory as the JAR file.
The `perfecto.properties` file is used to configure the URL of the Perfecto Cloud and the username and password which would be used by the application to perform automatic log-in inside the web page, by invoking the `doExternalLogin` JavaScript function on the client-side.

####Reading the executionId
The core feature of the application is to communicate with the client-side JavaScript code of the Perfecto application. The application receives and stores the **executionId** that's created after logging-in to "Perfecto Lab". The executionId is stored in memory, and can later be queried by other applications, e.g., code running from a JSystem test.

When launched, the application runs a TCP server, listening on port 3456. The server supprts the following requests from clients:<br>
`getExecutionId` - returns the current executionId<br>
`getHost` - returns the the current Perfeco cloud host, as was configured in the `perfecto.properties` file<br>

###How to build and deploy
This is a Maven project. To build it, run the following command line: `mvn clean install` from the project root directory. After a successful build, all artifacts will be found inside the `target` directory at the project's root.

The important artifacts are:
* `perfecto-mobile-browser-<version>.jar`
* `perfecto.properties`
* `perfecto_icon.png`
* `lib` (directory)

To make the **perfecto-mobile-browser** available from the JSystem Perfecto Mobile plugin tab, copy all the artifacts listed above into a new directory named `perfecto`, and put this directory at the root of your JSystem runner.

#jsystem-perfecto-plugin
![JSystem Perfecto Mobile Tab](https://github.com/ronyb/jsystem-perfecto-plugin/blob/master/images/jsystem_perfecto_tab.png)

###Description and use

This is a plugin for the JSystem runner, which allows adding a "Perfecto Mobile" tab to the JSystem runner GUI. Once installed, the Perfecto tab allows the user to configure the Perfecto Cloud properties (host, username, password), and to launch the **perfecto-mobile-browser** in "Lab" and/or "Dashboard" views, by clicking the appropreate button.

Before launching the **perfecto-mobile-browser**, the application validates that all Perfecto cloud properties fields are not empty.

When clicking the "Open Perfecto Lab" and "Open Perfecto Dashboard" buttons, the application first checks if there isn't an instance of one of these already running. The plugin doesn't allow launching more than one instance of "Perfecto Lab" and "Perfecto Dashboard" each.

###How to build and depoly

***Noet:*** This plugin will function properly only if the **perfecto** directory with the **perfecto-mobile-browser** was placed at the root of your JSystem runner (see "How to build and deploy" for **pefecto-mobile-browser** above).

This is a Maven project. To build it, run the following command line: `mvn clean install` from the project root directory. After a successful build, all artifacts will be found inside the `target` directory at the project's root.

The only important build artifact is the `jsystem-perfecto-plugin-<version>.jar` file.

To add the plugin to JSystem, copy the `jsystem-perfecto-plugin-<version>.jar` file into the `lib` directory inside your JSystem runner direcotry.

To enable the "Perfecto Mobile" tab inside JSystem, follow these steps, inside your JSystem runner:

1. Open "Tools" menu
2. Select "JSystem Properties"
3. Open the "New Features" tab
4. Click the "..." button at the "add.generic.tabs" line
5. Wait for "add.generic.tabs" window to show
6. Select "jsystem.perfecto.plugin.PerfectoTab" (if there are other tabs that you want to retain, hold the Ctrl key while selecting).
7. Click "Set" button and then "Save" button
8. Allow JSystem to restart. After restart you should see the "Perfecto Mobile" tab.

![How to add the "Perfecto Mobile" tab to JSystem] (https://github.com/ronyb/jsystem-perfecto-plugin/blob/master/images/add_jsystem_perfecto_tab.png)

#jsystem-perfecto-infra

###Description and use

###How to build and depoly

#jsystem-perfecto-tests

###Description and use

###How to build and depoly
