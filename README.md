#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>

int main()
{
    pid_t pid;

    printf("Parent process started.\n");
    printf("Parent PID: %d\n", getpid());

    // Create a new process
    pid = fork();

    if (pid < 0)
    {
        printf("Fork failed!\n");
        exit(1);
    }

    else if (pid == 0)
    {
        // Child process
        printf("\nChild process created.\n");
        printf("Child PID: %d\n", getpid());

        // Execute another program
        printf("Child is executing 'ls' command...\n");
        execl("/bin/ls", "ls", "-l", NULL);

        // This executes only if exec fails
        printf("Exec failed!\n");
        exit(1);
    }

    else
    {
        // Parent process
        printf("\nParent is waiting for child process...\n");

        // Wait for child to terminate
        wait(NULL);

        printf("Child process terminated.\n");
        printf("Parent process terminated.\n");

        // Terminate parent process
        exit(0);
    }

    return 0;
}
