# Before You Begin

Before you begin writing shell scripts, you should familiarize yourself a bit with the shell environment.

## Obtaining a Shell Prompt

There are many ways to get shell access, depending on the operating system you are running.

### In OS X

There are four ways to get a shell prompt in OS X:

- Run Terminal.
    
    This is, by far, the easiest way to get a shell prompt. It has the advantage of providing access to other GUI applications at the same time.This is the recommended way to get shell access.
    
    You can find Terminal in the Utilities folder inside your Applications folder.
    
- Connect via SSH (secure shell).
    
    First, enable “Remote Login” in the Sharing preferences pane.
    
    Next, use the SSH client of your choice to log in. For example, you might use the `ssh` command in Terminal to run scripts on a remote computer. For more information, see the documentation for `ssh`.
    
- Use the OS X (Mach) console.
    
    In System Preferences, open the Accounts preference pane (Users in OS X v10.1 and earlier), and set the “Display login window as” setting to “Name and Password”. Then log out.
    
    Next, at the login window, Type `>console` as the username. (Leave the password field blank.)
    
    You will then see a text-based login prompt. Log in with your “short name” and password.
    
    Log out (type `exit` or `logout` and press return) to get back to GUI-land (or just enter a few wrong passwords in a row).
    
- Boot single user.
    
    This environment is _not_ generally recommended for scripting. It takes considerable effort to enable networking, mount external disks, and enable other functionality. Also, the root volume is mounted read-only by default. As a result, this mode is mainly useful for disaster recovery.
    

### In Other UNIX Variants or Linux Variants

In most other UNIX or Linux variants, you can gain access to a shell by running XTerm, GTerm, KTerm, Terminal, or some other similarly named application. Alternatively, if you log into such a machine remotely using `ssh`, you should get a shell prompt as soon as you log in.

Some UNIX or Linux variants provide a text-based login prompt. On these systems, you generally get a shell prompt as soon as you log in.

### In Windows

Although Windows does not provide a shell, you can add one by installing Cygwin. Instructions for installing Cygwin are beyond the scope of this document. See [http://www.cygwin.com/](http://www.cygwin.com/) for more information.

**Note:** The Cygwin environment is not a complete UNIX shell scripting environment. The examples in this document have not been tested in Cygwin and are not guaranteed to work correctly in the Cygwin environment.

## Familiarize Yourself With the Command Line

Read [Command Line Primer](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/CommandLInePrimer/CommandLine.html#//apple_ref/doc/uid/TP40004268-CH271-BBCBEAJD) to get a good overview of how to get things done in a command line environment.

## Tips for Shell Users

While this document is primarily focused on writing shell scripts, there are a few helpful tips that can be useful to shell users and programmers alike. This section includes a few of those tips.

### The alias Builtin

Various Bourne shells also offer a number of other builtin commands that you may find useful, one of the more useful for command-line users being `alias`. This command allows you to assign a short name to replace a longer command. While the `alias` builtin is not frequently used in shell scripts (unless you are intentionally trying to obfuscate your code), it is very convenient when using the shell interactively. For example:

|   |
|---|
|alias listsource="ls *.c *.h"|

Typing the command `listsource` after entering this line will result in listing all of the `.c` and `.h` files in the current directory.

For more information, see the man page `builtins`, or for ZSH, `zshbuiltins`.

**C Shell Note:** The C shell syntax is similar, but not identical. In the C shell, the equals sign is replaced with a space. For example:

|   |
|---|
|alias listsource "ls *.c *.h"|

An alias is only active for the remainder of the current shell session. To make an alias permanent, you must add it to an appropriate script that gets run automatically whenever your shell starts up. See [Login Scripts](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/BeforeYouBegin/BeforeYouBegin.html#//apple_ref/doc/uid/TP40004268-CH1-SW15) to learn how.

For more information, see the manual page for your login shell (for example, `bash`, `csh`, `sh`, `tcsh`, or `zsh`).

### Login Scripts

OS X provides support for login scripts and environment property lists to allow you to set environment variables and aliases that are automatically set whenever you run a new shell. There are two ways to do this:

- **Bourne shell (bash, zsh, and so on):**
    
    To persistently set environment variables and add aliases, you can add the appropriate `alias`, variable assignment, and `export` commands to the following files:
    
    - `~/.profile`—executed automatically for all login shells.
        
    - `~/.bash_profile`—similar to `.profile`, but only runs for `bash` login shells.
        
    - `~/.bashrc` and `~/.zshrc`—executed automatically for all non-login `bash` or `zsh` shells (when you explicitly type `bash` or `zsh` on the command line or run a script that starts with `#!/bin/bash` or `#!/bin/zsh`).
        
    
    You may also find it useful to create a `.bashrc` file that sources your `.profile` file. For example:
    
    |   |
    |---|
    |. $HOME/.profile|
    
    Sourcing is described in more detail in [Subroutines, Scoping, and Sourcing](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW1).
    
- **C shell (csh, tcsh, and so on):**
    
    To persistently set environment variables and add aliases, you can add the appropriate `alias`, `set`, and `setenv` commands to the following files:
    
    - `~/.login`—automatically executes for all login shells.
        
    - `~/.cshrc`—automatically executes for all non-login shells (when you explicitly type `bash` on the command line or run a script that starts with `#!/bin/csh` or `#!/bin/tcsh`).
        
    
    You may also find it useful to create a `.cshrc` file that sources your `.login` file. For example:
    
    |   |
    |---|
    |source $HOME/.login|
    
    Sourcing is described in more detail in [Subroutines, Scoping, and Sourcing](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW1).
    

### Entering Special Characters

Some shells treat tabs and other control characters in special ways. When writing a script in a text file, the reuse of these characters for shell-specific purposes is not generally an issue. However, when entering commands on the command line, it may get in the way if you need to enter any of these characters as part of a command for some reason.

To enter a tab or other control character on the command line, type control-v followed by the tab key or other control character. The control-v tells the shell to treat whatever character comes next literally without interpreting it in any way during entry.

For example, to enter the ASCII bell character (control-G), you can type the following:

|   |
|---|
|echo "control-V control-G"|

This will be seen on your screen as:

|   |
|---|
|echo "^G"|

When you press return, your computer should beep.

## Creating Text Files in Your Home Directory

In various parts of this document, you need to create a text file and save it into your home directory.

In Terminal, your home directory is the directory that you are in when you first open the Terminal window.

In the rest of OS X, your home directory can be found in the “PLACES” list in Finder window sidebars, Save dialog sidebars, and so on. It's the icon that looks like a house. Your home directory is also the default location if you create a new finder window by choosing **File** > **New Finder Window** in Finder.

### Creating Text Files with TextEdit

Creating a text file in TextEdit is fairly straightforward.

1. Create a new file by choosing **File** > **New** (from the **File** menu).
    
2. Choose **Format** > **Make Plain Text**.
    
    By default, TextEdit saves files in Rich Text Format (RTF). Choosing Make Plain Text from the Format menu tells it that you want to work with a plain text file instead.
    
3. Type or paste in the script as directed in the text.
    
4. Choose **File** > **Save As**.
    
5. In the resulting Save dialog, scroll the sidebar on the left until you see the “PLACES” section, and click the house icon beside your username.
    
6. Name the file as directed in the text and save it.
    

**Important:** If you are running OS X v10.7.3, any text files you create with TextEdit may fail to execute with the error “bad interpreter: Operation not permitted”. To fix this problem, upgrade to OS X v10.7.4 or later and paste the script into a new file.

### Creating Text Files with Xcode

Creating a text file in Xcode is fairly straightforward.

1. Create a new file by choosing **File** > **New** > **File...** (from the **File** menu).
    
2. Choose “Other” in the “OS X” section of the sidebar, then choose “Shell Script” as the file type.
    
3. Click the “Next” button.
    
4. In the resulting Save dialog, click the disclosure triangle so that the entire save panel is visible. Then, scroll the sidebar on the left until you see the “PLACES” section, and click the house icon beside your username.
    
5. Name the file as directed in the text and save it.
    
6. Type or paste in the script as directed in the text.
    
7. Choose **File** > **Save**.
    

### Creating Text Files with pico or nano

If you are logging into a computer remotely using SSH, you must use a text editor that can be run on the command line (unless you use X11 forwarding and an X11-based editor).

The `pico` and `nano` commands are two very easy command-line text editors. At least one of these commands is available in most UNIX or Linux-based operating systems.

To create a text file in NANO or PICO:

1. Type `nano` _filename_ or `pico` _filename_ and press return. (Type the name of the file you want to create or edit instead of the word _filename_.)
    
2. Edit the file. Use arrow keys to navigate.
    
3. When you are finished editing, press Control-O. Adjust the name of the file (if desired), then press return to save the file to disk.
    
4. To exit the editor, press Control-X.
    

For other valid commands, see the list of control characters along the bottom of the screen or press Control-G for more complete documentation.

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/shell_scripts/shell_scripts.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/Introduction/Introduction.html)