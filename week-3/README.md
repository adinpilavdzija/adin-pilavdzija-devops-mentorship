# 03 Bash/Shell scripting

## Task

For the week-3 task you need to do following.

Read `Chapter 13`, `Chapter 14`, `Chapter 15`, and `Chapter 16` from the book `Linux Command Line and Shell Scripting Bible, 3rd Edition`.  
The book is available for download inside our [Class notes reading materials](https://github.com/allops-solutions/devops-aws-mentorship-program/blob/main/devops-mentorship-program/02-february/week-3-280223/00-class-notes.md#-reading-materials).

As you read the chapters you will notice different scripts. For an example script test-1.sh

```bash
$ cat test1 
#!/bin/bash 
# basic for command 

for test in Alabama Alaska Arizona Arkansas California Colorado 
do 
    echo The next state is $test 
done 
$ ./test1 
The next state is Alabama 
The next state is Alaska 
The next state is Arizona 
The next state is Arkansas 
The next state is California 
The next state is Colorado 
$
```

You need to do the following:
1.  Log in on `ssh centos@3.68.91.255` and create directory called: --week-3.
2.  Create `.sh` files for each script and execute scripts
3.  Took screenshots after executing scripts.
4.  Push all scripts to your GitHub repository inside the directory `week-3`
5.  **TIER 1 ONLY** Create a pull request and post it as a comment on this issue. Inside the pull request, you need to post screenshots from step 3 so that we have confirmation you actually executed scripts.  
    you need to execute each script at our centos server `ssh centos@3.68.91.255`

## Task solution

- Chapter 13 -> [Scripts](./chapter13-scripts/), [Screenshots](./chapter13-screenshots/)
- Chapter 14 -> [Scripts](./chapter14-scripts/), [Screenshots](./chapter14-screenshots/)
- Chapter 15 -> [Scripts](./chapter15-scripts/), [Screenshots](./chapter15-screenshots/)
- Chapter 16 -> [Scripts](./chapter16-scripts/), [Screenshots](./chapter16-screenshots/)

### Chapter 13

- The for Command
- The C-Style for Command
- The while Command
- The until Command
- Nesting Loops
- Looping on File Data
- Controlling the Loop
- Processing the Output of a Loop

Looping is an integral part of programming. The bash shell provides three looping commands that you can use in your scripts.

The `for` command allows you to iterate through a list of values, either supplied within the command line, contained in a variable, or obtained by using file globbing, to extract file and directory names from a wildcard character.

The `while` command provides a method to loop based on the condition of a command, using either ordinary commands or the test command, which allows you to test conditions of variables. As long as the command (or condition) produces a zero exit status, the while loop continues to iterate through the specifi ed set of commands.

The `until` command also provides a method to iterate through commands, but it bases its iterations on a command (or condition) producing a non-zero exit status. This feature allows you to set a condition that must be met before the iteration stops.

You can combine loops in shell scripts, producing multiple layers of loops. The bash shell provides the `continue` and `break` commands, which allow you to alter the flow of the normal loop process based on different values within the loop.

The bash shell also allows you to use standard command redirection and piping to alter the output of a loop. You can use redirection to redirect the output of a loop to a file or piping to redirect the output of a loop to another command. This provides a wealth of features with which you can control your shell script execution.

### Chapter 14

- Passing Parameters
- Using Special Parameter Variables
- Being Shifty
- Working with Options
- Standardizing Options
- Getting User Input

This chapter showed three methods for retrieving data from the script user. Command line parameters allow users to enter data directly on the command line when they run the script. The script uses positional parameters to retrieve the command line parameters and assign them to variables.

The shift command allows you to manipulate the command line parameters by rotating them within the positional parameters. This command allows you to easily iterate through the parameters without knowing how many parameters are available.

You can use three special variables when working with command line parameters. The shell sets the `$#` variable to the number of parameters entered on the command line. The `$*` variable contains all the parameters as a single string, and the `$@` variable contains all the parameters as separate words. These variables come in handy when you’re trying to process long parameter lists.

Besides parameters, your script users can use command line options to pass information to your script. Command line options are single letters preceded by a dash. Different options can be assigned to alter the behavior of your script.

The bash shell provides three ways to handle command line options.

The first way is to handle them just like command line parameters. You can iterate through the options using the positional parameter variables, processing each option as it appears on the command line.

Another way to handle command line options is with the `getopt` command. This command converts command line options and parameters into a standard format that you can process in your script. The `getopt` command allows you to specify which letters it recognizes as options and which options require an additional parameter value. The `getopt` command processes the standard command line parameters and outputs the options and parameters in the proper order.

The final method for handling command line options is via the `getopts` command (note that it’s plural). The `getopts` command provides more advanced processing of the command line parameters. It allows for multi-value parameters, along with identifying options not defined by the script.

An interactive method to obtain data from your script users is the `read` command. The `read` command allows your scripts to query users for information and wait. The `read` command places any data entered by the script user into one or more variables, which you can use within the script.

Several options are available for the `read` command that allow you to customize the data input into your script, such as using hidden data entry, applying timed data entry, and requesting a specific number of input characters.

### Chapter 15

- Understanding Input and Output
- Redirecting Output in Scripts
- Redirecting Input in Scripts
- Creating Your Own Redirection
- Listing Open File Descriptors
- Suppressing Command Output
- Using Temporary Files
- Logging Messages
- Practical Example

Understanding how the bash shell handles input and output can come in handy when creating your scripts. You can manipulate both how the script receives data and how it displays data, to customize your script for any environment. You can redirect the input of a script from the standard input (`STDIN`) to any file on the system. You can also redirect the output of the script from the standard output (`STDOUT`) to any file on the system.

Besides the `STDOUT`, you can redirect any error messages your script generates by redirecting the `STDERR` output. This is accomplished by redirecting the file descriptor associated with the `STDERR` output, which is file descriptor 2. You can redirect `STDERR` output to the same file as the `STDOUT` output or to a completely separate file. This enables you to separate normal script messages from any error messages generated by the script.

The bash shell allows you to create your own file descriptors for use in your scripts. You can create file descriptors 3 through 8 and assign them to any output file you desire. After you create a file descriptor, you can redirect the output of any command to it, using the standard redirection symbols.

The bash shell also allows you to redirect input to a file descriptor, providing an easy way to read data contained in a file into your script. You can use the `lsof` command to display the active file descriptors in your shell.

Linux systems provide a special file, called `/dev/null`, to allow you to redirect output that you don’t want. The Linux system discards anything redirected to the `/dev/null` file. You can also use this file to produce an empty file by redirecting the contents of the `/dev/null` file to the file.

The `mktemp` command is a handy feature of the bash shell that allows you to easily create temporary files and directories. Simply specify a template for the `mktemp` command, and it creates a unique file each time you call it, based on the file template format. You can also create temporary files and directories in the `/tmp` directory on the Linux system, which is a special location that isn’t preserved between system boots.

The `tee` command is a handy way to send output both to the standard output and to a log file. This enables you to display messages from your script on the monitor and store them in a log file at the same time.

### Chapter 16

- Handling Signals
- Running Scripts in Background Mode
- Running Scripts without a Hang-Up
- Controlling the Job
- Being Nice
- Running Like Clockwork

The Linux system allows you to control your shell scripts by using signals. The bash shell accepts signals and passes them on to any process running under the shell process. Linux signals allow you to easily kill a runaway process or temporarily pause a long-running process. 

You can use the `trap` statement in your scripts to catch signals and perform commands. This feature provides a simple way to control whether a user can interrupt your script while it’s running.

By default, when you run a script in a terminal session shell, the interactive shell is suspended until the script completes. You can cause a script or command to run in background mode by adding an ampersand sign (&) after the command name. When you run a script or command in background mode, the interactive shell returns, allowing you to continue entering more commands. Any background processes run using this method are still tied to the terminal session. If you exit the terminal session, the background processes also exit. 

To prevent this from happening, use the `nohup` command. This command intercepts any signals intended for the command that would stop it — for example, when you exit the terminal session. This allows scripts to continue running in background mode even if you exit the terminal session.

When you move a process to background mode, you can still control what happens to it. The jobs command allows you to view processes started from the shell session. After you know the job ID of a background process, you can use the `kill` command to send Linux signals to the process or use the `fg` command to bring the process back to the foreground in the shell session. You can suspend a running foreground process by using the Ctrl+Z key combination and place it back in background mode, using the `bg` command.

The `nice` and `renice` commands allow you to change the priority level of a process. By giving a process a lower priority, you allow the CPU to allocate less time to it. This comes in handy when running long processes that can take lots of CPU time.

In addition to controlling processes while they’re running, you can also determine when a process starts on the system. Instead of running a script directly from the command line interface prompt, you can schedule the process to run at an alternative time. You can accomplish this in several different ways. The at command enables you to run a script once at a preset time. The `cron` program provides an interface that can run scripts at a regularly scheduled interval. 

Finally, the Linux system provides script files for you to use for scheduling your scripts to run whenever a user starts a new bash shell. Similarly, the startup files, such as `.bashrc`, are located in every user’s home directory to provide a location to place scripts and commands that run with a new shell. 

## Cheatsheet

https://devhints.io/bash

![bash-scripting-cheatsheet](https://github.com/adinpilavdzija/adin-pilavdzija-devops-mentorship/assets/65655945/ccb843e9-b132-43b2-9fa4-843ca627d360)
