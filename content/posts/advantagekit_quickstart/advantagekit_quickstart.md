+++
title = 'AdvantageKit Quick-Start'
date = 2024-05-14T15:41:01-05:00

tags = ['advantagekit', 'frc', 'robotics', 'how to', 'install']

[cover]
image = "https://github.com/Mechanical-Advantage/AdvantageKit/raw/main/banner.png"
alt = "AdvantageKit Logo"
relative = false
+++

# To install AdvantageKit In *your* project

## Install Libraries

### Install Vendor Depo

To install the required vendor depo:

1. Click the WPILib button in the top-right corner:

![WPILib Icon in the Corner](/Blog/posts/advantagekit_quickstart/wpilib_icon.jpg)

1. Click on the "Manage Vendor Libraries" button

![Manage Vendor Libraries](/Blog/posts/advantagekit_quickstart/manage_libraries.jpg)

1. click "Install new libraries (online)".

![Install new libraries (online)](/Blog/posts/advantagekit_quickstart/install_online.jpg)

Now paste the link below into the box presented. :point_down:

___
:stop_sign: Make sure to download the **appropriate** version for your WPILib version (you can check in the changelogs [here](https://github.com/Mechanical-Advantage/AdvantageKit/releases/latest)). But if you are using the latest version of WPILib (you should be), you can paste in:

```bash
https://github.com/Mechanical-Advantage/AdvantageKit/releases/latest/download/AdvantageKit.json
```

___

### Update `build.gradle`

Now, locate the `build.gradle` file in your project. If you can't see the Explorer view (the menu with the files) hit `Ctrl + j`.

1. `build.gradle`.

![Install new libraries (online)](/Blog/posts/advantagekit_quickstart/gradle_file.jpg)

2. insert this code *before* the dependencies block:

```groovy
repositories {
    maven {
        url = uri("https://maven.pkg.github.com/Mechanical-Advantage/AdvantageKit")
        credentials {
            username = "Mechanical-Advantage-Bot"
            password = "\u0067\u0068\u0070\u005f\u006e\u0056\u0051\u006a\u0055\u004f\u004c\u0061\u0079\u0066\u006e\u0078\u006e\u0037\u0051\u0049\u0054\u0042\u0032\u004c\u004a\u006d\u0055\u0070\u0073\u0031\u006d\u0037\u004c\u005a\u0030\u0076\u0062\u0070\u0063\u0051"
        }
    }
    mavenLocal()
}

configurations.all {
    exclude group: "edu.wpi.first.wpilibj"
}

task(checkAkitInstall, dependsOn: "classes", type: JavaExec) {
    mainClass = "org.littletonrobotics.junction.CheckInstall"
    classpath = sourceSets.main.runtimeClasspath
}
compileJava.finalizedBy checkAkitInstall
```

*before* dependencies, like shown below:

```groovy
// <-- Right Here

dependencies {
    // ...
}
```

___
:stop_sign: Leave the password be, and **:exclamation:DON'T TURN IT TO PLAINTEXT:exclamation:**. This will cause the team Mechanical-Advantage a lot a grief, don't try it.
___

3. *Inside* the dependencies block insert:

```groovy
def akitJson = new groovy.json.JsonSlurper().parseText(new File(projectDir.getAbsolutePath() + "/vendordeps/AdvantageKit.json").text)
annotationProcessor "org.littletonrobotics.akit.junction:junction-autolog:$akitJson.version"
```

*inside* dependencies, like shown below:

```groovy
// <-- Not here

dependencies {
    // ...
    def akitJson = new groovy.json.JsonSlurper().parseText(new File(projectDir.getAbsolutePath() + "/vendordeps/AdvantageKit.json").text)
    annotationProcessor "org.littletonrobotics.akit.junction:junction-autolog:$akitJson.version"
    // But there ^, after everything else.
}
```

If everything has worked out, you should be able to build. Now you have all libraries install and can move on onto implementation.

Good Job! :partying_face:

## Implement Base Code

To allow the robot to log, we need to insert some code into the `Robot.java` file. (located in `src\main\java\frc\robot\Robot.java`)

1. Change the `Robot` class from extending `TimedRobot` to `LoggedRobot`. They function the exact same, except for some things listed [here](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#robot-configuration).

```java
public class Robot extends TimedRobot {
    //...
}
```

:arrow_down::arrow_down::arrow_down:

```java
public class Robot extends LoggedRobot {
    //...
}
```

2. Insert the code below into the `robotInit()` method.

```java
Logger.recordMetadata("ProjectName", "YourProjectName"); // Set a metadata value

if (isReal()) {

    // If the robot is real (not a sim) log
    Logger.addDataReceiver(new WPILOGWriter()); // Log to a USB stick ("/U/logs")
    Logger.addDataReceiver(new NT4Publisher()); // Publish data to NetworkTables
    new PowerDistribution(1, ModuleType.kRev); // Enables power distribution logging
} else {

    // If the robot is simulated
    setUseTiming(false); // Run as fast as possible
    String logPath = LogFileUtil.findReplayLog(); // Pull the replay log from AdvantageScope (or prompt the user)
    Logger.setReplaySource(new WPILOGReader(logPath)); // Read replay log
    Logger.addDataReceiver(new WPILOGWriter(LogFileUtil.addPathSuffix(logPath, "_sim"))); // Save outputs to a new log
}

// Logger.disableDeterministicTimestamps() // See "Deterministic Timestamps" in the "Understanding Data Flow" page
Logger.start(); // Start logging! No more data receivers, replay sources, or metadata values may be added.
```

``` java
public void robotInit() {
    // <-- Here

    m_robotContainer = new RobotContainer();
}
```

___
:stop_sign: If you are having issues with writing to the USB drive, make sure the drive is:

- in a USB port on the RoboRIO
- formatted to FAT32
- labeled 'U:'
- also try plugging it into another machine

___

Now that should be all you need to install AdvantageKit on your robot! :+1:

# Logging with AdvantageKit

Logging inputs and other random values with Advantage Kit
is extremely simple. Literally just insert:

## With `recordOutput()`

```java
Logger.recordOutput(<name>, <value>);
```

`<name>` is a string containing the location and name of the value saved (and thus where it will appear in Advantage Scope). Such as `"Odometry/Location"` will
save the value as `Location` under the `Odometry` Folder.

`<value>` can be of any rudimentary variable, Translation2d, Pose3d,
SwerveModuleState, and Mechanism2d. These are the values that will be logged.
All supported variable types are listed [here](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/DATA-FLOW.md#data-types).

## With `@AutoLogOutput`

Logging can also be done using the `@AutoLogOutput` annotation.
More info [here](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/RECORDING-OUTPUTS.md#autologoutput-annotation).

```java
public class Example {
    @AutoLogOutput // Logged as "Example/Angle"
    private double angle = 0.1f;

    @AutoLogOutput(key = "Custom/Pressed") // Logged as "Custom/Pressed"
    public boolean pressed;
    pressed = true;
}
```

___
If you find time, check out [Mechanical Advantage's documentation](https://github.com/Mechanical-Advantage/AdvantageKit/tree/main/docs), as there are many other cool features of AdvantageKit, such as simulation. Also check out [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope), a nice way to view your logs.

# Credits

These instructions were based off [Mechanical Advantage's own install instructions](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md). If you have more questions, please check their tutorials or [our 2024 comp code](https://github.com/FIRST-Team-2472/Parallax-2024-Revamped/blob/main/src/main/java/frc/robot/subsystems/SwerveSubsystem.java), specifically in the `SwerveSubsystem.class`. All credit goes to team Mechanical Advantage for the wonderful libraries and tools.
