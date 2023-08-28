# Chapter 13 교착 상태 (Deadlock)

## 13-1 교착 상태란

**교착 상태(Deadlock)** : 시스템 자에 대한 요구가 뒤엉킨 상태 즉, **둘 이상의 프로세스가 다른 프로세스가 점유하고 있는 자원을 서로 기다릴 때 무한 대기에 빠지는 상황**을 말하는 것이다.

### 식사하는 철학자 문제

**식사하는 철학자 문제**는 교착 상태를 설명하기 위한 고전적이고 재미있는 문제 상황입니다.

동그란 원탁에 다섯 명의 철학자가 앉아 있다. 이 철학자들 앞에 있는 맛있는 식사가 있고, 철학자들 사이 사이에는 식사에 필요한 포크가 있다. 그리고 철학자들 앞에 있는 식사는 두 개의 포크로 먹을 수 있는 음식이라 가정하자

![1](C:\Users\pc\Desktop\발표\이미지\1.jpg)

철학자들은 아래와 같은 순서로 식사를 한다.

1. 계속 생각을 하다가 왼쪽 포크가 사용 가능하면 집어든다.
2. 계속 생각을 하다가 오른쪽 포크가 사용 가능하면 집어든다.
3. 왼쪽과 오른쪽 포크를 모두 집어들면 정해진 시간동안 식사를 한다.
4. 식사 시간이 끝나면 오른쪽 포크를 내려놓는다.
5. 오른쪽 포크를 내려놓은 뒤 왼쪽 포크를 내려 놓는다.
6. 다시 1번부터 반복한다.

이 결과로는 모든 철학자가 동시에 포크를 집어 식사를 하면 어떤 철학자도 식사를 할 수 없고 영원히 생각만 하는 상황이 발생할 수 있다. 모든 철학자가 왼쪽 포크를 집어들면 모두가 오른쪽 포크를 집어들 수 없기 때문이다. 다시 말해 모든 철학자는 다른 철학자가 포크를 내려 놓을 때까지 기다린다.

식사하는 철학자 문제에서 철학자는 프로세스 혹은 스레드, 포크는 자원, 생각하는 행위는 자원을 기다리는 것에 빗대어 볼 수 있다. 그리고 포크는 한 번에 하나의 프로세스 혹은 스레드만 접근할 수 있으니 임계구역이라고 볼 수 있다.

다른 예로는

```C
pthread_mutex_t first_mutex;				// pthread_mutex_t 타입으로 first_mutex 선언
pthread_mutex_t second_mutex;				// pthread_mutex_t 타입으로 second_mutex  선언

pthread_mutex_init(&first_mutex,NULL);     
pthread_mutex_init(&second_mutex,NULL);

/*thread_one runs in this function*/
void *do_work_one(void *param)                                         
{
    pthread_mutex_lock(&first_mutex);		// first_mutex 획득
    pthread_mutex_lock(&second_mutex);		// second_mutex 획득
    /**
     * Do some work
     */
    pthread_mutex_unlock(&second_mutex);	// second_mutex 반납
    pthread_mutex_unlock(&first_mutex);		// first_mutex 반납
    
    pthread_exit(0);
    
}
/*thread_two runs in this function*/
void *do_work_two(void *param)
{
    pthread_mutex_lock(&first_mutex);		// second_mutex 획득
    pthread_mutex_lock(&second_mutex);		// first_mutex 획득
    /**
     * Do some work
     */
    pthread_mutex_unlock(&second_mutex);	// first_mutex 반납
    pthread_mutex_unlock(&first_mutex);		// second_mutex 반납
    
    pthread_exit(0);
    
}
```

첫 번째 스레드가 first_mutex를 점유하고 second_mutex를 점유하려고 할때 두 번째 스레드가 second_mutex를 점유했다고 가정하면,
첫 번째 스레드는 second_mutex를 점유하기 위해 대기하고 있고 두 번째 스레드는 first_mutex를 점유하기 위해 대기하고 있게 된다. 여기서 첫 번째 스레드는 second_mutex를 점유해야 일을 할 수 있고 두 번째 스레드는 first_mutex를 점유해야 일을 할 수 있다. 하지만 일을 끝내지 못하면 각각에 필요한 mutex는 반환되지 않기 때문에 무한 대기에 빠지게 된다.그러므로 데드락이 발생하기 쉬운 코드이다.

### 교착 상태 발생 조건

- **상호 배제** : 한 프로세스가 사용하는 자원을 다른 프로세스가 사용할 수 없을 때, 즉 **상호 배제**상황에서 교착 상태가 발생할 수 있다.
- **점유와 대기** : 프로세스가 어떠한 자원을 할당받은 상태에서 다른 자원을 할당받기를 기다린다면 교착 상태가 발생할 수 있다. 이렇게 '자원을 할당받은 상태에서 다른 자원을 할당받기를 기다리는 상태'를 **점유와 대기**라고 한다.
- **비선점** : **비선점 자원**은 그 자원을 이용하는 프로세스의 작업이 끝나야만 비로소 이용할 수 있다. 즉, 어떤 프로세스도 다른 프로세스의 자원을 강제로 빼앗지 못했기 때문에 교착 상태가 발생했다고 볼 수 있다.
- **원형 대기** : 프로세스들과 프로세스가 요청 및 할당받은 자원이 원의 형태를 이루었기 때문이다. 이렇게 프로세스들이 원의 형태로 대기하는 것을 **원형 대기**라 한다.

### 자원 할당 그래프

교착 상태는 **자원 할당 그래프**를 통해 단순하게 표현할 수 있다. 자원 할당 그래프는 어떤 프로세스가 어떤 자원을 사용하고 있고, 또 어떤 자원을 기다리고 있는지를 표현하는 간단한 그래프이다.

**자원 할당 그래프 규칙**

1. **프로세스는 원으로, 자원의 종류는 사각형으로 표현한다.**

![2](C:\Users\pc\Desktop\발표\이미지\2.jpg)

2. **사용할 수 있는 자원의 개수는 자원 사각형 내에 점으로 표현한다.**

  같은 자원이라 할지라도 사용 가능한 자원의 개수는 여러 개 있을 수 있습니다.

![3](C:\Users\pc\Desktop\발표\이미지\3.jpg)

3. **프로세스가 어떤 자원을 할당받아 사용 중이라면 자원에서 프로세스를 향해 화살표를 표시합니다.**

​                                                     ![4](C:\Users\pc\Desktop\발표\이미지\4.jpg)

4. **프로세스가 어떤 자원을 기다리고 있다면 프로세스에서 자원으로 화살표를 표시합니다.**

![5](C:\Users\pc\Desktop\발표\이미지\5.jpg)

예제 1)

![6](C:\Users\pc\Desktop\발표\이미지\6.jpg)

예제 2)

![7](C:\Users\pc\Desktop\발표\이미지\7.jpg)

이로써 알 수 있는 중요한 점은

- **만약 자원 할당 그래프에 싸이클이 없다면 교착 상태(Deadlock)은 존재하지 않는다.**
- **만약 자원 할당 그래프에 싸이클이 있다면 교착 상태(Deadlock)은 존재할 수도 있고 존재하지 않을 수도 있다.**

## 13-2 교착 상태 해결 방법

1. **예방**
2. **회피**
3. **검출 후 회복**

### 교착 상태 예방

- **우선 자원의 상호 배제 제거** : 자원의 상호 배제를 없엔다는 말의 의미는 모든 자원을 공유 가능하게 만든다는 말과 같다. 다만 이 방식대로면 이론적으로는 교착 상태를 없앨 수 있지만, 현실적으로 모든 자원의 상호 배제를 없에기는 어렵기에 이 방식을 현실에서 사용하기에는 다소 무리가 있다.
- **점유와 대기 제거** : 점유와 대기를 없애면 운영체제는 특정 프로세스에 자원을 모두 할당하거나, 아예 할당하지 않는 방식으로 배분한다. 이 방식도 이론적으로는 교착 상태를 해결할 수 있지만, 단점도 있다. 우선 자원의 활용률이 낮아질 우려가 있다. 당장 자원이 필요해도 기다릴 수밖에 없는 프로세스와 사용되지 않으면서 오랫동안 할당되는 자원을 다수 양산하기 때문에 자원의 활용률이 낮아진다.
- **비선점 조건 제거** : 비선점 조건을 없애면 자원을 이용 중인 프로세스로부터 해당 자원을 빼앗을 수 있습니다. 이 방식은 선점하여 사용할 수 있는 일부 자원에 대해서는 효과적이다. 하지만 범용성이 떨어진다.
- **원형 대기 조건 제거** : 원형 대기를 없애는 방법은 모든 자원에 번호를 붙이고, 오름차순으로 자원을 할당하면 원형 대기는 발생하지 않는다. 원형 대기를 없앰으로써 교착 상태를 예방하는 방식은 앞선 세 방식에 비하면 비교적 현실적이고 실용적인 방식이지만, 역시 단점은 있다. 모든 컴퓨터 시스템 내에 존재하는 수많은 자원에 번호를 붙이는 일은 간단한 작업이 아니거니와 각 자원에 어떤 번호를 붙이는지에 따라 특정 자원의 활용률이 떨어질 수 있다.

**결과적으로 교착 상태의 발생 조건을 원천적으로 제거하여 교착 상태를 사전에 방지하는 예방 방식은 교착 상태가 발생하지 않을을 보장할 수는 있지만 여러 부작용이 따른다.**

### 교착 상태 회피

**교착 상태 회피**는 교착 상태가 발생하지 않을 정도로만 자원을 할당하는 방식이다. 교착 상태를 한정된 자원의 무분별한 할당으로 인해 발생하는 문제로 간주한다.

프로세스들에 할당할 수 있는 자원이 충분한 상황에서 프로세스들이 한두 개의 적은 자원만을 요구한다면 교착 상태는 발생하지 않는다. 반면 프로세스들에 할당할 수 있는 자원의 한정된 상황에서 모든 프로세스들이 한 번에 많은 자원을 요구하면 교착상태가 발생할 위험이 증가한다.

그렇기 때문에 프로세스들에 배분할 수 있는 자원의 양을 고려하여 교착 상태가 발생하지 않을 정도의 양만큼만 자원을 배분하는 방법이 **교착 상태 회피**이다.

교착 상태를 회피하는 방법을 학습하기 위해서는 안전 상태와 불안전 상태, 그리고 안전 순서열이라는 용어를 알아야 합니다.

- **안전 상태** : 교착 상태가 발생하지 않고 모든 프로세스가 정상적으로 자원을 할당받고 종료될 수 있는 상태
- **불안전 상태** : 교착 상태가 발생할 수도 있는 상황
- **안전 순서열** : 교착 상태 없이 안전하게 프로세스들에 자원을 할당할 수 있는 순서를 의미한다.
- **안전 상태** : 안전 순서열대로 프로세스들에 자원을 배분하여 교착 상태가 발생하지 않는 상태
- **불안정 상태** : 안전 순서열이 없는 상태

결과적으로

![8](C:\Users\pc\Desktop\발표\이미지\8.jpg)

### **Avoidance Alogrithm**

1. **Single-Instance (Resource Allocation Graph Algorithm)**

- 자원 할당 그래프를 사용해 Process의 수행 순서를 결정한다.
- 미래에 사용할 자원을 나타내는 Claim Edge를 사용한다. (점선)

​       -> 요청할 때 점선이 실선으로 바뀜. 이 때 cycle이 생기지 않으면 자원을 할당한다.

![9](C:\Users\pc\Desktop\발표\이미지\9.jpg)

 위와 같은 상황에서 P2가 R2를 요청해 할당받게 되면 Cycle이 생기게 된다. 이러한 경우 Resource를 할당받지 않고 P1이 끝나기를 기다린다.

2. **Multiple instances (Banker's Algorithm)**

- 가진 자원 정보를 가지고 safe state가 존재하는지 확인하고 하나의 경우라도 존재하는지 확인하여 deadlock을 회피할 수 있는 알고리즘이다.
- 프로세스가 사용할 자원들을 미리 보고 Safe sequence를 찾는 알고리즘이다.

​	**사용되는 변수**

1. **Available** : 각 Resource 별로 할당할 수 있는 남은 인스턴스 수
2. **Max** : Process가 하나의 타입의 resource에 최대로 요구하는 인스턴스 수
3. **Allocation** : Process에 할당된 resource의  인스턴스 수
4. **Need** : Process가 필요로 하는 resource의 인스턴스 수

(1) **Safety Algorithm** - Safe state인지 검사하는 알고리즘

​		**데이터 구조 (스레드 : n, 자원 : m)**

  - **Available [m]** : Available[j] = =k인 경우, R(j)의 인스턴스 k개를 사용할 수 있다.
  - **Max[n*m]** : Max[i] [j] == k인 경우 , T(i)가 앞으로 최대 R(j)의 인스턴스 k개를 요청할 것이다.
  - **Allocation[n*m]** : Allocation[i] [j] == k인 경우, R(j)의 인스턴스 k개를 현재 할당하고 있다.
  - **Need[n*m]** : Need[i] [j] == k인 경우, R(j)의 자원을 k개를 더 요청할 것이다.

​		**규칙**

1. Work와 Finish를 길이가 m과 n인 백터로 만든다. 그리고 Work = Available로 Finish[i] = false로 초기화한다.

​		(i = 0, 1,..., n-1)

2. 두 가지 규칙을 확인한다.

​	a. Finish[i] == false

​	b. Need <= Work

​	두 가지 규칙 모두 맞지 않는다면 다음 스레드, 프로세스로 넘어간다.

3. Work = Work + Allocation

​		Finish[i] = true

​		로 바꿔준다.

4. Finish[i] == true 모든 i에 대해 맞다면 이 시스템은 Safe state이다.

(2) **Resource-Request Algorithm** - 새로운 자원을 요청할때 적용하는 알고리즘

1. Request(i) <= Need(i)인지 확인한다. 맞으면 다음 단계로 넘어간다.

2. Request(i) <= Available인지 확인한다. 맞으면 다음 단계로 넘어간다.

3. Request를 받아줬다고 가정하고

   Available = Available - Request(i)

​	   Allocation(i) = Allocation(i) + Request(i)

​	   Need(i) = Need(i) - Request(i)

​		를 적용한다.

​    이 후에 다시 **safety Algorithm**을 적용하여 safe state인지 확인한다.



**Safety Algorithm**  예제)

![10](C:\Users\pc\Desktop\발표\이미지\10.jpg)

각 자원의 타입은 A = 10, B = 5, C = 7이다.

 Need를 구해야하기 때문에 Need[i] [j]= Max[i] [j] - Allocation[i] [j] 사용해서 구해준다.

![11](C:\Users\pc\Desktop\발표\이미지\11.jpg)

그 후

![12](C:\Users\pc\Desktop\발표\이미지\12.jpg)

규칙을 이용해 구해준다.

**Resource-Request Algorithm** 예제)

![13](C:\Users\pc\Desktop\발표\이미지\13.jpg)

### 교착 상태 검출 후 회복

**Deadlock Detection**

- Detection은 Prevention과 거의 같은 방식을 이용한다.
- Detection도 single Instance와 multiple Instance로 나누어지는데 single instance는 회피와 마찬가지로 자원 할당 그래프를 활용한다.

- multiple instance의 경우, Banker's Algorithm과 거의 비슷한 알고리즘을 사용한다.

​     -> Need 대신 Request 사용

​	 -> 미래에 필요한 정보가 아닌 현재의 Request 사용

**Deadlock Recovery**

  -- Deadlock이 발생한 상황에서 복구하는 방법

1) Process Termination

- Deadlock이 일어난 모든 프로세스를 종료시키는 방법
- 하나의 프로세스씩 종료시키며 deadlock cycle이 없어질때까지 반복한다.

2) Check & Rollback

- 비용을 최소화할 victim process를 선정한다.
- safe state로 return하고 프로세스를 재시작한다.