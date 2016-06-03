# Phingit

Automatic deployment helper using Phing and Git.

# Why
I was looking for a script that can update one or more repositories using cron.

I first did the script in bash, then I rewrote it using Phing.

# Requirements
- [Phing](https://github.com/phingofficial/phing)
- [Git](https://git-scm.com/)

# How to use
- In the directory *conf*, copy the file *default.phingit.yml* into *[CUSTOM_NAME].phingit.yml*.
- Edit the file and update it accordingly.
- Run the command: *phing -f build.xml phingit:main*.

# Hooks

A Hook is a custom command that you can create. Everytime Phingit run an action, it runs one hook before: *the pre hook* and one hook after: *the post hook*.
By default, commands are executed in the repository directory if it exists, if not in the directory where the build.xml file exists.

There are two ways to create hooks:

- In the configuration file (*.phingit.yml)
- In a phing build file (*.xml)

To create a hook, you have to follow this particular naming:

```phingit.hook.[HOOK_NAME].pre``` or ```phingit.hook.[HOOK_NAME].post```

Replace ```[HOOK_NAME]``` with the name of an action.

example: I want to run the command: df -h after the 'git.clone' command:

```
phingit.hook.git.clone.post:
 - df -h
```

or in a buildfile ([NAME].xml in the *hooks* directory):

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project name="hooks" default="hooks">
    <target name="hooks">
        <tstamp/>
    </target>
    <target name="phingit.hook.git.clone.post">
        <exec command="df -h" passthru="true"/>
    </target>
</project>
```

I want to run a drush command before the 'git.pull'.

```
phingit.hook.git.pull.pre:
 - drush status
 - drush archive-backup
```

You can also use properties:

```
phingit.hook.git.pull.post:
 - drush --root=${phingit.repository.directory} updatedb
```

# Command line options
- By default, Phingit will search for **configuration** files in the *conf* subdirectory. You can override the path of this
directory by specifying it on the command line: *phing -f build.xml phingit:main -Dphingit.conf.directory=/another/directory*
- By default, Phingit will search for **hooks** in the *hooks* subdirectory. You can override the path of this
directory by specifying it on the command line: *phing -f build.xml phingit:main -Dphingit.hooks.directory=/another/directory*

# Metadata
- Creation date: 2016/05/24
- Author: Pol Dellaiera - pol.dellaiera at protonmail dot com