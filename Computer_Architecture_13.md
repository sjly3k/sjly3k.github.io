## [컴퓨터구조] Virtual Memory


### 목표
  1. Cache 등장 배경 이해
  2. 계층화 메모리 이해
  3. Cache Miss와 Write를 Handling하는 방법 이해
  4. Cache 성능 측정 방법 
  
  
### Virtual Memory의 등장 배경
  * 왜 Virtual Memory가 등장했는가
    - Processor가 원하는 수많은 데이터를 Main Memory에만 저장하기엔 MM의 용량이 부족함.
    - 이로 인해 MM이 Disk의 Cache처럼 이용됨.
    - 따라서 MM의 용량 문제를 해결하기 위한 방법으로 Virtual Memory가 등장함.
    
  * Multi-Process 환경에서 여러개의 프로세스들은 MM을 동시간에 공유한다. 그리하여 다음과 같은 것을이 동적으로 바뀐다.
    - 각각의 프로세스가 사용할 수 있는 메모리의 총 용량
    - 프로세스에서 사용되는 메모리 주소
    - 데이터의 물리적 위치
    - 2가지 의문점
    
    
      - 이런 변화를 누가 관리할 것인가?
      - 안전성은 어떻게 확보할 것인가?
  
  * Virtual Memory란?
    - 각각의 프로세스에 거대한 메모리 분신을 제공한다.
    - CPU와 OS가 VM, MM, Disk 사이의 mapping을 관리한다.
    - 장점 
      - 다른 프로세스가 메모리를 사용하여 생기는 영향을 생각하지 않아도 된다. (각 프로세스 별 메모리 공간이 고정되므로)
      - 다른 프로세스들은 다른 물리적 주소를 가지므로 다른 데이터에 침범할 수 없다.
    - 용어
      - Page : VM 속 정보의 최소 단위
      - Page Fault : 요청된 page가 MM에 존재하지 않을 경우        
        
### Page Fault가 발생하면?
  - (1) Fully Associative placement
    - VM에서 모든 페이지들은 물리 주소로 MM과 연관되거나 디스크 주소로 Disk와 연관되어 있다. 
    - Page Table 사용 : Valid Bit | Physical Page Number
       - 물리 주소와 Valid Bit가 있음.
        - VB == 0이면, PTE(Page Table Entry)는 Disk와의 mapping 정보를 가지고 있음.
        - VB == 1이면, PTE는 PPN을 가지고 있음.
       - VPN(Virtual Page Number)로 인덱스되어 있다.
    - BUT! MM으로 가는 과정이 매우 복잡함. 따라서 Performance Issue가 발생함.
    
    즉, 메모리 요청이 들어오면, Page Table(메모리에 존재)에 접근하기 위해 메모리를 한번 더 접근 해야함.
    
    - 위와 같은 이유로, TLB (Translation Lookahead Buffer)가 등장 : Valid Bit | Dirty Bit | Tag | PPN
      - Page Table의 Cache와 같은 역할
      - 최근에 사용됬거나, 사용될 것같은 PTE들이 TLB에 저장 (Locality 확보)
      
   - (2) Smart replacement Algorithms
     - LRU (Least Recently Used) : reference bit 이용 
        - Reference bit == 1, 참조되었다는 의미. 즉 사용된적이 있다는 의미이다.
        - Reference bit == 0, 참조되지 않았다는 의미. 즉 사용되지 않은 기간이 오래되었다는 뜻이므로 여기에 다른 Data가 들어옴.
     - Random 
    
### Integrating Cache, TLB, Memory
  - Physically addressed Cache
    - 장점 : Easy to support Multi-processing environment
    - 단점 : TLB is in a critical path (TLB를 무조건 거쳐야 함.)
  - Virtually addressed Cache
    - 장점 : TLB is not in a critical path (TLB를 거치지 않아도 됨.)
    - 단점 : Difficult to support Multi-processing environment due to Aliasing
    * Why Aliasing?
      - 다른 Process에서 다른 VM이 같은 PA와 mapping
        - PM이 서로 다른 두 위치에 Cached 될 수 있다.
        - Coherency 문제 발생
      - 다른 Process에서 같은 VM이 다른 PA와 mapping
        - Wrong cache hit이 발생할 수 있음.
      - 해결책 : Flush cache at context switching -> too high miss rate
      * Context switching : 모든 switch마다 CPU는 victim process의 상태를 저장함.
  - Hybrid Cache : VAC의 문제점 (Aliasing 해결)
  - Typical multi-level cache setup with TLB
    - Typical L1 : hybrid cache
    - Typical L2 : Physically Addressed cache

### Handling TLB misses
  - TLB miss가 발생하면, page table로 가서 찾는다.
  

  
  

