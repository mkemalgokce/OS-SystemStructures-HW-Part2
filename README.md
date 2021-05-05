# OS-SystemStructures-HW-Part2

## I. Writing to the /proc File System
In the kernel module project in Chapter 2, you learned how to read from the
/proc file system. We now cover how to write to /proc. Setting the field
.write in struct file operations to
.write = proc write
causes the proc write() function of Figure 3.37 to be called when a write
operation is made to /proc/pid

The kmalloc() function is the kernel equivalent of the user-level mal-
loc() function for allocating memory, except that kernel memory is being

allocated. The GFP KERNEL flag indicates routine kernel memory allocation.

The copy from user() function copies the contents of usr buf (which con-
tains what has been written to /proc/pid) to the recently allocated kernel

memory. Your kernel module will have to obtain the integer equivalent of this
value using the kernel function kstrtol(), which has the signature
int kstrtol(const char *str, unsigned int base, long *res)
This stores the character equivalent of str, which is expressed as a base into
res.
Finally, note that we return memory that was previously allocated with

kmalloc() back to the kernel with the call to kfree(). Careful memory man-
agement—which includes releasing memory to prevent memory leaks—is

crucial when developing kernel-level code.

## II. Reading from the /proc File System

Once the process identifier has been stored, any reads from /proc/pid
will return the name of the command, its process identifier, and its state.
As illustrated in Section 3.1, the PCB in Linux is represented by the
structure task struct, which is found in the <linux/sched.h> include
file. Given a process identifier, the function pid task() returns the associated
task struct. The signature of this function appears as follows:
struct task struct pid task(struct pid *pid,
enum pid type type)
The kernel function find vpid(int pid) can be used to obtain the struct
pid, and PIDTYPE PID can be used as the pid type.
For a valid pid in the system, pid task will return its task struct. You
can then display the values of the command, pid, and state. (You will probably
have to read through the task struct structure in <linux/sched.h> to obtain
the names of these fields.)
If pid task() is not passed a valid pid, it returns NULL. Be sure to perform
appropriate error checking to check for this condition. If this situation occurs,
the kernel module function associated with reading from /proc/pid should
return 0.

In the source code download, we give the C program pid.c, which pro-
vides some of the basic building blocks for beginning this project.
