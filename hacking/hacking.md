gcc firstprogram.c
objdump -D -M intel file.bin | grep main.: -A20

# Starts the debugger

gdb -q ./a.o
run
break main

eax: accumulator
ecx: counter data
edx: data
ebx: base register

Those are temporarly register for the CPU when it is executing the. program.
bre
esp: stack pointer --> it is pointer because it stores 32 bits
ebp: base pointer
esi: source index
edi: destination index

eip; instruction pointer register


Intel syntax : <operation> <destination> , <source>

# Debugging with source code

gcc -g firstprogram.c --> It will include extra information on the ./a.out in order to access the source code from gdb

gdb -q ./a.out

set disassembly-flavor intel
list
disassemble main
break main
nexti

https://cs.ebay.com/ws/eBayAdmin.dll?VerificationAdminPage

# Examine a speciffic piece of memory

x/o $eip => print octal value of registry eip
x/x $eip => print hexadecimal value of registry eip
x/u $eip => print decimal value of registry eip
x/t $eip => print binary value of registry eip


# Programming with C

pointers could have a type associate or being void:

*char_pointer : the pointer value itself
&char_pointer : the memory address itself



```
# include <stdio.h>

char char_array = {'a', 'b', 'c', 'd', 'e'}

char *char_ponter

char_pointer = char_array

for(i = 0; i < 5; i++) {

printf("char_pointer points to %p which contains char %c/n", char_pointer, *char_pointer)
}

```


You can provide arguments to a program in th emain function

```
#include <stdio.h> void usage(char *program_name) {
    printf("Usage: %s <message> <# of times to repeat>\n", program_name); exit(1);
 }

 int main(int argc, char *argv[]) {
        int i, count;
        if(argc < 3)// If fewer than 3 arguments are used,

        usage(argv[0]); // display usage message and exit.
        count = atoi(argv[2]); // Convert the 2nd arg into an integer.
        printf("Repeating %d times..\n", count);
        for(i=0; i < count; i++)
            printf("%3d - %s\n", i, argv[1]); // Print the 1st arg.
}
```

# Memory Segmentation

A compiled program's memory is divided into five segments:

1. text: nobody can write in this segment
2. data
3. bss
4. heap
5. stack.

Each segment represents a special portion of memory that is set aside for a certain purpose.

The text segment is also sometimes called the code segment. This is where the assembled machine language instructions of the program are located. The execution of instructions in this segment is nonlinear, thanks to the aforementioned high-level control structures and functions, which compile into branch, jump, and call instructions in assembly language. As a program executes, the __EIP__ is set to the first instruction in the text segment. The processor then follows an execution loop that does the following:

1. Reads the instruction that EIP is pointing to
2. Adds the byte length of the instruction to EIP
3. Executes the instruction
4. Goes back to step 1

The data and bss segments are used to store global and static program variables. The data segment is filled with the initialized global and static variables, while the bss segment is filled with their uninitialized counterparts. Although these segments are writable, they also have a fixed size. Remember that global variables persist, despite the functional context (like the variable j in the previous examples). Both global and static variables are able to persist because they are stored in their own memory segments.

The heap segment is a segment of memory a programmer can directly control. Blocks of memory in this segment can be allocated and used for whatever the programmer might need. One notable point about the heap segment is that it isn't of fixed size, so it can grow larger

The stack segment also has variable size and is used as a temporary scratch pad to store local function variables and context during function calls. This is what GDB's backtrace command looks at. When a program calls a function, that function will have its own set of passed variables, and the function's code will be at a different memory location in the text (or code) segment.

