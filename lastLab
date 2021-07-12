ORG 0

setb p3.4
mov 33h,#15;freq
mov 34h,#128
mov 35h,#20;süre
mov 36h,#6
mov dptr,#str1
acall clearr
acall loop2
LOOP:
	acall clearr
	DATA_LOOP:
		movc a,@a+dptr
		jz qqq
		acall SEND_DATA
		clr a
		inc dptr
		sjmp DATA_LOOP
	qqq:
	mov b,r6
	mov r2,b
	mov b,r7
	mov r3,b
	delayx: 
	mov b,r4
	mov r0,b
	DELAY_OUTER_LOOP1:
	mov b,r5
	mov r1,b
	djnz r1,$
	djnz r0,DELAY_OUTER_LOOP1
	cpl p3.4
	djnz r2,delayx
	djnz r3,newBreak
	ret 
	
newBreak:
	mov b,r6
	mov r2,b
	acall qqq

loop2:
	mov dptr,#str1
	mov r6,#255
	mov r4,#15
	mov r5,#128
	mov r7,#1
	acall loop

loopwait:
	mov dptr,#str2
	mov 33h,#200
	outer:
	mov 34h,#128
	djnz 34h,$
	djnz 33h,outer

loop3:
	mov dptr,#str3
	mov r6,#40
	mov r4,#10
	mov r5,#150
	mov r7,#6
	acall loop


loopwait2:
	mov dptr,#str4
	mov 33h,#30
	outer1:
	mov 34h,#128
	djnz 34h,$
	djnz 33h,outer1

	
CONFIGURE_LCD: 
mov a,#38H ;TWO LINES, 5X7 MATRIX
acall SEND_COMMAND
mov a,#0FH ;DISPLAY ON, CURSOR BLINKING
acall SEND_COMMAND
mov a,#06H ;INCREMENT CURSOR (SHIFT CURSOR TO RIGHT)
acall SEND_COMMAND
mov a,#01H ;CLEAR DISPLAY SCREEN
acall SEND_COMMAND
mov a,#80H ;FORCE CURSOR TO BEGINNING OF THE FIRST LINE
acall SEND_COMMAND
ret


;P1.0-P1.7 ARE CONNECTED TO LCD DATA PINS D0-D7
;P3.5 IS CONNECTED TO RS
;P3.6 IS CONNECTED TO R/W
;P3.7 IS CONNECTED TO E

SEND_COMMAND: ;THIS  SUBROUTINE IS FOR SENDING THE COMMANDS TO LCD
mov p1,a ;THE COMMAND IS STORED IN A, SEND IT TO LCD
clr p3.5 ;RS=0 BEFORE SENDING COMMAND
clr p3.6 ;R/W=0 TO WRITE
setb p3.7 ;SEND A HIGH TO LOW SIGNAL TO ENABLE PIN
acall DELAY
clr p3.7
ret

clearr:
acall CONFIGURE_LCD
clr a
ret

SEND_DATA: ;THIS  SUBROUTINE IS FOR SENDING THE DATA TO BE DISPLAYED
mov p1,a ;SEND THE DATA STORED IN A TO LCD
setb p3.5 ;RS=1 BEFORE SENDING DATA
clr p3.6 ;R/W=0 TO WRITE
setb p3.7 ;SEND A HIGH TO LOW SIGNAL TO ENABLE PIN
acall DELAY
clr p3.7
ret


DELAY: ;A SHORT DELAY SUBROUTINE
push 0
push 1
mov r0,#1
DELAY_OUTER_LOOP:
mov r1,#255
djnz r1,$
djnz r0,DELAY_OUTER_LOOP
pop 1
pop 0
ret

str1: DB ' 120Hz',0
str2: DB ' 1.5KHz',0
str3: DB ' 240Hz',0
str4: DB ' 0Hz',0

END

