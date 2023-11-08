---
Tags: 
Created: 2023-11-02 10:55:42
---
(Links:: [[Operating Systems]])
# UNIX
C was originally created in 1972 to develop UNIX programs. In the world of UNIX, **everything is a file**. By default, every process starts with 3 files open: *stdin*, *stdout* and *stderr*. If we'd want to output a string to the terminal in C, we'd normally do it like this:
```c
#include <stdio.h>

int main(int argc, char **argv)
{
	printf("Hello World!\n");
	return 0;
}
```
But we can also output to a file descriptor explicitly:
```c
#include <unistd.h>
#define STDOUT 1

int main(int argc, char **argv)
{
	char msg[] = "Hello World!\n";
	write(STDOUT, msg, sizeof(msg));
	return 0;
}
```
We can also use system calls:
```c
#define _GNU_SOURCE
#include <sys/syscall.h>
#define STDOUT 1

int main(int argc, char **argv)
{
    char msg[] = "Hello World!\n";
	int nr = SYS_write;
	syscall(nr, STDOUT, msg, sizeof(msg)); return 0;
}
```
Luckily, the libc standard library contains useful wrappers around syscalls (`write`, `read`, `exit`). System calls are normally triggered by triggering the special instruction `0x80`. Any arguments are usually passed in with registers, additional ones are on the stack. 
In a minimal shell, the program would first **wait for the user** to type in a command. A **new process is started** to execute the command. It the waits for the **process to finish**. To create a new process we use `pid_t fork()`. This duplicates the current process, returns a child pid in the caller and returns 0 in the new process. To wait for the process to finish, we use the instruction `pid_t wait(int *wstatus)`. This waits for the child processes to change state and write the status to `wstatus`.
To execute the command given we call `int execv(const cahr *path, char *constargv[]);`.
> [!example]+ fork, wait and execv
> ```c
> void main(void)
> {
> 	int pid, child_status;
> 	char *args[] = {"/bin/ls", "-l", NULL};
> 	if (fork() == 0) {           // fork creates child process
> 		execv(args[0], args)     // in child: load+exec program
> 	} else {
> 		wait(&child_status);     // Wait for child
> 	}
> }
> ```

When pressing `Ctrl+C`, a program stops. This is done by using the signal syscall handler.
Processes sometimes need to be interrupted during their execution. A **signal** is sent to the process that needs to be interrupted. An interrupted process can catch the signal by installing a **signal handler**. There are 3 different types: `signal`, `alarm` and `kill`.

## Pipe
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#define STDIN  0
#define STDOUT 1
#define PIPE_RD 0
#define PIPE_WR 1
int main(int argc, char** argv)
{
    pid_t cat_pid, sort_pid;
    int fd[2];
	pipe(fd);
	cat_pid = fork();
	if ( cat_pid == 0 ) {
		close(fd[PIPE_RD]);
		close(STDOUT);
		dup(fd[PIPE_WR]);
		execl("/bin/cat", "cat", "names.txt" , NULL);
	}
	sort_pid = fork();
	if ( sort_pid == 0 ) {
		close(fd[PIPE_WR]);
		close(STDIN);
		dup(fd[PIPE_RD]); 
		execl("/usr/bin/sort", "sort", NULL);
	} 

	close(fd[PIPE_RD]);
	close(fd[PIPE_WR]);
	/* wait for children to finish */ 
	waitpid(cat_pid, NULL, 0); 
	waitpid(sort_pid, NULL, 0);
	return 0;
}
```

---
References: