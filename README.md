# CS281-Assignment-5
Hongtao Hua
Assignment 5
1.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 5:100,5:100 -c -p
Time     PID: 0     PID: 1        CPU        IOs 
  1     RUN:cpu      READY          1            
  2     RUN:cpu      READY          1            
  3     RUN:cpu      READY          1            
  4     RUN:cpu      READY          1            
  5     RUN:cpu      READY          1            
  6        DONE    RUN:cpu          1            
  7        DONE    RUN:cpu          1            
  8        DONE    RUN:cpu          1            
  9        DONE    RUN:cpu          1            
 10        DONE    RUN:cpu          1            

Stats: Total Time 10
Stats: CPU Busy 10 (100.00%)
	
The CPU utilization should be 100%. It should be 100% since there is no I/O happening in the instruction. I used python process-run.py –l 5:100,5:100 –c –p  to know this. 

2.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 4:100,1:0 -c -p
Time     PID: 0     PID: 1        CPU        IOs 
  1     RUN:cpu      READY          1            
  2     RUN:cpu      READY          1            
  3     RUN:cpu      READY          1            
  4     RUN:cpu      READY          1            
  5        DONE     RUN:io          1            
  6        DONE    WAITING                     1 
  7        DONE    WAITING                     1 
  8        DONE    WAITING                     1 
  9        DONE    WAITING                     1 
 10*       DONE       DONE                       

Stats: Total Time 10
Stats: CPU Busy 5 (50.00%)
	
It takes total time 10 to complete both processes. 

3.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 1:0,4:100 -c -p
Time     PID: 0     PID: 1        CPU        IOs 
  1      RUN:io      READY          1            
  2     WAITING    RUN:cpu          1          1 
  3     WAITING    RUN:cpu          1          1 
  4     WAITING    RUN:cpu          1          1 
  5     WAITING    RUN:cpu          1          1 
  6*       DONE       DONE                       

Stats: Total Time 6
Stats: CPU Busy 5 (83.33%)
Stats: IO Busy  4 (66.67%)

The total time changes to 6. Because in this order, when the first process is on the waiting status, the second process can run and thus saves 4 time. 

4.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 1:0,4:100 -c -S SWITCH_ON_END
Time     PID: 0     PID: 1        CPU        IOs 
  1      RUN:io      READY          1            
  2     WAITING      READY                     1 
  3     WAITING      READY                     1 
  4     WAITING      READY                     1 
  5     WAITING      READY                     1 
  6*       DONE    RUN:cpu          1            
  7        DONE    RUN:cpu          1            
  8        DONE    RUN:cpu          1            
  9        DONE    RUN:cpu          1     
The system does not switch to the second process and has to wait the first I/O to finish before the second process can run.  The total time is 9.

5.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 1:0,4:100 -c -S SWITCH_ON_IO -p
Time     PID: 0     PID: 1        CPU        IOs 
  1      RUN:io      READY          1            
  2     WAITING    RUN:cpu          1          1 
  3     WAITING    RUN:cpu          1          1 
  4     WAITING    RUN:cpu          1          1 
  5     WAITING    RUN:cpu          1          1 
  6*       DONE       DONE                       

Stats: Total Time 6
Stats: CPU Busy 5 (83.33%)
Stats: IO Busy  4 (66.67%)

The system switches to the second process when the first process is waiting.  After using –p, terminal tells that the total time is 6. 
6.vagrant@Xubuntu-Vagrant:~/CS3281/scheduling-simulator$ python process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH_ON_IO -I IO_RUN_LATER -c -p
Time     PID: 0     PID: 1     PID: 2     PID: 3        CPU        IOs 
  1      RUN:io      READY      READY      READY          1            
  2     WAITING    RUN:cpu      READY      READY          1          1 
  3     WAITING    RUN:cpu      READY      READY          1          1 
  4     WAITING    RUN:cpu      READY      READY          1          1 
  5     WAITING    RUN:cpu      READY      READY          1          1 
  6*      READY    RUN:cpu      READY      READY          1            
  7       READY       DONE    RUN:cpu      READY          1            
  8       READY       DONE    RUN:cpu      READY          1            
  9       READY       DONE    RUN:cpu      READY          1            
 10       READY       DONE    RUN:cpu      READY          1            
 11       READY       DONE    RUN:cpu      READY          1            
 12       READY       DONE       DONE    RUN:cpu          1            
 13       READY       DONE       DONE    RUN:cpu          1            
 14       READY       DONE       DONE    RUN:cpu          1            
 15       READY       DONE       DONE    RUN:cpu          1            
 16       READY       DONE       DONE    RUN:cpu          1            
 17      RUN:io       DONE       DONE       DONE          1            
 18     WAITING       DONE       DONE       DONE                     1 
 19     WAITING       DONE       DONE       DONE                     1 
 20     WAITING       DONE       DONE       DONE                     1 
 21     WAITING       DONE       DONE       DONE                     1 
 22*     RUN:io       DONE       DONE       DONE          1            
 23     WAITING       DONE       DONE       DONE                     1 
 24     WAITING       DONE       DONE       DONE                     1 
 25     WAITING       DONE       DONE       DONE                     1 
 26     WAITING       DONE       DONE       DONE                     1 
 27*       DONE       DONE       DONE       DONE                       

Stats: Total Time 27
Stats: CPU Busy 18 (66.67%)
Stats: IO Busy  12 (44.44%)
	
The first IO process waits till the following three process all finished and then it finishes. The total is 27. 
