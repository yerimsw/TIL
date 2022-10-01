## Java Array

- 배열은 동적으로 생성되는 객체로, 동일한 타입에 대해 일정한 크기의 저장공간을 제공합니다.
- 메모리 공간 상에 연속적으로 위치해 있으며, 일단 배열이 생성되면, 그 크기는 변경할 수 없습니다.
- 다음 구문을 통해 배열을 생성할 수 있습니다.
    
    ```java
    int array[] = new int[size];
    ```
    
- 배열의 크기를 넘어가는 범위의 인덱스에 접근하려 하면 ArrayIndexOutOfBoundsException 예외가 발생합니다.



## Java ArrayList

- 어레이 리스트는 컬렉션 프레임워크의 클래스 중 하나입니다.
- List, Collection, Iterable 인터페이스를 상속 및 구현합니다.
- 다음 구문을 통해 ArrayList를 생성할 수 있습니다.
    
    ```java
    ArrayList<Type> arrayList=new ArrayList<Type>();
    ```
    
- 어레이리스트는 동적으로 그 크기를 변경하는 것이 가능하다고 알려져 있지만, 사실은 새로운 배열 객체를 변경하려는 크기로 생성하여 이전 배열의 원소들을 복사하는 과정을 거칩니다.
- 이는 System.arraycopy라는 메서드를 통해 이루어지고, 메모리 공간을 더 사용하기 때문에, 프로그램의 성능을 저하시키는 요인이 됩니다.
- 객체만을 저장하는 자료구조이기 때문에, 원시 타입을 저장할 수 없습니다. JVM은 원시타입에 따라 해당하는 래퍼클래스 타입으로 객체를 생성하는, 오토 박싱을 진행합니다.



## Array vs ArrayList

| Basis | Array | ArrayList |
| --- | --- | --- |
| Adding Elements | 배열의 원소에 대입 연산자로 값을 할당한다. | add() 메서드로 원소를 추가한다. |
| Dimensional | 다차원 배열을 구현할 수 있다. | 항상 1차원 선형구조이다. |
| Iterating Values | for, for each 반복문을 통해 순회할 수 있다. | iIterator를 통해 순회할 수 있다. |
| Length | 배열 객체 내 length 변수가 있다. | size() 메서드로 길이를 알 수 있다. |
| Data type | 원시타입 및 객체를 저장할 수 있다. | 원시타입을 저장할 수 없다. 객체(레퍼런스타입)만을 저장한다. |
| Initialization | 배열 선언 시 저장할 데이터 타입 및 크기를 지정해야 한다. | 배열의 크기를 지정하지 않아도 된다. JVM에 의해 디폴트 크기인 10으로 자동 지정된다. |
| Resizable | 배열 길이를 변경할 수 없다. | 배열 길이를 변경할 수 있다. |
