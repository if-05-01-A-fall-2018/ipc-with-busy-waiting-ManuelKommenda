### 1 Race Condition

  A Race Condition is when two threads enter a critical region and they need to use free
  disk space. The data of one of the threads will be lost.

### 2 Disabling Interrupts

#### 2.1)

  On a multi-core machine each core handles different threads at the same time, when you
  interrupt or block a thread on the one core on an other if they run exactly parallel, they both can
  enter a critical region at the same time.

#### 2.2)

  When you give a user more power to prevent interrupt, it is possible he forgets to turn in on again,
  then the process will be down forever unless he remembers to activate it again.

### 3 Peterson's Solution

#### 3.1)

#####  Scenario 1

  Process 0 enters_region
    process = 0;
    other = 1;
    interested[process] = true;
    loser = process;

  Process 1 enters_region
    process = 1;
    other = 0;
    interested[process] = true;
    loser = process;
    while(loser == process && interested[other])


    both are interested
    loser = 1;
    Process 1 is interested, but also the loser he needs to wait in the while. Process 0 can just enter the CR
    and when he is finished call leave_region;
    Now Process 0 is not interested and Process 1 can enter the CR.


##### Scenario 2

    It is like the first scenario the loser variable can just be one process id, the only other difference   
    is that you can't say which one is the loser, so it is possible that loser = 0 and the second process
    will be able to enter the CR first.

#### 3.2)

    The only possibility that I see is that in a multi-core machine, when the two processes enter at the exact
    same time somehow the loser variable is for both there own id, but I am not sure if that could even happen!

#### 3.3)

    loser is very important, it is the last condition the process checks before entering the CR, because of it,
    it's not possible for two process to enter the CR at the same time. The effect shows when two processes are in the while,
    both want to enter but only one can because the loser blocks the other one.

#### 3.4)

    int loser //shard variable
    Bool interested[3]

    void enter_region(int process){
      int otherOne = 2 - process;
      if(process == 1) otherOne = 2;
      int otherTwo = 1 - process;
      if (process == 2) otherTwo = 1;

      interested[process] = true;
      loser = process;

      while(loser == process && (interested[otherOne] && interested[otherTwo]));

    }
