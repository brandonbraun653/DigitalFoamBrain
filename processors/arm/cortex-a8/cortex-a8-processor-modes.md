# Cortex A8 Processor Modes

Source Document: [Cortex-A8 Technical Reference Manual](https://developer.arm.com/documentation/ddi0344/k/)

There are a total of eight modes of operation:
- [Cortex A8 Processor Modes](#cortex-a8-processor-modes)
  - [User Mode](#user-mode)
  - [Interrupt Modes](#interrupt-modes)
    - [Some Useful Properties of FIQ Mode](#some-useful-properties-of-fiq-mode)
    - [Downsides of FIQ Mode](#downsides-of-fiq-mode)
  - [Supervisor Mode](#supervisor-mode)
  - [System Mode](#system-mode)
  - [Abort Mode](#abort-mode)
  - [Undefined Mode](#undefined-mode)
  - [Monitor Mode](#monitor-mode)


## User Mode
This is the standard ARM execution state used for executing most of the application programs. It's the only
mode that is run as unpriviledged and software spends most of its time here. There is limited access to
system resources.

## Interrupt Modes
[arm - What is the difference between FIQ and IRQ interrupt system? - Stack Overflow](https://stackoverflow.com/questions/973933/what-is-the-difference-between-fiq-and-irq-interrupt-system)
There are two primary interrupt modes handled here, *fast interrupt* (FIQ) and *interrupt* (IRQ). The main difference
lies with the priority mechanisms of each, with the FIQ being handled with more urgency than the IRQ.

### Some Useful Properties of FIQ Mode
* It has it's own dedicated bank of registers (R8-R14) that can be used to not incur the overhead of push/pop for registers used
  by the ISR routine.
* The handler can rely on values persisting from one call to the next
* If the FIQ is placed at the end of the exception vector, it can execute directly without a branch, saving a few cycles
* When an FIQ occurs, it's at a higher priority than IRQ, so it cannot be interrupted. IRQ events are effectively masked.

### Downsides of FIQ Mode
* The handler generally is written in assembly because the compiler won't use the correct registers (R8-R14) for the mode
* Typically there is only a single interrupt source for generating an FIQ, which limits functionality.

## Supervisor Mode
This mode provides unrestricted access to system resources and is the first mode entered on power up. Allows enabling and disabling of interrupts
and appears to be very similar to system mode. Can only be entered via an SVC instruction.

## System Mode
Seems to be the same as supervisor mode from what it looks like online. There must be a distinction, but it doesn't seem clear yet.

## Abort Mode
This mode is entered when the processor attempts to access a non-existant memory location.

## Undefined Mode
This mode is entered when the processor has an issue executing an instruction, including ones that aren't defined.

## Monitor Mode
Monitor mode exists to help with debugging as well as changing between secure and non-secure states