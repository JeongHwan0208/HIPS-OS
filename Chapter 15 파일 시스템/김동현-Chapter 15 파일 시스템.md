# 파일 시스템

# 파일

**파일** : 하드 디스크나 SSD와 같은보조기억장치에 저장된 관련 정보의 집합. 의미 있고 관련 있는 정보를 모은 논리적 단위. 

**파일을 이루는 정보**

- 이름
- 파일을 실행하기 위한 정보
- 파일 관련 부가 정보 → 속성 / 메타데이터라고 함.
    
    파일 형식, 위치, 크기 등 관련된 다양한 정보
    

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-1.png)

## 파일 속성과 유형

**대표적인 속성의 종류**

- **유형** : 운영체제가 인지하는 파일의 종류를 나타낸다.
- **크기** : 파일의 현재 크기와 허용 가능한 최대 크기를 나타낸다.
- **보호** : 어떤 사용자가 해당 파일을 읽고, 쓰고, 실행할 수 있는지를 나타낸다.
- **생성 날짜** : 파일이 생성된 날짜를 나타낸다.
- **마지막 접근 날짜** : 파일이 마지막으로 수정된 날짜를 나타낸다.
- **생성자** : 파일을 생성한 사용자를 나타낸다.
- **소유자** : 파일을 소유한 사용자를 나타낸다.
- **위치** : 파일의 보조기억장치상의 현재 위치를 나타낸다.

**파일 유형** : 운영체제가 인식하는 파일 종류

같은 이름의 파일일지라도 유형이 다르면 실행 양상도 달라진다.

**확장자(extension)** : 파일 유형을 알리기 위해 가장 흔히 사용하는 방식. 파일 이름 뒤에 붙는다.

- **실행 파일** : 없는 경우, exe, com, bin
- **목적 파일** : obj, o
- **소스 코드 파일** : c, cpp, cc, java, asm, py
- **워드 프로세서 파일** : xml, rtf, doc, docx
- **라이브러리 파일** : lib, a, so, dll
- **멀티미디어 파일** : mpeg, mov, mp3, mp4, avi
- **백업/보관 파일** : rar, zip, tar

## 파일 연산을 위한 시스템 호출

파일을 다루는 모든 작업은 운영체제에 의해 이뤄진다.

어떤 응용 프로그램도 임의로 파일을 조작할 수 없고, 파일을 다루려면 운영체제를 통해야 한다.

**운영체제가 제공하는 파일 연산을 위한 시스템 호출**

1. 파일 생성
2. 파일 삭제
3. 파일 열기
4. 파일 닫기
5. 파일 읽기
6. 파일 쓰기

# 디렉터리

**디렉터리** : 파일들을 일목요연하게 관리하기 위해 사용하는 방식, 윈도우 운영체제에서는 폴더(folder)라고 부름.

**1단계 디렉터리(single-level directory)** : 모든 파일이 하나의 디렉터리 아래에 있는 구조

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-2.png)
컴퓨터 용량이 커지면서 저장할 수 있는 파일도 많아지고, 1단계 디렉터리로는 많은 파일을 관리하기가 어렵다.

**트리 구조 디렉터리(tree-structured directory)** : 최상위 디렉터리가 있고 그 아래에 여러 서브 디렉터리(자식 디렉터리)가 있는 구조. 

최상위 디렉터리는 흔히 **루트 디렉터리**라고 부르고 슬래시(/)로 표현.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-3.png)

**경로** : 디렉터리를 이용해 파일 위치, 나아가 파일 이름을 특정 짓는 정보

## 절대 경로와 상대 경로

같은 디렉터리에는 동일한 이름의 파일이 존재할 수 없지만, 서로 다른 디렉터리에는 루트 디렉터리부터 파일까지 경로가 다르기 때문에 동일한 이름의 파일이 존재할 수 있다.

**절대 경로** : 루트 디렉터리에서 자기 자신까지 이르는 고유한 경로

ex) /home/minchul/a.sh

**상대 경로** : 현재 디렉터리부터 시작하는 경로

ex) guest/d.jpg

**상대 경로를 나타내는 또 다른 방법**

현재 작업 디렉터리는 마침표( **.** ), 현재 작업 디렉터리의 상위 디렉터리(부모 디렉터리)를 마침표 두 번( **..** )으로 나타낸다.

guest/d.jpg → ./guest/d.jpg

## 디렉터리 연산을 위한 시스템 호출

**운영체제가 제공하는 디렉터리 연산을 위한 시스템 호출**

1. 디렉터리 생성
2. 디렉터리 삭제
3. 디렉터리 열기
4. 디렉터리 닫기
5. 디렉터리 읽기

## 디렉터리 엔트리

디렉터리는 조금 특별한 정보를 포함하는 **특별한 형태의 파일**이다.

파일이 내부에 해당 파일과 관련된 정보를 담고 있다면, 디렉터리는 내부에 해당 디렉터리에 담겨 있는 대상과 관련된 정보를 담고 있다. 이 정보(디렉터리)는 보조기억장치에 테이블(표) 형태의 정보로 저장된다.

각각의 엔트리(행)에 담기는 정보는 파일 시스템마다 차이가 있다.

디렉터리 엔트리가 공통으로 포함하는 정보가 있다면 그것은 디렉터리에 포함된 대상의 이름과 그 대상이 보조기억장치 내에 저장된 위치를 유추할 수 있는 정보가 담긴다.

디렉터리 엔트리만 보아도 해당 디렉터리에 무엇이 담겨 있는지, 그리고 그것들은 보조기억장치의 어디에 있는지를 직간접적으로 알 수 있다. 

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-4.png)

ex) home 디렉터리 → guest 디렉터리

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-5.png)

디렉터리 엔트리를 통해 보조기억장치에 저장된 위치를 알 수 있기 때문에 home디렉터리에서 guest 디렉터리가 저장된 곳을 알 수 있고 이동할 수도 있다. 

guest 디렉터리 엔트리에는 디렉터리에 속한 파일들의 이름과 이들의 위치를 알 수 있는 정보 등이 포함되어 있기 때문에 이 파일들이 보조기억장치 내에 저장된 위치를 알 수 있고 실행할 수 있다.

---

# 파티셔닝과 포매팅

새 하드 디스크 또는 SSD는 파티션을 나누는 작업**(파티셔닝)**과 포맷 작업**(포맷팅)**을 거쳐야 하기 때문에 곧바로 파일을 생성하거나 저장할 수 없다.

**파티셔닝(partitioning)** : 저장 장치의 논리적인 영역을 구획하는 작업. 칸막이로 영역을 나누는 작업.

**파티션** : 파티셔닝 작업을 통해 나누어진 영역 하나하나

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-6.png)

**포매팅(formatting)** : 파일 시스템을 설정하여 어떤 방식으로 파일을 저장하고 관리할 것인지를 결정하고, 새로운 데이터를 쓸 준비를 하는 작업. 

이때 어떤 종류의 파일 시스템을 사용할지 결정된다.

**저수준 포매팅** : 저장 장치를 생성할 당시 공장에서 수행되는 물리적인 포매팅.

**논리적 포매팅** : 파일 시스템을 생성하는 포매팅.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-7.png)

파일 시스템에는 여러 종류가 있고, 파티션마다 다른 파일 시스템을 설정할 수도 있다.

# 파일 할당 방법

운영체제는 파일과 디렉터리를 블록(block)단위로 읽고 쓴다. 하나의 파일이 보조기억장치에 저장될 때는 하나 이상의 블록에 걸쳐 저장된다.

하드 디스크의 가장 작은 저장 단위는 섹터이지만, 운영체제는 하나 이상의 섹터를 블록이라는 단위로 묶은 뒤 파일과 디렉터리를 관리한다.

파일의 크기에 따라 사용되는 블록에 개수가 다르다.

**파일을 보조기억장치에 할당하는 방법**

1. 연속 할당
2. 불연속 할당
    1. 연결 할당
    2. 색인 할당

## 연속 할당

**연속 할당(contiguous allocation)** : 보조기억장치 내 연속적인 블록에 파일을 할당하는 방식.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-8.png)

연속으로 할당된 파일에 접근하기 위해서는 파일의 첫 번째 블록 주소와 블록 단위의 길이만 알면 된다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-9.png)

연속 할당을 사용하는 파일 시스템에서는 디렉터리 엔트리에 파일 이름, 첫 번째 블록 주소, 블록 단위의 길이를 명시한다.

**장점** : 파일을 그저 연속적으로 저장하는 방식이기에 구현이 단순하다.

**단점** : **외부 단편화**를 야기한다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-10.png)

파일 A, 파일 C 삭제

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-11.png)

할당할 수 있는 블록은 총 10개지만, 크기가 블록 7개 이상을 사용하는 파일은 할당할 수 없다.

## 연결 할당

**연결 할당(linked allocation)** : 각 블록 일부에 다음 블록의 주소를 저장하여 각 블록이 다음 블록을 가리키는 형태로 할당하는 방식.

파일을 이루는 데이터를 연결 리스트로 관리한다. 

불연속 할당의 일종이기에 파일이 여러 블록에 흩어져 저장되어도 무방하다.

마지막 블록에는 다음 블록이 없다는 특별한 표시자를 사용한다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-12.png)

연결 할당을 사용하는 파일 시스템은 디렉터리 엔트리에 연속 할당과 마찬가지로 파일 이름, 첫 번째 블록 주소, 블록 단위의 길이를 명시한다.

**연결 할당의 단점** 

1. **반드시 첫 번째 블록부터 하나씩 차례대로 읽어야 한다.**
    
    파일의 중간 부분부터 접근하고 싶어도 파일의 첫 번째 블록부터 접근해 하나씩 읽어야 한다
    
    → 파일 내 임의의 위치에 접근하는 속도, 즉 임의 접근(random access)속도가 매우 느리다.
    
    성능면에서 상당히 비효율적이다.
    
2. **하드웨어 고장이나 오류 발생 시 해당 블록 이후 블록은 접근할 수 없다.**
    
    하나의 블록 안에 파일 데이터와 다음 블록 주소가 모두 포함되어 있다 보니, 하드웨어 고장이나 오류로 인해 파일을 이루는 블록에 하나라도 문제가 발생하면 그 블록 이후의 블록에 접근할 수 없다.
    
    하드 디스크 헤드는 플래터 위에 대단히 미세한 간격으로 떨어져 있는 만큼 충격을 받으면 헤드가 플래터에 충돌하여 데이터를 손상시킬 수 있다.
    

이러한 단점으로 인해 연결 할당을 조금 변형한 대표적인 파일 시스템인 FAT 파일 시스템을 많이 사용한다.

## 색인 할당

**색인 할당(indexed allocation)** : 파일의 모든 블록 주소를 색인 블록(index block)이라는 하나의 블록에 모아 관리하는 방식.

연결 할당과는 달리 파일 내 임의의 위치에 접근하기 쉽다.

색인 블록 안에 파일을 구성하는 데이터 블록 주소가 있으므로 색인 블록만 알면 해당 파일 데이터에 접근할 수 있다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-13.png)

색인 할당을 사용하는 파일 시스템은 디렉터리 엔트리에 파일 이름, 색인 블록 주소를 명시한다.

# 파일 시스템 살펴보기

1. FAT 파일 시스템 (저용량 저장 장치에서 사용)
2. 유닉스 파일 시스템 (유닉스 계열 운영체제에서 사용) 

## FAT 파일 시스템

**FAT 파일 시스템** : 연결 할당의 단점을 보완한 파일 시스템

연결 할당의 근본적인 원인은 블록 안에 다음 블록의 주소를 저장한 것이다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-14.png)

**파일 할당 테이블(FAT; File Allocation Table)** : 각 블록에 포함된 다음 블록의 주소들을 한데 모아 놓은 테이블 형태

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-15.png)

파일의 첫 번째 블록 주소만 알면 파일의 데이터가 담긴 모든 블록에 접근할 수 있다.

![FAT 영역에는 FAT가 저장되고, 루트 디렉터리가 저장되는 영역이 있고, 서브 디렉터리와 파일들을 위한 영역이 존재한다.](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-16.png)

FAT 영역에는 FAT가 저장되고, 루트 디렉터리가 저장되는 영역이 있고, 서브 디렉터리와 파일들을 위한 영역이 존재한다.

FAT는 하드 디스크 파티션의 시작 부분에 있지만, 실행하는 도중 FAT가 메모리에 캐시될 수 있다.

FAT가 메모리에 적재된 채 실행되면 기존 연결 할당보다 다음 블록을 찾는 속도가 매우 빨라지므로, 임의 접근의 성능이 개선된다.

FAT 파일 시스템의 디렉터리 엔트리에는 파일 이름과 첫 번째 블록 주소가 명시되고, 이외에도 파일 속성과 관련한 다양한 정보들이 있다.

![FAT 파일 시스템에서 디렉터리들은 이런 형식으로 블록에 저장된다.](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-17.png)

FAT 파일 시스템에서 디렉터리들은 이런 형식으로 블록에 저장된다.

**FAT파일 시스템에서 파일을 읽는 과정**

ex) /home/minchul/a.sh에 접근

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-18.png)

1. 루트 디렉터리에서 home 디렉터리가 몇 번 블록에 있는지 살펴본다. 3번 블록
2. 3번 블록에 home 디렉터리에서 minchul 디렉터리가 몇 번 블록에 있는지 살펴본다. 15번 블록
3. 15번 블록에 minchul 디렉터리에서 [a.sh](http://a.sh) 파일의 첫 번째 블록 주소가 9번인 것을 알 수 있다.
4. FAT를 보면 [a.sh](http://a.sh) 파일이 9번 → 8번 → 11번 → 13번 블록 순서로 저장되어 있는 것을 알 수 있다. 따라서 파일 시스템은 9번, 8번, 11번, 13번 블록에 접근한다.

## 유닉스 파일 시스템

**유닉스 파일 시스템** : 색인 할당 기반의 파일 시스템

**i-node(index-node)** : 색인 할당 방식에 사용하는 색인 블록

- 파일 속성 정보와 열다섯 개의 블록 주소가 저장될 수 있다.
- 유닉스 파일 시스템에서는 파일 속성 정보는 i-node에 표현된다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-19.png)

파일마다 i-node가 있고, i-node마다 번호가 부여되어 있다.

i-node들은 파티션 내 i-node영역에 들어있고, 데이터 영역에는 디렉터리와 파일들이 있다.

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-20.png)

**i-node의 크기 제한을 해결하는 방법**

1. **블록 주소 중 열두 개에는 직접 블록 주소를 저장한다.**
    
    **직접 블록(direct block)** : 파일 데이터가 저장된 블록
    
    ![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-21.png)
    
2. **1번으로 충분하지 않다면 열세 번째 주소에 단일 간접 블록 주소를 저장한다.**
    
    **단일 간접 블록(single indirect block)** : 파일 데이터가 저장된 블록이 아닌 파일 데이터를 저장한 블록 주소가 저장된 블록
    
    ![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-22.png)
    
3. **2번으로 충분하지 않다면 열네 번째 주소에 이중 간접 블록 주소를 저장한다.**
    
    **이중 간접 블록(double indirect block)** : 데이터 블록 주소를 저장하는 블록 주소가 저장된 블록
    
    ![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-23.png)
    
4. **3번으로 충분하지 않다면 열다섯 번째 주소에 삼중 간접 블록 주소를 저장한다.**
    
    **삼중 간접 블록(triple indirect block)** : 이중 간섭 블록 주소가 저장된 블록
    
    웬만한 크기의 파일의 표현이 가능하다.
    
    ![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-24.png)
    

**유닉스 파일 시스템에서 파일을 읽는 과정**

ex) /home/minchul/a.sh에 접근

![Untitled](https://github.com/DongHyeon1004/Computer-Architecture-Operating-System/blob/main/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B5%AC%EC%A1%B0%2B%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/image/15-25.png)
파일 시스템은 우선 루트 디렉터리 위치부터 찾는다. 루트 디렉터리 위치는 루트 디렉터리의 i-node 보면 알 수 있고, 유닉스 파일 시스템은 루트 디렉터리의 i-node번호를 항상 기억한다.

1. 2번 i-node에 접근해 루트 디렉터리의 위치를 파악한다. 1번 블록
2. 1번 블록에 루트 디렉터리에서 home 디렉터리의 i-node 번호를 파악한다. 3번 i-node
3. 3번 i-node에 접근해 home 디렉터리가 몇 번 블록에 있는지 파악한다. 210번 블록
4. 210번 블록에 home 디렉터리에서 minchul 디렉터리의 i-node 번호를 파악한다. 8번 i-node
5. 8번 i-node에 접근해 minchul 디렉터리가 몇 번 블록에 있는지 파악한다. 121번 블록
6. 121번 블록에 minchul 디렉터리에서 파일 a.sh의 i-node 번호를 파악한다. 9번 i-node
7. 9번 i-node에 접근해 [a.sh](http://a.sh) 파일이 몇 번 블록에 있는지 파악한다. 98번, 12번, 13번 블록
    
    따라서 파일 시스템은 98번, 12번, 13번 블록에 접근한다.
