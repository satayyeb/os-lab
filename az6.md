
- [x] Read Session Contents.

## Section 6.4

- [x] Using `malloc` and `free` in program

خروچی دستور malloc آدرس جایی از حافظه است که به ما [پس از درخواست] اختصاص داده شد. و تایپ آن void* است که به صورت خودکار به تایپ صحیح تبدیل خواهد شد.

```c
#include "stdio.h"
#include "stdlib.h"
#include "memory.h"

struct MyStruct
{
    int a;
    int b;
    char name[20];
};

int main()
{
    struct MyStruct* my_struct = malloc(sizeof(struct MyStruct));
    
    // Assign:
    my_struct->a = 12;
    my_struct->b = 24;
    memcpy(my_struct->name, "Tayyeb", 6);

    // Print:
    printf("my_struct info:  a=%d  b=%d  name=%s\n", my_struct->a, my_struct->b, my_struct->name);
    
    free(my_struct);
    
    return 0;
}
```

نتایج اجرای کد فوق:

![image](https://github.com/user-attachments/assets/37a4ab87-d642-433f-866e-3d0821c2c89c)

    
- [x]  Using `ps`


![image](https://github.com/user-attachments/assets/420a50a4-dcb8-4564-9d72-8d166af39322)


با استفاده از پایپ grep اطلاعات لازم را کسب می‌کنیم:

- euser (EUSER)  effective user name.  This will be the textual user ID, if it can be obtained and the field width permits, or a decimal representation otherwise.  The n option can be used to force the decimal representation.  (alias uname, user).
- vsz  (VSZ)  virtual memory size of the process in KiB (1024-byte units).  Device mappings are currently excluded; this is subject to change.  (alias vsize).
- rss  (RSS)  resident set size, the non-swapped physical memory that a task has used (in kilobytes).  (alias rssize, rsz).
- pmem  (%MEM)   ratio of the process's resident set size  to the physical memory on the machine, expressed as a percentage.  (alias pmem).
- fname (COMMAND)   first 8 bytes of the base name of the process's executable file.  The output in this column may contain spaces.



- [x]  Getting started with memory segments

![image](https://github.com/user-attachments/assets/b00db213-9e5b-49dd-9903-0bca1ef0d1be)

دستور size صرفا اطلاعات static باینری را نمایش می‌دهد و قادر به نمایش اطلاعات دینامیکی که موقع اجرای برنامه به وجود می‌آیند نیست. یعنی صرفا text و data و bss را نمایش می‌دهد و heap و stack و env/arg را نمایش نمی‌دهد.
    

- [x] Getting started with memory sharing

![image](https://github.com/user-attachments/assets/c2b7a7db-24d3-4f37-ae93-499422143b4f)

![image](https://github.com/user-attachments/assets/bb871ad0-0f0a-4b52-80cb-59843f67fca1)

![image](https://github.com/user-attachments/assets/d131323c-905a-4d28-af41-4512252af2f2)



- [x] Getting started with addresses

```
DESCRIPTION
       The addresses of these symbols indicate the end of various program segments:

       etext  This is the first address past the end of the text segment (the program code).

       edata  This is the first address past the end of the initialized data segment.

       end    This is the first address past the end of the uninitialized data segment (also known as the BSS segment).
```

![image](https://github.com/user-attachments/assets/5d573845-b3a1-4854-9608-ffb596c85870)

بله با ساختار تطابق دارد. چرا که آدرس bss بزرگ‌تر از data و data هم بزرگ‌تر از text است.

دلیل کامنت این است که اگر کلیدواژه char را نذاریم (تایپ تعریف نکنیم)، کامپایلر هشدار خواهد داد. لذا باید تایپ بذاریم.

دلیل اینکه به این‌ها نماد گفته می‌شود این است که اگر متغیر بودند می‌شد آن‌ها را تغییر داد یا اینکه مکان مشخصی در حافظه می‌داشتند. در حالی که این موجودیت‌ها صرفا آدرس  بخش‌های مختلف برنامه را نشان می‌دهند و نباید در حافظه جایگاه جداگانه‌ای برای آن‌ها در نظر گرفت و نباید بتوان مقدار آن‌ها را تغییر داد. به بیان دیگر این نمادها نوعی statics هستند که linker در مرحله link آن‌ها را بدست می‌آورد نه اینکه متغیر‌هایی باشند که امکان تغییر داشته باشند.

دلیل استفاده از کلیدواژه extern این است که کامپایلر بفهمد این نماد‌ها در فایل دیگری تعریف شده‌اند و نباید در این فایل فعلی حافظه‌ای به آن‌ها اختصاص یابد. به عبارتی دیگر این کلیدواژه به کامپایلر می‌گوید که این آدرس‌ها در زمان linking اضافه خواهند شد.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    void* first = malloc(1024);

    void *initial_break = sbrk(0);
    printf("Initial end of heap: %p\n", initial_break);

    int count = 0;
    while (1) {
        void* looped = malloc(1024);

        void *current_break = sbrk(0);

        count++;

        if (current_break != initial_break) {
            break;
        }
    }

    printf("Number of iterations: %d\n", count);
    return 0;
}

```

![image](https://github.com/user-attachments/assets/4fcd33cc-2974-40a5-8ca1-56446b1da5d1)

به طور معمول انتظار داریم که بعد از اولین فراخوانی malloc، انتهای heap تغییر کند، اما در عمل مشاهده می‌شود که ممکن است چندین بار malloc انجام شود بدون اینکه انتهای heap تغییر کند. دلیل این رفتار به مدیریت داخلی تخصیص حافظه توسط کتابخانه malloc برمی‌گردد. دلیل اصلی این پدیده، این است که malloc معمولاً حافظه را به صورت بلوک‌های بزرگ‌تر از آنچه که درخواست شده است از سیستم عامل درخواست می‌کند. این بلوک‌های بزرگ به صورت داخلی توسط malloc مدیریت می‌شوند و تخصیص‌های کوچک‌تر در درون این بلوک‌های بزرگ انجام می‌شوند. بنابراین، تا زمانی که کل بلوک بزرگ مصرف نشده باشد، malloc نیازی به درخواست حافظه جدید از سیستم عامل (که منجر به افزایش brk و تغییر انتهای heap می‌شود) نخواهد داشت.

```c
#include <stdio.h>

void recursiveFunction(int cnt, int max) {
    int i;
    printf("Recursion depth: %d, Address of i: %p\n", cnt, (void*)&i);
    if (cnt < max)
        recursiveFunction(cnt + 1, max);
}

int main() {
    recursiveFunction(0, 100);
    return 0;
}
```

![image](https://github.com/user-attachments/assets/4ec5cfcc-55c1-4ea9-9865-46fdd5420bfd)


با بررسی کد فوق مشاهده می‌کنیم که هر چه عمق بیشتر می‌شود و متغیر‌های بیشتری تعریف می‌شود، آدرس کوچک‌تر می‌شود. چراکه stack از بالا به پایین است.

[hw6.zip](https://github.com/user-attachments/files/16572994/hw6.zip)


