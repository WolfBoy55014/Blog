+++
title = 'Install Java on Linux'
date = 2024-04-17T07:33:09-05:00

tags = ['raspberry pi', 'java', 'linux', 'how to']

[cover]
image = "https://adoptium.net/images/social-image.png"
alt = "Adoptium Logo"
relative = false
+++

# Intro
Java is a very versatile and fast programming language. Knowing how to download Java can be extremely useful, especially when you want to do anything with *Minecraft: Java Edition*. I also recommend it as beginner language, as it teaches you concepts and syntax used in highly performant and robust languages.
___
*Prerequisites*: This guide should be easy, only a basic knowledge of the command line is needed.
___

# Linux
___
## Install SDKMan

To download, install, and manage JDKs, [SDKMan](https://sdkman.io/) is my favorite. SDKMan makes having multiple Java installations a breeze. To download and install SDKMan, run:

```shell
curl -s "https://get.sdkman.io" | bash
```

Then, follow the helpful onscreen instructions and run:

```shell
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

Have SDKMan print out its version to make sure it works.

```shell
sdk version
```

## Choose JDK

If the latest version is all you want, thats easy. But, for many applications, you may need a very specific version (or you might just be picky, like me).
To download the latest java version you *could* simply do:

```shell
sdk install java
```

But, if you *are* picky, you can view their **whole catalog** by running:

```shell
sdk list java
```

You will be greeted by the most varied list of JDK's *I have ever seen*. Feel free to scroll through the list and find your favorite (or the one you need). :smile:

// TODO Actually run this and explain scrolling picking ID

## Install JVM

Once you find the ID of the JDK you want. (I personally want *17.0.2-tem*). It can be installed on your machine with the command below:

```shell
sdk install java 17.0.2-tem
```

When it prompts you if you want this to be your default, type `Y` to accept. This is not necessary, especially if you have multiple installs, but is extremely useful.

You did it! Now if you run:

```shell
java -version
```
It should print out its version! 

## Tips

There are plenty of other features within SDKMan. Check out their documentation for all the tricks!

# Raspberry Pi

For install instructions for Raspberry Pi, check out the [Linux](#linux) instructions as they are the same, except it will default to ARM when it downloads.

# Conclusion
___
I think Java is a great language, and having an easy way to install makes it even better. Huge shout-out to the creators of SDKMan, as they have saved us a huge headache! I hope this guide helped, and have fun! :wave: