
- [x] Read Session Contents.

### Section 5.3.1

- [x] Write the `Hello World!` program
    - [x] code:  

```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <wait.h>

int main() {
    int pipefd[2];
    pid_t pid;
    char buffer[128];
    const char *message = "Hello World!";

    if (pipe(pipefd) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }

    pid = fork();
    if (pid == -1) {
        perror("fork");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) {
        // Child process
        close(pipefd[1]);
        read(pipefd[0], buffer, sizeof(buffer));
        close(pipefd[0]);
        printf("Child received: %s\n", buffer);
    } else {
        // Parent process
        close(pipefd[0]);
        write(pipefd[1], message, strlen(message) + 1);
        close(pipefd[1]);
        wait(NULL);
    }

    return 0;
}
```
![image](https://github.com/user-attachments/assets/a20fdf44-ec1d-4b67-b532-3cf18cf72ab2)

    
- [x] Write the `ls` to `wc` program
    - [x] code:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int pipefd[2];
    pid_t pid;

    if (pipe(pipefd) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }

    pid = fork();
    if (pid == -1) {
        perror("fork");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) {
        // Child process
        close(pipefd[1]);
        dup2(pipefd[0], STDIN_FILENO);
        close(pipefd[0]);

        execlp("wc", "wc", NULL);
        perror("execlp");
        exit(EXIT_FAILURE);
    } else {
        // Parent process
        close(pipefd[0]);
        dup2(pipefd[1], STDOUT_FILENO);
        close(pipefd[1]);

        execlp("ls", "ls", NULL);
        perror("execlp");
        exit(EXIT_FAILURE);
    }

    wait(NULL);

    return 0;
}
```
![image](https://github.com/user-attachments/assets/7985476d-3be3-420e-923c-f8d860dd65b1)


- [x] Investigate how to have a bi-direction pipe
    - [x] Two-way interprocess communication can be achieved by using two pipes, one for each direction of data flow between the processes. we create two pipes: one for sending data from process A to process B and another for sending data from process B to process A. Each process writes to one pipe and reads from the other.
    
    

### Section 5.3.2

- [x] Describe the usecase of different signals:
    1. [x] SIGINT: Interrupt signal, typically initiated by the user (Ctrl+C) to terminate a process.
    1. [x] SIGHUP: Hangup signal, sent when a terminal is disconnected, often used to reload configuration files.
    1. [x] SIGSTOP: Stop signal, suspends a process's execution without terminating it.
    1. [x] SIGCONT: Continue signal, resumes a process that has been stopped by SIGSTOP.
    1. [x] SIGKILL: Kill signal, forcefully and immediately terminates a process without cleanup.

- [x] Describe SIGALRM
    1. [x] The alarm signal, sent to a process when a timer set by the alarm() system call expires. It is commonly used for implementing timeouts in program execution, allowing a process to handle situations where operations take too long.

- [x] Investigate the given code
    1. [x] This program sets an alarm to trigger for 5 seconds, prints "Looping forever . . ." and then enters an infinite loop. When the alarm goes off, it sends a `SIGALRM` signal to the process, terminating it and printing "Alarm clock".
    1. [x] ![image](https://github.com/user-attachments/assets/3d9b7c80-7aa1-4eb4-9434-d7a27e04a95f)

- [x] Modify the given program by handling SIGALRM
    1. [x] code:
 ```
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handle_sigalrm(int sig) {
    printf("Received SIGALRM signal\n");
}

int main() {
    struct sigaction sa;

    sa.sa_handler = handle_sigalrm;
    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sigaction(SIGALRM, &sa, NULL);

    alarm(5);

    printf("Looping forever . . . \n");

    pause();

    printf("This line is executed after receiving SIGALRM\n");

    return 0;
}
```
![image](https://github.com/user-attachments/assets/6bbdafe9-cf14-4afa-9641-08c3735d017a)
- [x] Write a program that handles Ctrl + C
    1. [x] Code:
```
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
#include <unistd.h>

int interrupt_count = 0;

void handle_sigint(int sig) {
    interrupt_count++;
    if (interrupt_count == 1) {
        printf("Press Ctrl+C again to exit...\n");
    } else if (interrupt_count == 2) {
        exit(0);
    }
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handle_sigint;
    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sigaction(SIGINT, &sa, NULL);

    while (1) {
        pause();
    }

    return 0;
}
```
![image](https://github.com/user-attachments/assets/9d9bce9b-b4da-469d-91e8-30722d62ee84)


