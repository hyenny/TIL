# CPU 스케줄러(CPU Scheduler)
CPU 스케줄러는 **프로세스가 생성된 후 종료될 때까지 모든 상태 변화를 조정하는 일**을 하며, CPU 스케줄링은 CPU 스케줄러가 하는 모든 작업을 가리킨다. 

> 참고 : CPU 스케줄러는 운영체제 안에 스케줄링 하는 코드를 의미하며, 하드웨어가 아니다.

## Long-term scheduler
- `장기(Long-term) 스케줄러` or `작업(Job) 스케줄러` or `고수준(High-level) 스케줄러`
- **시스템 내의 전체 프로세스 수를 조절하는 스케줄러**
    - 시작 프로세스 중 어떤 것들을 준비 큐(ready queue)에 보낼지 결정
    - 프로세스에 메모리(및 각종 자원)를 주는 문제
    - degree of multiprogramming을 제어
    - time sharing system에는 보통 장기 스케줄러가 없다. 메인 프레임같은 큰 시스템에서 규모가 큰 일괄 작업을 처리할 때 사용(오늘날 잘 사용하지 않음)

## Short-term scheduler
- `단기(Short-term) 스케줄러` or `CPU 스케줄러` or `저수준(Low-level) 스케줄러`
    - 일반적으로 CPU 스케줄러라고 하면 단기 스케줄러를 의미한다.
- **어떤 프로세스에 CPU를 할당할지, 어떤 프로세스를 대기 상태로 보낼지 등을 결정하는 스케줄러**
    - 어떤 프로세스를 다음번에 실행(running)시킬지 결정
    - 프로세스에 CPU를 주는 문제
    - 스케줄링이 아주 짧은 시간에 일어난다.(millisecond 단위)

## Medium-term scheduler
- `중기(Medium-term) 스케줄러` or `swapper` or `중간 수준(Middle-level) 스케줄러`
- **전체 시스템의 활성화된 프로세스 수를 조절하여 과부하를 막는 스케줄러**
    - 중지(suspend)와 활성화(active)로 전체 시스템의 활성화된 프로세스 수를 조절한다. => 일부 프로세스를 중지 상태로 옮김으로써 나머지 프로세스가 원만하게 작동하도록 지원함.
    - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크(swap area)로 쫓아낸다.
    - 프로세스에게서 memory를 뺏는 문제
    - 단기 스케줄링이 원만하게 이루어지도록 완충하는 역할
 
> swap area : 메모리에서 쫓겨난 데이터가 임시로 보관되는 곳

![스케줄링 단계](./images/scheduling-step.PNG)