# Zadaca1-8085

Секој 30ms од изолирана порта со адреса 0Ah се чита
податок. Ако битовите 2 и 5 се 1 и 0 соодветно на мемориска
пресликана порта на адреса F00Ah се испраќа прочитаниот
податок поделен со 2, инаку се испраќа прочитаниот податок
помножен со 7. Фреквенцијата на кристалот на осцилаторот е
5MHz.


     If bit 2,5==1,0 
            F00Ah <- Data/2
     Else
            F00Ah <- Data*7


**Resenie:**
```
fosc=5MHz

Tosc=0,2 µsec ;Периодата е 1/f. Работната периода е 2 пати поголема.

Ts=2Tosc

Ts=0,4µsec

 DOCNI_1: MVI D,178d ;14 циклуси x 0,4 = 5,6 микросекунди x 178= 1ms
 
 DOCNI: DCR D ;4 циклуси
 
 JNZ DOCNI ;10 циклуси
 
 RET
 
 DOCNI_30: MVI E,30d ; 30 пати по 1 ms = 30 ms
 
 DOCNI_1: MVI D,178d ; јамка за 1ms
 
 DOCNI: DCR D
 
 JNZ DOCNI
 
 DCR E
 
 JNZ DOCNI_1
 
 RET
 
 START: CALL DOCNI_30 ;доцнење од 30 ms

 IN VLEZNA ;се вчитува податокот од I/O уред на адреса OAh
 
 MOV B,A
 
 ANI 00100000b ;се проверува 5-тиот дали е 0
 
 JNZ MNOZI_SO_7 ;доколку резултатот не е нула се скока на mnozi_so_7
 
MOV A,B

 ORI 11111011b ;се проверува дали битот 2 е единица
 
 CPI FFh ;ACC се споредува со FFh
 
 JNZ MNOZI_SO_7 ;доколку не е 0 се скока на mnozi_so_7
 
 DELI_SO_2: MOV A,B ; исполнети се двата услови
 
RRC ; ACC се дели со 2

 STA IZLEZNA ;резултатот се запишува на Mm порта
 
 JMP START ;безусловен скок на почеток

 MNOZI_SO_7:MVI C,7d ;множењето е реализирано со собирање
 
 MVI A,0
 
 PAK: ADD B
 
 DCR C
 
 JNZ PAK
 
 STA IZLEZNA ;резултатот се запишува на F00Ah

 JMP START
 
 END
 
 VLEZNA EQU OAh
 
 IZLEZNA EQU F00Ah 
```

 ![Screenshot (1)](https://github.com/slavko444/8085-Zadaca-1/blob/main/8085A.png)

 

**Develop by:**

[Tamara Ristova ](https://github.com/Ristova123)


**Subject**

Microcomputer's systems

**Built With**

This project is built using the following tools:

- [e8085](https://emu8086-microprocessor-emulator.en.softonic.com/): Assembler and emulator for the Intel 8085 microprocessor.

**Getting Started**

To get a local copy up and running, follow these steps.

**Prerequisites**

In order to run this project you need:

A working computer
Connection to internet
Setup

**How to Run**

To run the program, you need an 8085 emulator or assembler. You can use emulators like DOSBox or TASM (Turbo Assembler). Here's how to run the program using e8085.exe:

1. Download and install e8085.exe from [here](https://emu8086-microprocessor-emulator.en.softonic.com/).
2. Clone this repository to your local machine.
3. Open e8085.exe and load the `Zadaca 1 code.asm` file.
4. Assemble the code by pressing the Assemble button.
5. Run the program by pressing the Run button or by pressing F10.
