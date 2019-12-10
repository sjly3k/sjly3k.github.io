## [컴퓨터구조] Cache Overview

### 목표
  1. 너비 우선 탐색의 개념
  2. 너비 우선 탐색의 특징
  3. 너비 우선 탐색의 구현 
  
  
### Cache의 등장 배경
  * Cache란?
    - 특정 data에 대해 요청이 들어왔을때, 이 data를 빠르게 제공할 수 있게 도와주는 SW / HW 장치이다.
    
    
  * 지역성 (Locality)
    - Spatial(공간) Locality : 가장 최근에 사용한 item 근처의 item을 재사용할 가능성이 높다.
    - Temporal(일시적) Locality : 가장 최근에 사용한 item을 재사용할 가능성이 높다.
    
    
  * 이런 지역성을 활용하여 크고, 빠른 메모리를 지원할 수 있게된다.
        
        
### 메모리 Hierarchy
  - SRAM (Cache memory attached to CPU)
    - Most expensive, Smallest, Fastest
    
    
  - DRAM (Main memory)
    - Expensive, Smaller, Faster
    
    
  - Disk (HDD, SSD)
    - Cheapest, Biggest, Slowest

### Cache Hit / Miss
  * Cache Hit : Processor가 찾으려고 하는 data가 존재하는 (present) 경우
    - Hit ratio = # of hits / # of cache access
    - 우리는 이 Hit ratio를 높이는 것이 목표이다.
    
    
  * Cache Miss : Processor가 찾으려고 하는 data가 존재하지 않는 (absent) 경우
    - 따라서, lower-level로 부터 요청된 data를 복사해와야 한다. (추가적인 시간이 소모)
    - Miss ratio = 1 - Hit ratio
   
        
### Cache Memory에 접근하는 방법

  - Cache Memory : Cache Address | Valid Bit | Tag | Data
    - Valid Bit : 해당 Cache Address에 유효한 information이 저장되어 있는지 확인할 수 있는 Bit
      - Valid Bit = 1 : contains Valid information
      - Valid Bit = 0 : contains Invalid information
      - 초기에는 모든 Valid Bit이 0으로 초기화 되어있다.
      
      
  - Directed Mapped Cache (= 1-Way Set Associative Cache)
    - Memory location이 정확히 한 위치에만 mapping 될 수 있도록 하는 cache
    - Cache address = (block address) modulo (# of cache blocks)
    - 이때, (# of cache blocks)는 power of 2이다. (2, 4, 8, 16, ..)
    - Cache Size = # of Cache Block Size * (block size + valid bit + tag)
    
### Cache Block의 size를 키우면 어떻게 될까?
  - Locality에 의해 size를 키우면 miss rate를 줄일 수 있다고 생각할 수 있다.
  - 하지만, 실제로 block의 size를 키우면, 한정되어 있는 Cache의 크기로 인해 block의 개수가 줄어들고, 이로 인해 더 많은 경쟁이 일어나 Miss rate가 증가한다.
  
### Handling Cache Miss
  - 일반적으로 Cache Hit이 발생하면, Clock Cycle = 1이다.
  - 하지만, Cache Miss가 발생하면 Clock Cycle은 증가한다.
    - 1. CPU pipeline에 stall(bubble)이 발생한다.
    - 2. Lower-level로 부터 필요한 Data를 복사해와야 한다. (main memory)
    - 3. Stall이 발생한 Task를 실행한다.
      - IF stage stall : Instruction Fetch 재시작
      - MEM stage stall : Data Access 마무리
    


   

* Post1
* Post2
* Post3

