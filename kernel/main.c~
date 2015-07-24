
/*++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
                            main.c
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
                                                    Forrest Yu, 2005
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*/

#include "type.h"
#include "const.h"
#include "protect.h"
#include "string.h"
#include "proc.h"
#include "tty.h"
#include "console.h"
#include "global.h"
#include "proto.h"

PUBLIC void clear_screen(int pos, int len);
PUBLIC void clear();
/*======================================================================*
                            kernel_main
 *======================================================================*/
PUBLIC int kernel_main()
{
	disp_str("-----\"kernel_main\" begins-----");

	TASK*		p_task		= task_table;
	PROCESS*	p_proc		= proc_table;
	char*		p_task_stack	= task_stack + STACK_SIZE_TOTAL;
	u16		selector_ldt	= SELECTOR_LDT_FIRST;
	int i;
        u8              privilege;
        u8              rpl;
        int             eflags;
	for (i = 0; i < NR_TASKS+NR_PROCS; i++) {
                if (i < NR_TASKS) {     /* 任务 */
                        p_task    = task_table + i;
                        privilege = PRIVILEGE_TASK;
                        rpl       = RPL_TASK;
                        eflags    = 0x1202; /* IF=1, IOPL=1, bit 2 is always 1 */
                }
                else {                  /* 用户进程 */
                        p_task    = user_proc_table + (i - NR_TASKS);
                        privilege = PRIVILEGE_USER;
                        rpl       = RPL_USER;
                        eflags    = 0x202; /* IF=1, bit 2 is always 1 */
                }

		strcpy(p_proc->p_name, p_task->name);	// name of the process
		p_proc->pid = i;			// pid

		p_proc->ldt_sel = selector_ldt;

		memcpy(&p_proc->ldts[0], &gdt[SELECTOR_KERNEL_CS >> 3],
		       sizeof(DESCRIPTOR));
		p_proc->ldts[0].attr1 = DA_C | privilege << 5;
		memcpy(&p_proc->ldts[1], &gdt[SELECTOR_KERNEL_DS >> 3],
		       sizeof(DESCRIPTOR));
		p_proc->ldts[1].attr1 = DA_DRW | privilege << 5;
		p_proc->regs.cs	= (0 & SA_RPL_MASK & SA_TI_MASK) | SA_TIL | rpl;
		p_proc->regs.ds	= (8 & SA_RPL_MASK & SA_TI_MASK) | SA_TIL | rpl;
		p_proc->regs.es	= (8 & SA_RPL_MASK & SA_TI_MASK) | SA_TIL | rpl;
		p_proc->regs.fs	= (8 & SA_RPL_MASK & SA_TI_MASK) | SA_TIL | rpl;
		p_proc->regs.ss	= (8 & SA_RPL_MASK & SA_TI_MASK) | SA_TIL | rpl;
		p_proc->regs.gs	= (SELECTOR_KERNEL_GS & SA_RPL_MASK) | rpl;

		p_proc->regs.eip = (u32)p_task->initial_eip;
		p_proc->regs.esp = (u32)p_task_stack;
		p_proc->regs.eflags = eflags;

		p_proc->nr_tty = 0;

		p_task_stack -= p_task->stacksize;
		p_proc++;
		p_task++;
		selector_ldt += 1 << 3;
	}

	proc_table[0].ticks = proc_table[0].priority = 15;
	proc_table[1].ticks = proc_table[1].priority =  5;
	proc_table[2].ticks = proc_table[2].priority =  5;
	proc_table[3].ticks = proc_table[3].priority =  5;

        proc_table[1].nr_tty = 0;
        proc_table[2].nr_tty = 1;
        proc_table[3].nr_tty = 1;

	k_reenter = 0;
	ticks = 0;

	p_proc_ready	= proc_table;

	init_clock();
    init_keyboard();
	restart();
	

	while(1){}
}

/*======================================================================*
                               TestA
 *======================================================================*/


void TestA()
{	
	int i = 0;
	while (1) {
                char n[5]="\n";
        char a[200] = "********************\n 1.What kind of fruit do you prefer to eat?\nA, strawberry\nB, apple\nC, watermelon\nD, pineapple \nE, orange \n ********************\n";
		char b[200] = "********************\n 2, Where do you like to go?\nA, suburban \nB, cinema \nC, Park \nD, shopping malls \nE, bar \nF, practice song room\n********************\n";
                char c[200] = "********************\n 3.Which one do you love? \nA, talented people \nB, who rely on you \nC, elegant people \nD, good people \nE, outgoing people\n********************\n";
                char d[200] = "********************\n 4, What kind of animal do you want to be?\nA, cat \nB, ma \nC, elephant \nD, monkey \nE, dog\nF, lion \n********************\n";
                char e[200] = "********************\n 5, The weather is very hot, which way are u more willing to choose ?\nA, swimming\nB, have a cold drink \nC, open air conditioning \n********************\n";
                char f[200] = "********************\n 6. If you have to live with an animal or insect that you hate, which one you can tolerate?\nA, snake \nB, pig\nC, mouse\nD, flies \n********************\n";
                char g[200] = "********************\n 7, what kind of movies do you like to see ?\nA, class of mystery\nB, fairy tale\nC, natural science\nD, ethics and Morals\nE, war gun battle class\n********************\n";
                char h[200] = "********************\n 8, Which one you will take with yourself?\nA, lighter\nB, lipstick\nC, Notepad \nD, tissue \nE, cell phone \n********************\n";
                char i[200] = "********************\n 9, when you travel,what kind of transport do you like to choose?\nA, train\nB, bicycle\nC, car\nD, aircraft\nE, walking\n********************\n";
                char j[200] = "********************\n 10. Choose one of your favorite sports?\nA, yoga \nB, bicycle\nC, table tennis\nD, Boxing \nE, football\nF,bungee jumping\n********************\n";
                char k[200] = "********************\n 11, if you have a villa, where should it be established?\nA, Lake \nB, grassland \nC, the sea \nD, forest \nE, urban area \n********************\n";
                char l[200] = "********************\n 12,What kind of weather do you prefer?\nA, snow\nB, wind\nC, rain\nD, fog \nE, thunder and lightning \n********************\n";
                char m[200] = "********************\n 13, Which floor you want to stay in a 30 floors building?\nA, seven\nB, a layer of \nC, twenty-three \nD, eighteen\nE, thirty\n********************\n";
                char o[200] = "********************\n 14. Which city do you want to stay?\nA, Lijiang \nB, Lhasa \nC, Kun Ming\nD, Xi'an\nE, Hangzhou\nF, Beijing \n***********************************\n";
        /*char p[200] ="Strong, calm, have a strong desire to lead, ambitious, and never give up. Looks nice outside, chesty inside, be conducive to their interpersonal relationship , sometimes appear to impatience, aggressive, be not conducive to their tenacious struggle, do not admit failure easily. The view of love and marriage is very realistic, the desire for money is generally";
        char q[200] ="Smart, lively, nice, good at making friends. Strong, eager to succeed. Mind is more rational, and beadvocating love, but when love and marriage in the conflict ,it will help their marriage. Lust for money.";
        char r[200] ="Love fantasy, thinking is more perceptual. Character appear more aloof, sometimes looks more impatience, sometimes looks irresolute and hesitant. Strong career-mind, like to have a creative work, do not like to do the routine work. Character is stubborn, language is sharp, do not like compromise. Advocating romantic love, but the idea is often not practical. ";
       
        char s[200] ="A gentle, heavy friendship, character dependably steady but sometimes is more cunning. Cause heart and on the job can be taken seriously, but outside of their professional things no much interest, like regular work and life, don't like to take risks, strong family values, relatively good at financial management.";*/
        /*char t[200] ="Rambling, playful, imaginative. Clever and clever, treat people with enthusiasm, love to make friends, but no strict selection criteria for friends. The cause of poor heart, better enjoy life, willpower and patience are poor, persist in one's old ways. Have good sex, but the love is not enough to adhere to the serious, easy to compromise. No concept of property";
          */      
                int score=0;
		getch = 'z';
                clear();
                printf(n);
		printf(a);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
				clear();
                                printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		
                printf(b);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'||getch=='f'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                else if(getch=='f'){score=score+20;}
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(c);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
				clear();
                                printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(d);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'||getch=='f'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                else if(getch=='f'){score=score+20;}
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(e);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'){
                                if(getch=='a'){score=score+5;}
                                else if(getch=='b'){score=score+10;}
                                else if(getch=='c'){score=score+15;}
                                clear();
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(f);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+5;}
                                else if(getch=='c'){score=score+10;}
                                else if(getch=='d'){score=score+15;}
                              
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(g);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                clear();
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(h);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+2;}
                                else if(getch=='c'){score=score+3;}
                                else if(getch=='d'){score=score+5;}
                                else if(getch=='e'){score=score+10;}
                               
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(i);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                clear();
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
		printf(j);
		while(1)
		{
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'||getch=='f'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+8;}
                                else if(getch=='e'){score=score+10;}
                                else if(getch=='f'){score=score+15;}
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
		}
               printf(k);
               while(1)
               {
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                clear();
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
              }
              printf(l);
              while(1)
              {
	 		if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                               
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
              }
              printf(m);
              while(1)
              {
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'){
                                if(getch=='a'){score=score+2;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+10;}
                                else if(getch=='e'){score=score+15;}
                                clear();
				printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

	  		}
              }
              printf(o);
              while(1)
              {
			if(getch != 'z')
			{       
                             if(getch=='a'||getch=='b'||getch=='c'||getch=='d'||getch=='e'||getch=='f'){
                                if(getch=='a'){score=score+1;}
                                else if(getch=='b'){score=score+3;}
                                else if(getch=='c'){score=score+5;}
                                else if(getch=='d'){score=score+8;}
                                else if(getch=='e'){score=score+10;}
                                else if(getch=='f'){score=score+15;}
				//clear();
                                printf(n);
		                getch = 'z';                                
                                break;}
                            else{printf(n);printf("error! Please try again."); printf(n); getch= 'z';}

			}
             }

             
             if(score>=180){
                           
                           printf("Strong, calm, have a strong desire to lead, ambitious, and never give up. Looks nice outside, chesty inside, be conducive to their interpersonal relationship , sometimes appear to impatience, aggressive, be not conducive to their tenacious struggle, do not admit failure easily. The view of love and marriage is very realistic, the desire for money is generally");

                           }
             else if(score>=140&&score<=179){
                           printf("Smart, lively, nice, good at making friends. Strong, eager to succeed. Mind is more rational, and beadvocating love, but when love and marriage in the conflict ,it will help their marriage. Lust for money.");
                           }
             else if(score>=100&&score<=139){
                           printf("Love fantasy, thinking is more perceptual. Character appear more aloof, sometimes looks more impatience, sometimes looks irresolute and hesitant. Strong career-mind, like to have a creative work, do not like to do the routine work. Character is stubborn, language is sharp, do not like compromise. Advocating romantic love, but the idea is often not practical. ");
                           }
             else if(score>=40&&score<=100){
                           printf("A gentle, heavy friendship, character dependably steady but sometimes is more cunning. Cause heart and on the job can be taken seriously, but outside of their professional things no much interest, like regular work and life, don't like to take risks, strong family values, relatively good at financial management.");
                           }
             else{
                           printf("Rambling, playful, imaginative. Clever and clever, treat people with enthusiasm, love to make friends, but no strict selection criteria for friends. The cause of poor heart, better enjoy life, willpower and patience are poor, persist in one's old ways. Have good sex, but the love is not enough to adhere to the serious, easy to compromise. No concept of property");
                           }

             while(1){}


	}
}

/*======================================================================*
                               TestB
 *======================================================================*/
void TestB()
{
	int i = 0x1000;
	while(1){
		milli_delay(10);
	}
}

/*======================================================================*
                               TestC
 *======================================================================*/
void TestC()
{
	int i = 0x2000;
	while(1){
		milli_delay(10);
	}
}


/*****************************************************************************
 *                                clear_screen
 *****************************************************************************/
/**
 * Write whitespaces to the screen.
 * 
 * @param pos  Write from here.
 * @param len  How many whitespaces will be written.
 *****************************************************************************/
PUBLIC void clear_screen(int pos, int len)
{
	u8 * pch = (u8*)(V_MEM_BASE + pos * 2);
	while (--len >= 0) {
		*pch++ = ' ';
		*pch++ = DEFAULT_CHAR_COLOR;
	}
}

/*======================================================================*
                               clear
 *======================================================================*/
PUBLIC void clear()
{
	clear_screen(0,console_table[nr_current_console].cursor);
	console_table[nr_current_console].current_start_addr = 0;
	console_table[nr_current_console].cursor = 0;
	
}
