## 2.3 생성

### 컬렉션에서 생성

```java
  List<Integer> list=Arrays.asList(1,2,3,4,5);
    Stream<Integer> intStream=list.stream();
```

### 배열에서 생성

```java
  Stream<Integer> strStream1=Stream.of("a","b","c");
    Stream<Integer> strStream2=Stream.of(new String[]{"a","b","c"});
    Stream<Integer> strStream3=Arrays.stream(new String[]{"a","b","c"});
    Stream<Integer> strStream4=Arrays.stream(new String[]{"a","b","c"}, 0, 3); // 0~3으로 크기 제한
```

### 특정 정수를 갖는 스트림 생성

```java
  IntStream intStream = IntStream.range(1, 5);        // 1,2,3,4
  IntStream intStream = IntStream.rangeClosed(1, 5);  // 1,2,3,4,5
```


### 난수를 갖는 스트림 생성

```java
  IntStream intStream = new Random().ints().limit(5) // 무제한 크기라 5개로 제한
  IntStream intStream = new Random().ints(5)         // 크기가 5인 난수 스트림 반환
```

지정된 범위의 난수를 갖는 스트림 생성
- IntStream ints(int begin, int end) : 무한 스트림
- IntStream ints(long streamSize, int begin, int end) : 유한 스트림
- LongStream longs(long begin, long end)
- LongStream longs(long streamSize, long begin, long end)
- DoubleStream doubles(double begin, double end)
- DoubleStream doubles(long streamSize, double begin, double end)

### 람다로 생성

```java
  static <T> Stream<T> iterate(T seed, UnaryOperator<T> f);   // 이전 요소에 종속적
  static <T> Stream<T> generate(Supplier<T> s);               // 이전 요소에 독립적

  Stream<Integer> evenStream = Stream.iterate(0, n->n+2);     // 0,2,4,...
  Stream<Double> randomStream = Stream.generate(Math::random);
  Stream<Integer> oneStream = Stream.generate(()->1);
```

### 파일에서 생성

```java
  Stream<Path> Files.list(Path dir)   // Path는 파일 또는 디렉토리
  Stream<String> Files.lines(Path path)
  Stream<String> Files.lines(Path path, Charset cs)
  Stream<String> lines()    // BufferedReader 클래스의 메서드
```

## 2.4 중간연산

### 자르기 - skip, limit

``
  Stream<T> skip(long n)
``: n개 건너뛰기  
``
  Stream<T> limit(long maxSize)
``: maxSize이후는 버리기  
```java
  IntStream intStream = IntStream.rangeClosed(1, 10);  // 1,2,3...,10
  intStream.skip(3).limit(5).forEach(Stytem.out::print)  // 4,5,6,7,8
```

### 거르기 - filter, distinct

``Stream<T> filter(Predicate<? super T> predicate)``: 조건에 맞지 않는 요소 제거  
``Stream<T> distinct()``: 중복 제거

### 정렬 - sorted

``Stream<T> sorted()``: 기본정렬  
``Stream<T> sorted(Comparator<? super T> comparator)``: 지정된 comparator로 정렬  
```java
Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
strStream.sorted() // 기본정렬
```