
### Section 4.4.1
- [x] Investigate the ps command:

با دستور `ps aux` می‌توان لیست کل پردازه‌ها را مشاهده کرد.

![image](https://github.com/user-attachments/assets/3c8aebe7-a1e4-443b-9daf-552c6dd5781a)

    
- [x] Infromation about processes with PID = 1

با زدن دستور `ps aux | less` مشاهده می‌کنیم که پردازه sbin/init صاحب PID  یک است.

![image](https://github.com/user-attachments/assets/42280f39-d463-406f-8aa6-dceaf7b0da24)


پردازه init اولین پردازه‌ای است که توسط کرنل پس از بوت شدن سیستم راه‌اندازی می‌شود. این پردازه مسئول آغاز و مدیریت تمامی پردازه‌های دیگر در سیستم است. در سیستم‌های جدیدتر مانند توزیع‌های مدرن لینوکس، init معمولاً با systemd جایگزین شده است که وظایف مشابهی را انجام می‌دهد اما با ویژگی‌ها و قابلیت‌های بیشتری. هنگامی که سیستم بوت می‌شود، کرنل لینوکس پس از بارگذاری اولیه، پردازه init را اجرا می‌کند. این اولین پردازه‌ی کاربری است که راه‌اندازی می‌شود و دارای PID برابر با ۱ است. init یا systemd سپس بر اساس تنظیمات پیکربندی سیستم (مانند فایل‌های /etc/inittab یا فایل‌های پیکربندی systemd) دیگر سرویس‌ها و پردازه‌ها را راه‌اندازی می‌کند.

- [x] Program using getpid

```c
#include "stdio.h"
#include "unistd.h"

int main() {
    printf("%d\n", getpid());
    return 0;
}
```

![image](https://github.com/user-attachments/assets/6e198e7f-7efe-499b-b881-64a5165bda9f)


### Section 4.4.2


- [x] Program using getppid

```c
#include "stdio.h"
#include "unistd.h"

int main() {
    printf("%d\n", getppid());
    return 0;
}
```

پردازه والد با زدن دستور `gcc -o o 2.c && ./o | tee /dev/tty | ps` مشخص می‌شود که همان shell ای است که برنامه را از آن اجرا کرده‌ایم.

![image](https://github.com/user-attachments/assets/26af1e83-904a-4937-ad67-592714de0764)



- [x] Describe the C program (fork program)

در خط شماره ۵ که fork صدا زده می‌شود، برنامه‌ای که در حال اجرا است (پردازه والد) یک کپی از خود ایجاد می‌کند (پردازه فرزند). این پدر و فرزند عین هم هستند و هر دو باید الان خط ۶ را اجرا کنند. با این تفاوت که متغیر ret برای فرزند صفر است و برای پدر PID پردازه فرزند است. لذا پردازه فرزند تکه اول if را اجرا خواهد کرد و مقدار 23 را return خواهد کرد و پردازه پدر منتظر پایان فرزند باقی می‌ماند (wait) و سپس مقدار return فرزند را چاپ خواهد کرد.


- [x] Program showing that memory of the parent and the child is seperate

دو متغیر تعریف کرده‌ایم یکی محلی و یکی global. و مشاهده می‌کنیم که با تغییر آن‌ها در یکی، آن یکی تغییر نمی‌کند.
```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>


int global_variable = 1;

int main() {
    
    int local_variable = 2;
    
    int ret = fork();
    if (ret == 0) {
        printf("child: I am the child\n");
        global_variable = 10;
        printf("child: Set global_variable to 10\n");
        local_variable = 20;
        printf("child: Set global_variable to 20\n");
        return 23;
    } else {
        int rc = 0;
        wait(&rc);
        printf("parent: I am the parent\n");
        printf("parent: Return code is %d\n", WEXITSTATUS(rc));
        printf("parent: global_variable is %d\n", global_variable);
        printf("parent: global_variable is %d\n", local_variable);
    }
    return 0;
}

```
![image](https://github.com/user-attachments/assets/00c628af-0494-4bf2-858b-d93d70aaee9a)


- [x] Program printing different messages for parent and child process

    در کد بالا پیغام‌های متفاوتی چاپ می‌شود.

- [x] Program for the last task of this section
```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>


int global_variable = 1;

int main() {
    
    int local_variable = 2;
    
    int ret = fork();
    if (ret == 0) {
        printf("PID=%d\tI am the main child!\n", getpid());
        int cret = fork();
        if (cret == 0)
            printf("PID=%d\tI am the child of the child!\n", getpid());
        else
            printf("PID=%d\tI am the continue of the main child!\n", getpid());
        
        return 23;
    } else {
        int rc = 0;
        wait(&rc);
        printf("PID=%d\tI am the main parent!\n", getpid());
        int pret = fork();
        if (pret == 0)
            printf("PID=%d\tI am the child of the parent!\n", getpid());
        else
            printf("PID=%d\tI am the continue of the main parent!\n", getpid());
        
    }
    return 0;
}

```
  
![image](https://github.com/user-attachments/assets/6e5cee43-070a-4c24-ab06-53b973ac2c21)


توضیحات:

با پرینت های معنادار نیازی به توضیح نیست ولی به هر حال اجمالا توضیح می‌دهم: ابتدا پردازه فرزند به مرحله fork می‌رسد (به دلیل wait پدر) هنگام fork، فرزند به دو پردازه تقسیم می‌شود. یکی خودش یکی یک کپی از خودش. با بررسی PID ها در عکس بالا به صحت این ادعایم پی می‌برید. بعد از اتمام این مرحله، نوبت به fork شدن پردازه پدر می‌رسد. و آنجا هم مشابه اینجا عملیات fork اتفاق می‌افتد.

## Section 4.4.3

- [x] Program using `wait` and counting from 1 to 100
```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    int pid = fork();

    if (pid == 0) {
        for (int i = 1; i <= 100; i++)
            printf("%d\n", i);
        return 0;
    } else if (pid > 0) {
        wait(NULL);
        printf("Child process has finished.\n");
    } else {
        perror("fork failed");
        return 1;
    }
    return 0;
}
```
![image](https://github.com/user-attachments/assets/3478ad89-574b-48b1-ac19-14535e4c19f3)

سیستم‌کال wait منتظر می‌ماند تا پردازه فرزندِ پردازه فعلی پایان یابد. (توجه کنید که منتظر فرزندِ فرزند (نوه) نمی‌ماند) همچنین خروجی status-code فرزند را در آرگومان اول خود که یک پوینتر است می‌ریزد. وقتی که آرگومان اول را NULL می‌دهیم یعنی وضعیت return فرزند برایمان مهم نیست و صرفا منتظر پایان آن هستیم.

- [x] Program showing process adoption

```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    int pid = fork();
    if (pid == 0) {
        printf("CHILD: My parrent PID is: %d\n", getppid());
        printf("CHILD: Let's sleep for 2 seconds...\n");
        sleep(2);
        printf("CHILD: My parrent PID is: %d\n", getppid());
    } else {
        printf("PARENT: Let's sleep for 1 second...\n");
        sleep(1);
        printf("PARENT: Let's die :(\n");
        return 0;
    }
    return 0;
}

```

![image](https://github.com/user-attachments/assets/8dee1f1e-7272-4ebb-bf75-f7286b771ee5)

همان‌طور که در تصویر فوق مشخص است آی‌دی پردازه فرزند به پردازه init(systemd) یوزر تغییر می‌کند. توجه کنید به init اصلی تغییر نمی‌کند بلکه به init یوزر ali تغییر می‌کند.


### Section 4.4.4

- [x] Describe following commands/APIs:

1- دستور execv: این دستور آدرس absolute برنامه قابل اجرا را می‌گیرد و یک آرایه‌ای از آرگومان‌ها را دریافت می‌کند.
2- دستور execl: این دستور آدرس absolute برنامه قابل اجرا را می‌گیرد و کل آرگومان‌ها را به عنوان ورودی تابع  دریافت می‌کند.
3- دستور execvp: این دستور مشابه execv است با این تفاوت که آدرس کامل و absolute نمی‌خواهد بلکه نام برنامه را در دایرکتوری‌های لیست‌شده در PATH environment را جست‌و‌جو می‌کند.
4- دستور execlp: این دستور مشابه execl است با این تفاوت که آدرس کامل و absolute نمی‌خواهد بلکه نام برنامه را در دایرکتوری‌های لیست‌شده در PATH environment را جست‌و‌جو می‌کند.


- [x] Program which forks and executues `ls` command

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) {
        execlp("ls", "ls", "-g", "-h", (char *)NULL);
        perror("execlp failed");
        exit(-1);
    } else {
        int status;
        waitpid(pid, &status, 0);
        if (WIFEXITED(status)) 
            printf("Child process exited with status %d\n", WEXITSTATUS(status));
        else 
            printf("Child process did not exit successfully\n");
    }
    return 0;
}
```

![image](https://github.com/user-attachments/assets/850ae96b-b12c-476e-b1dd-1a7bb6880d49)


## Source Code Submission

please submit all your codes in a zip file

[HW4.zip](https://github.com/user-attachments/files/16402540/HW4.zip)

