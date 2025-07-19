
- [x] Read Session Contents.

### Section 3.3.1
- [x] Investigate the /proc/ directory
 ![image](https://github.com/user-attachments/assets/11ca5ea3-614a-4b3a-83c7-19bcc4bcb4ab)

### Section 3.3.2

- [x] Do 5 subtasks from 1 to 5:

1- توضیحات خوانده شد.

2- نشان‌دهنده ورژن سیستم‌عامل (کرنل) است:
![image](https://github.com/user-attachments/assets/0191d934-cf69-4606-b810-a77958e442db)
  

3- زمان uptime در uptime موجود است:
![image](https://github.com/user-attachments/assets/edd05a1a-c44e-41e2-b208-b67979d61f52)


همچنین مشخصات پردازنده در cpuinfo موجود است:
![image](https://github.com/user-attachments/assets/db2ba956-0085-46c5-8f04-5358f2f174a0)


    
    4- کد به صورت زیر است:

 ```c
 #include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *srcFile, *destFile;
    char buffer[256];

    // Open the source file for reading
    srcFile = fopen("/proc/version", "r");
    if (srcFile == NULL) {
        perror("Error opening /proc/version");
        return EXIT_FAILURE;
    }

    // Open the destination file for writing
    destFile = fopen("LinuxVersion.txt", "w");
    if (destFile == NULL) {
        perror("Error creating LinuxVersion.txt");
        fclose(srcFile);
        return EXIT_FAILURE;
    }

    // Read from the source file and write to the destination file
    while (fgets(buffer, sizeof(buffer), srcFile) != NULL) {
        fputs(buffer, destFile);
    }

    // Close the files
    fclose(srcFile);
    fclose(destFile);

    printf("Content of /proc/version has been written to LinuxVersion.txt\n");
    return EXIT_SUCCESS;
}

 ```
نتایج اجرای کد فوق به صورت زیر خواهد بود:
![image](https://github.com/user-attachments/assets/9f9533e2-0276-4a3f-b799-2bbea8632472)

 5- این کار ممکن نیست و به ارور permission می‌خوریم:
![image](https://github.com/user-attachments/assets/1c5c9613-5346-48c6-a3e9-e9b3d83daacd)
 
کد به زبان c هم نمی‌تواند محتوای read-only این فایل‌ها را تغییر دهد:
![image](https://github.com/user-attachments/assets/e78d9490-2ef6-42f7-ae6f-bc35902d9b76)


## Section 3.3.3

- [x] Write (in English or Persian) about each file in /proc/(PID) directory:

Sure, here is a description of each file found in the `/proc/(PID)` directory in a Linux system:

1. **cmdline**:
This file contains the command line arguments passed to the process when it was started. The arguments are stored as a null-separated string.

2. **environ**:
This file contains the environment variables that were set for the process. The variables are stored as a null-separated list.

3. **stat**:
This file contains detailed information about the process. It provides a large amount of data, including the process state, process ID, parent process ID, CPU time used, and more. The fields are space-separated and require interpretation based on the `man proc` documentation.


4. **status**:
This file provides more readable and structured information about the process compared to `stat`. It includes details such as the process state, memory usage, user and group IDs, and more, presented in a human-readable format.

5. **statm**:
This file contains information about the process's memory usage. It includes details such as total program size, resident set size, shared pages, and more. The values are space-separated and are presented in pages, not bytes.

6. **cwd**:
This is a symbolic link to the current working directory of the process. By examining this link, you can determine the directory in which the process is currently operating.

7. **exe**:
This is a symbolic link to the executable binary of the process. It points to the binary file that is running the process. This can be useful to determine which executable started the process.

8. **root**:
This is a symbolic link to the root directory as seen by the process. It is particularly useful for chrooted environments where the root directory might be different from the system's root.


- [x] Place your script for shwoing PID of running porcesses and their name here:

```bash
#!/bin/bash

# Print the header
printf "%-10s %-s\n" "PID" "Process Name"

# Iterate through each directory in /proc
for pid in /proc/[0-9]*; do
    # Extract the process ID from the directory name
    pid=${pid#/proc/}
    
    # Read the process name from /proc/<PID>/comm
    if [ -r "/proc/$pid/comm" ]; then
        process_name=$(cat /proc/$pid/comm)
        # Print the process ID and name
        printf "%-10s %-s\n" "$pid" "$process_name"
    fi
done
```

خروجی اجرای این اسکریپت به صورت زیر خواهد بود:
![image](https://github.com/user-attachments/assets/97cfce6b-1378-462b-a50f-ae3f597203c9)



- [x] Place your source code for a program that shows details of a program by receiving PID:
```bash
#!/bin/bash

# Check if PID is provided
if [ -z "$1" ]; then
    echo "Usage: $0 <PID>"
    exit 1
fi

PID=$1

# Check if the process exists
if [ ! -d "/proc/$PID" ]; then
    echo "Process with PID $PID does not exist."
    exit 1
fi

# Get process name
if [ -r "/proc/$PID/comm" ]; then
    PROCESS_NAME=$(cat /proc/$PID/comm)
else
    PROCESS_NAME="N/A"
fi

# Get executable path
if [ -r "/proc/$PID/exe" ]; then
    EXECUTABLE_PATH=$(readlink -f /proc/$PID/exe)
else
    EXECUTABLE_PATH="N/A"
fi

# Get memory usage
if [ -r "/proc/$PID/statm" ]; then
    MEMORY_USAGE=$(awk '{print $2 * 4096}' /proc/$PID/statm) # Convert pages to bytes (assuming 4KB pages)
else
    MEMORY_USAGE="N/A"
fi

# Get command line arguments
if [ -r "/proc/$PID/cmdline" ]; then
    CMDLINE_ARGS=$(tr '\0' ' ' < /proc/$PID/cmdline)
else
    CMDLINE_ARGS="N/A"
fi

# Get environment variables
if [ -r "/proc/$PID/environ" ]; then
    ENV_VARS=$(tr '\0' '\n' < /proc/$PID/environ)
else
    ENV_VARS="N/A"
fi

# Print the information
echo "Process Information for PID $PID:"
echo "---------------------------------"
echo "Process Name: $PROCESS_NAME"
echo "Executable Path: $EXECUTABLE_PATH"
echo "Memory Usage (in bytes): $MEMORY_USAGE"
echo "Command Line Arguments: $CMDLINE_ARGS"
echo "Environment Variables:"
echo "$ENV_VARS"
```

 خروجی اسکریپت بالا برای یک پردازه دلخواه به صورت زیر خواهد بود:

![image](https://github.com/user-attachments/assets/213e3feb-b6fb-4683-85e2-85b378ca79db)


### Section 3.3.4

- [x] Write (in English or Persian) about each file in /proc/ directory:

The `/proc` directory in Linux is a virtual filesystem that provides a mechanism for the kernel to communicate with userspace. It contains files and directories that give insights into the system's hardware and the kernel's state. Below are descriptions of some key files in the `/proc` directory:

1. **meminfo**
    - The `/proc/meminfo` file provides detailed information about the system's memory usage. It includes data on total memory, free memory, available memory, buffers, cached memory, and swap space, among other metrics. This file is crucial for monitoring and managing memory resources on the system.

2. **version**
    - The `/proc/version` file contains information about the Linux kernel version, the GCC version used to compile the kernel, and the build timestamp. This file is useful for identifying the specific kernel version and compiler details, which can be important for troubleshooting and system management.

3. **uptime**
    - The `/proc/uptime` file displays the system's uptime and idle time in seconds. The first value indicates the total uptime since the last reboot, and the second value represents the amount of time all CPUs have been idle. This information is helpful for monitoring system stability and performance.

4. **stat**
    - The `/proc/stat` file provides various statistics about the system since it was booted. This includes CPU usage, interrupts, context switches, boot time, and processes created. The data in this file can be used for performance monitoring and analysis.

5. **mounts**
    - The `/proc/mounts` file lists all the currently mounted filesystems. Each line represents a mounted filesystem and includes details such as the device, mount point, filesystem type, and mount options. This file is essential for understanding the current state of the filesystem hierarchy.

6. **net directory**
    - The `/proc/net` directory contains various files and directories related to networking. It includes information about network protocols, interfaces, connections, and routing tables. This directory is vital for network diagnostics and management.

7. **loadavg**
    - The `/proc/loadavg` file provides the system load averages for the past 1, 5, and 15 minutes, along with the number of currently running processes and the total number of processes. It also includes the last process ID. This file is useful for assessing the system's load and performance over time.

8. **interrupts**
    - The `/proc/interrupts` file lists the number of interrupts per CPU per interrupt line. It includes information about the IRQ (Interrupt Request) lines, the number of occurrences, and the associated devices or drivers. This file is helpful for debugging hardware issues and understanding the distribution of interrupt handling.

9. **ioports**
    - The `/proc/ioports` file shows the range of I/O port addresses used by the devices on the system. It details which ports are reserved by which drivers or devices. This information is important for understanding hardware resource allocation and potential conflicts.

10. **filesystems**
    - The `/proc/filesystems` file lists the filesystems supported by the kernel. Each line represents a filesystem type, with a possible prefix indicating whether the filesystem type supports block devices or not. This file is useful for understanding which filesystem types the kernel can handle.

11. **cpuinfo**
    - The `/proc/cpuinfo` file provides detailed information about the CPU(s) on the system. It includes details such as the processor model, vendor ID, clock speed, cache size, and the number of cores. This file is crucial for understanding the hardware capabilities and configuration of the system's processors.

12. **cmdline**
    - The `/proc/cmdline` file contains the complete command line passed to the kernel at boot time. This includes kernel parameters and options. It is useful for troubleshooting boot issues and understanding the kernel's boot configuration.


- [x] Place your source code for a program that shows details of processor:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFFER_SIZE 1024

void get_cpu_info() {
    FILE *fp;
    char buffer[BUFFER_SIZE];
    char model_name[BUFFER_SIZE] = "Unknown";
    char cpu_mhz[BUFFER_SIZE] = "Unknown";
    char cache_size[BUFFER_SIZE] = "Unknown";
    
    fp = fopen("/proc/cpuinfo", "r");
    if (fp == NULL) {
        perror("Failed to open /proc/cpuinfo");
        exit(EXIT_FAILURE);
    }
    
    while (fgets(buffer, sizeof(buffer), fp) != NULL) {
        if (strncmp(buffer, "model name", 10) == 0) {
            sscanf(buffer, "model name : %[^\n]", model_name);
        } else if (strncmp(buffer, "cpu MHz", 7) == 0) {
            sscanf(buffer, "cpu MHz : %[^\n]", cpu_mhz);
        } else if (strncmp(buffer, "cache size", 10) == 0) {
            sscanf(buffer, "cache size : %[^\n]", cache_size);
        }
    }
    
    fclose(fp);
    
    printf("CPU Information:\n");
    printf("Model: %s\n", model_name);
    printf("Frequency: %s MHz\n", cpu_mhz);
    printf("Cache Size: %s\n", cache_size);
}

int main() {
    get_cpu_info();
    return 0;
}

```

خروجی آن به صورت زیر است:
![image](https://github.com/user-attachments/assets/d4563395-4e78-4156-9614-88f5503e560b)

- [x] Place your source code for a program that shows details of memory management sub-system:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFFER_SIZE 256

void get_memory_info() {
    FILE *fp;
    char buffer[BUFFER_SIZE];
    long total_memory = 0;
    long free_memory = 0;
    long available_memory = 0;

    fp = fopen("/proc/meminfo", "r");
    if (fp == NULL) {
        perror("Failed to open /proc/meminfo");
        exit(EXIT_FAILURE);
    }

    while (fgets(buffer, sizeof(buffer), fp) != NULL) {
        if (strncmp(buffer, "MemTotal:", 9) == 0) {
            sscanf(buffer, "MemTotal: %ld kB", &total_memory);
        } else if (strncmp(buffer, "MemFree:", 8) == 0) {
            sscanf(buffer, "MemFree: %ld kB", &free_memory);
        } else if (strncmp(buffer, "MemAvailable:", 13) == 0) {
            sscanf(buffer, "MemAvailable: %ld kB", &available_memory);
        }
    }

    fclose(fp);

    printf("Memory Information:\n");
    printf("Total Memory: %ld kB\n", total_memory);
    printf("Used Memory: %ld kB\n", total_memory - available_memory);
    printf("Free Memory: %ld kB\n", free_memory);
}

int main() {
    get_memory_info();
    return 0;
}
```

خروجی آن به صورت زیر است:
![image](https://github.com/user-attachments/assets/69c4b5d7-21ee-4c07-a2c7-025282c0bc42)

- [x] Write your description about five important files at /proc/sys/kernel:
    - *hostname:* name of the machine. (can be edited)
    - *ostype:* the type of os.
    - *osrelease:* the build version of the kernel.
    - *pid_max:* the maximum numerical PROCESS IDENTIFIER than can be assigned by the kernel.
    - *poweroff_cmd:* the path of power off command.

![image](https://github.com/user-attachments/assets/0d888fb3-752a-42c9-a8c7-97b8efa13ff2)


- [x] Write your description about /proc/self file
فایل self اشاره به پردازه در حال اجرای فعلی است که در حال خواندن فایل سیستم است. و کاربرد آن بدست آوردن اطلاعات پردازه فعلی است.


## Source Code Submission

please submit all your codes in a zip file
[hw3.zip](https://github.com/user-attachments/files/16236726/hw3.zip)
