# IPC 시스템의 예시

## POSIX 공유 메모리

- <u>유닉스(UNIX)</u>에서 휴대 가능한 운영체제 인터페이스(Portable Operating System Interface)

- 공유 메모리 영역을 파일과 연결하는 메모리 매핑(mapping) 파일을 사용하여 구성됨

  ```c
  // 1. 공유 메모리 생성
  fd = shm_open(name, O_CREAT | OREWR, 0666);
  // 2. fd의 파일 크기 정하기
  ftruncate(fd, 4096);
  // 3. 메모미 매핑(memory-mapped) 파일 만들기
  mmap(0,SIZE, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
  ```

  

## Pipes

- 초기 UNIX 시스템에서 IPC 메커니즘 중에 하나

- 두 개의 프로세스가 통신할 수 있도록 하는 도구 역할

- **pipe 실행에서 고민해야하는 것**

  - 단방향(unidirectional) or 양방향(bidirectional)
  - 양방향 통신의 경우, 반이중(half-duplex) or 전이중(full-duplex)
  - 통신하는 프로세스 사이에 관계가 존재(ex. 부모 자식 관계)
  - 파이프가 네트워크를 통해서 소통하는지 않하는지

- 종류

  - Ordinary pipes

    - 생산자 - 소비자처럼 두 프로세스가 소통함

    - 생산자는  파이프에 쓰고 소비자는 읽음

    - 단방향으로만 소통 가능

    - 양방향 소통 :arrow_right: 2개의 파이프를 이용

      <img src = "./img/img7.png" width = "80%">

      ```c
      // 파이프 생성
      pipe(int fd[]);
      fd[0]; // 읽기
      fd[1];// 쓰기
      ```

     

  - Named pipe : 부모 자식 관계가 없이도 접근 가능