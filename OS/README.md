# OS Cheatsheet

Interrupt Service Routine (ISR): In I/O devices, one of the bus control lines is dedicated for this purpose and is called the Interrupt Service Routine (ISR).

multitasking: A single computer can perform only one computer instruction at a time. But, because it can be interrupted, it can manage how programs or sets of instructions will be performed. This is known as multitasking

interrupt handler: An operating system usually has some code that is called an interrupt handler. The interrupt handler prioritizes the interrupts and saves them in a queue if more than one is waiting to be handled. The operating system has another little program called a **scheduler** that figures out which program to control next.

Hyper-Threading: Hyper-Threading Technology is a hardware innovation that allows more than one thread to run on each core.