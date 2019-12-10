## [컴퓨터구조] Cache Overview

### 목표
  1. Cache 등장 배경 이해
  2. 계층화 메모리 이해
  3. Cache Miss와 Write를 Handling하는 방법 이해
  4. Cache 성능 측정 방법 
  
  
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
      1. CPU pipeline에 stall(bubble)이 발생한다.
      2. Lower-level로 부터 필요한 Data를 복사해와야 한다. (main memory)
      3. Stall이 발생한 Task를 실행한다.
      
      
        - IF stage stall : Instruction Fetch 재시작
        - MEM stage stall : Data Access 마무리
        
### Handling Writes
  - Processor로 부터 SW (store)라는 Memory Write 명령어가 들어온다면 어떻게 해야할까?
  - Write-through : Cache와 Main Memory (lower-level memory)에 동시에 새로운 데이터를 Update한다.
    - 장점 : Cache와 Main Memory 사이의 Consistency가 보장된다. | 쉽게 실행할 수 있다.
    - 단점 : Write-back에 비해 느리다.
  - Write-back : Just Update Cache || Dirty Bit = 1인 Cache에 Write operation이 발생할 때, Main Memory에 복사한다.
    - 장점 : Write-through에 비해 빠르다. 
    - 단점 : Cache와 Main Memory 사이의 Consistency가 보장되지 않는다. | 실행하기 복잡하다.
    
  - Cache는 Write-through의 느린 속도를 보완하기 위해 Write Buffer를 제공한다.
    - 이 Write buffer는 Main memory보다 접근 속도가 빠르다.
    - Main memory에 write해야 하는 data를 write buffer에 먼저 적는다.
    - Processor가 프로그램을 대기없이 계속 실행한다면, write buffer의 data가 main memory로 이동한다.
    - 만약, write buffer가 꽉 찬다면?
      - Write Buffer에 자리가 생길때까지 Stall이 발생한다.
      
  - Write Miss를 해결하는 방법
    1. Write-allocate
      - Fetch the block to cache
      - Handle Write operation
    2. Write-around
      - Cacha에 Write하지 않고, Main memory의 블록 일부분만을 Update한다.
      - 메모리 공간 초기화가 필요할 때 좋다.
      
 ### Cache 성능 측정 방법
  - CPU time = clock cycles * clock periods = (CPU execution + Memory stall) clock cycles * clock periods
  - Memory stall clock cycles = # of cache misses * miss penalty 
  - (# of cache misses) = (# of memory accesses) * miss rate
  - 실생활에서 우리는 Instruction용 I-cache와 Data용 D-cache를 사용한다. 따라서 식은 다음과 같이 바뀐다.
  - (# of Instruction memory access) * I-cache miss rate * I-cache miss penalty + 
    
    (# of Data memory access) * D-cache miss rate * D-cache miss penalty
  
  
  

