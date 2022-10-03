### Java에서 컬렉션 정렬하기

- 컬렉션을 정렬할 때 오름차순, 내림차순 등 기준에 따라 정렬한다.
- 자바는 정렬 기준을 사용자가 정의할 수 있는 방법을 두가지 제시한다.
- 바로 `Comparable` 인터페이스와 `Comparator` 인터페이스다.

### Comparable

- 정렬할 클래스는 java.lang.Comparable 인터페이스를 구현해야 한다.
- 사용자 정의 클래스에 대해서도 정렬할 수 있다.
- `Comparable` 인터페이스는 `compareTo` 메서드를 지원하며, 이 메서드는 매개변수로 정렬될 클래스 타입의 객체를 받는다.
    
    ```java
    package java.lang; // 컴파일 시 자동으로 로드 되는 패키지, 따로 import 하지 않아도 됨.
    
    interface Comparable<T> {
      public int compareTo(T o);
    }
    ```
    
- 메서드를 호출한 객체가, 매개변수 객체보다 크면 양수, 같으면 0, 작으면 음수를 리턴하도록 구현을 강제한다.
    
    ```java
    public int compareTo(Movie m) {
    	return this.year - m.year;
    }
    ...
    class Main {
    	public static void main(String[] args) {
    		ArrayList<Movie> list = new ArrayList<>();
    		list.add(new Movie("Star Wars",1977));
    		list.add(new Movie("Return of the Jedi", 1983));
    
    		Collections.sort(list);
    		for(Movie movie : list) System.out.println(movie.toString());
    	}
    }
    ```
    
- Comparable을 구현한 메서드는 첫번째 Collections.sort() 메서드를 호출한다.
    
    ```java
    public static <T extends Comparable<? super T>> void sort(List<T> list) {
    	list.sort(null);
    }
    ```
    

### Comparator

- 정렬할 클래스는 java.util.Comparator 인터페이스를 구현해야 한다.
- Comparable과 마찬가지로 사용자 정의 클래스에 대해서도 정렬할 수 있다.
- Comparator 인터페이스는 compare 메서드를 지원하며, 클래스 내에서 오버라이드 하면 된다.
    
    ```java
    public interface Comparator<T> {
    	int compare(T o1, T o2);
    }
    ```
    
- `o1`이 `o2`보다 크다면 양수, 같다면 0, 작다면 음수를 리턴하면 된다.
    
    ```java
    class RatingCompare implements Comparator<Movie> {
      public int compare(Movie m1, Movie m2) {
        if (m1.getRating() < m2.getRating()) return -1;
        if (m1.getRating() > m2.getRating()) return 1;
    	  else return 0;
      }
    }
    ...
    class Main {
    	public static void main(String[] args) {
    		ArrayList<Movie> list = new ArrayList<>();
    		list.add(new Movie("Star Wars",1977));
    		list.add(new Movie("Return of the Jedi", 1983));
    
    		RatingCompare ratingCompare = new RatingCompare();
    		Collections.sort(list, ratingCompare);
    		for(Movie movie : list) System.out.println(movie.toString());
    	}
    }
    ```
    
- Comparator를 구현한 클래스는 두번째 Collections.sort() 메서드를 호출한다.
    
    ```java
    public static <T> void sort(List<T> list, Comparator<? super T> c) {
      list.sort(c);
    }
    ```
    

### Comparable vs Comparator

- Comparable을 구현했는지, Comparator을 구현했는지에 따라 호출하는 Collections.sort() 메서드가 다르다. (오버로드)
- Comparable의 compareTo 메서드는 호출한 객체와 매개변수로 주어진 객체를 비교한다.
- Comparator의 compare 메서드는 매개변수로 주어진 두 객체를 비교한다.
- Comparable 정렬은 하나의 오버라이딩 된 메서드를 사용하기 때문에, 한가지 속성만을 기준으로 정렬할 수 있다.
- Comparator는 애초에 클래스를 만드는 것이기 때문에, 여러 메서드를 오버로딩 할 수 있어서, 여러 속성을 기준으로 정렬할 수 있다.
