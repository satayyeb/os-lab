
- [x] Read Session Contents.

## Section 7.4

### Section 7.4.1
- [x] Creating a thread using pthread

![image](https://github.com/user-attachments/assets/0737a421-049b-4351-a653-ae927a6e0d72)

- [x]  Checking the process ids

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

void *child(void *arg) {
    puts("Hello from child thread!");
    printf("[CHILD] PID: %d\n", getpid());
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, child, NULL);
    pthread_join(thread, NULL);
    printf("[PARENT] PID: %d\n", getpid());
    return 0;
}
```

![image](https://github.com/user-attachments/assets/5650798a-9fd7-42be-9c85-4a03aee22bde)


بله شماره پردازه ها یکسان است.


- [x]  Shared variables


```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

int oslab = 10;


void *child(void *arg) {
    oslab *= 3;
}

int main() {
    pthread_t thread;
    printf("[PARENT] oslab = %d\n", oslab);
    oslab *= 2;
    pthread_create(&thread, NULL, child, NULL);
    pthread_join(thread, NULL);
    oslab *= 5;
    printf("[PARENT] oslab = %d\n", oslab);
    return 0;
}
```

![image](https://github.com/user-attachments/assets/fa7a7f61-7746-4e88-9cd1-fd2dc7415526)

  
متغیر سراسری بین این دو مشترک است. و هر کدام کپی جداگانه‌ای ندارند. چرا که یک بار در ۲ یک بار در ۳ و یک بار در ۵ ضرب شده و حاصل از ۱۰ شده ۳۰۰.





- [x] Sum of 2 to n


```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

int oslab = 10;


void *child(void *arg) {
    int n = *(int*) arg;
    unsigned long long sum = 0;
    for (int i = 2; i <= n; i++)
        sum += i;
    printf("Sum of numbers from 2 to %d is: %llu\n", n, sum);
}

int main() {
    int n = 0;
    printf("Enter a number (n): ");
    while (1){
        scanf(" %d", &n);
        if (n >= 2)
            break;
        printf("Enter a number greater than 1: ");
    }
    
    pthread_t thread;

    pthread_attr_t attr;

    pthread_attr_init(&attr);
    
    pthread_create(&thread, &attr, child, &n);

    pthread_join(thread, NULL);
    
    pthread_attr_destroy(&attr);
    
    return 0;
}
```

![image](https://github.com/user-attachments/assets/bd6510af-d967-4d23-80e5-56f14dfa6ae5)

### Section 7.4.2
- [x] Multiple threads    

```c
#include <stdio.h>
#include <pthread.h>

void* print_hello(void* arg) {
    printf("Hello World!\n");
    pthread_exit(NULL);
}

int main() {
    int NUM = 5;
    pthread_t threads[NUM];

    for(int i = 0; i < NUM; i++)
        pthread_create(&threads[i], NULL, print_hello, NULL);

    for(int i = 0; i < NUM; i++)
        pthread_join(threads[i], NULL);

    return 0;
}
```

![image](https://github.com/user-attachments/assets/73f03749-0116-4c6a-b027-a5cceacb1f3b)



### Section 7.4.3
- [x] Compiling the code

قسمت بعدی را مشاهده کنید.

- [x] global_param

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

int global_var = 10;


void *child(void *arg) {
    int local_var;

    printf("Thread %ld, pid %d, addresses: &global: %p, &local: %p \n",
        pthread_self(), getpid(), &global_var, &local_var);

    global_var++;

    printf("Thread %ld, pid %d, incremented global var=%d\n",
        pthread_self(), getpid(), global_var);

    pthread_exit(0);
}

int main(){
    pthread_t t1, t2;

    printf("Thread %ld, pid %d, addresses: &global: %p \n",
        pthread_self(), getpid(), &global_var);

    printf("Thread %ld, pid %d, initial global var=%d\n",
        pthread_self(), getpid(), global_var);
    
    pthread_create(&t1, NULL, child, NULL);
    pthread_create(&t2, NULL, child, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Thread %ld, pid %d, addresses: &global: %p \n",
        pthread_self(), getpid(), &global_var);

    printf("Thread %ld, pid %d, final global var=%d\n",
        pthread_self(), getpid(), global_var);

    return 0;
}
```
 
![image](https://github.com/user-attachments/assets/ba9f8154-b24f-4970-b270-111e19888679)

مشاهده می‌کنیم که در ترد ها مقدار global_var مشترک است. (حافظه یکسانی دارد.) اما ترد ها متغیر‌های محلی (استک‌های) مختلفی دارند.

- [x] Forking

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>
#include <sys/wait.h>

int global_var = 10;


int main(){
    pthread_t t1, t2;

    int local_var = 1;
    
    printf("Thread %ld, pid %d, addresses: &global: %p, &local: %p \n",
        pthread_self(), getpid(), &global_var, &local_var);

    global_var++;
    local_var++;

    printf("Thread %ld, pid %d, incremented global var=%d incremented local var=%d\n",
        pthread_self(), getpid(), global_var, local_var);

    int id = fork();
    if (id == 0) {
        printf("Thread %ld, pid %d, addresses: &global: %p, &local: %p \n",
            pthread_self(), getpid(), &global_var, &local_var);

        global_var++;
        local_var++;

        printf("Thread %ld, pid %d, incremented global var=%d incremented local var=%d\n",
            pthread_self(), getpid(), global_var, local_var);
    }else{
        wait(NULL);

        printf("Thread %ld, pid %d, addresses: &global: %p, &local: %p \n",
            pthread_self(), getpid(), &global_var, &local_var);

        printf("Thread %ld, pid %d, final global var=%d  final local var=%d\n",
            pthread_self(), getpid(), global_var, local_var);
        }


    return 0;
}
```

![image](https://github.com/user-attachments/assets/3fb0ce83-d4e7-4329-92a7-9f5129166173)

همان‌طور که مشاهده می‌کنید fork دقیقا یک پروسه دیگر از روی پروسه اصلی کپی می‌کند. به همین دلیل است که آدرس‌های مجازی چاپ شده یکسان است. ولی در حافظه فیزیکی این‌ آدرس‌ها یکی نیستند و با تغییر متغیر در  child، مقدار آن متغیر در parent تغییر نمی‌کند. و در این قاعده فرقی بین متغیر لوکال (استک) و متغیر گلوبال (دیتا) وجود ندارد.

### Section 7.4.4
- [x] Passing multiple variables

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <memory.h>

typedef struct thdata {
    int thread_no;
    char message[100];
} stdata;


void* routine(void* arg){
    stdata* data = (stdata*) arg;
    printf("I am thread number %d and my message is %s\n", data->thread_no, data->message);
}

int main(){
    stdata* data1 = malloc(sizeof(stdata));
    stdata* data2 = malloc(sizeof(stdata));
    memcpy(data1->message, "hello world!", 12);
    memcpy(data2->message, "goodbye world!", 22);
    data1->thread_no = 1;
    data2->thread_no = 2;

    pthread_t t1, t2;

    pthread_create(&t1, NULL, routine, (void*)data1);
    pthread_create(&t2, NULL, routine, (void*)data2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```

![image](https://github.com/user-attachments/assets/69c77d82-c000-4a74-a3a6-cf22225022a7)


[hw7.zip](https://github.com/user-attachments/files/16639891/hw7.zip)


