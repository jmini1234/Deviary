# Future

### Async Programming (비동기 프로그래밍)

비동기적 동작은 프로그램이 다른 동작을 하는 도중에 일을 완성할 수 있도록 한다.

비동기적 동작은 return 타입을 `Future or Stream`으로 반환한다.

1. Future API
2. Asyn Await Keyword

  

### Situation

- Fetching data over a network
- Writing to a database
- Reading data from a file

### Future<T>

- `비동기 작업의 결과 값을 반환` (represents the results of an asynchronous operation)
- 싱글 스레드 환경에서 비동기 처리를 위해 존재
- 지금은 값이 없지만 미래에 요청한 데이터 or 에러가 담길 예정
- 따라서 future을 반환하는 함수를 호출하면, 함수는 해당 일을 queue에 넣고, uncompleted future을 반환, 모두 완료하면 complete 상태로 만들고 값이나 오류를 반환
- 2개의 state로 나뉨
    - *Uncompleted*
        
        : 비동기 작업이 호출되면 초기에는 완성되지 않은 상태
        
    - *Completed*
        
        : 비동기 작업이 성공
        

```dart
String createOrderMessage() {
  var order = fetchUserOrder();
  return 'Your order is: $order';
}

Future<String> fetchUserOrder() =>
    // Imagine that this function is
    // more complex and slow.
    Future.delayed(
      const Duration(seconds: 2),
      () => 'Large Latte',
    );

void main() {
  print('Fetching user order...');
  print(createOrderMessage());
}

// Fetching user order...
// Your order is: Instance of '_Future<String>'
```

<aside>
💡 Future를 사용해서 비동기처리 값을 받아오는 부분을 위해서는 async, await를 사용해야하며, 반환 값도 Future (미래에 받아올 것이라는 것을 뜻함)로 변경해야함

</aside>

🤘🏻 문제점 

1. Future를 반환하는 함수는 await를 사용해서 완료되기까지 기다려야 한다.
2. await를 사용하는 부분이 있다면 그 함수는 앞에 async를 선언해야 한다. 

```dart
**Future<String>** createOrderMessage() **async** {
  var order = **await** fetchUserOrder();
  return 'Your order is: $order';
}

Future<String> fetchUserOrder() =>
    // Imagine that this function is
    // more complex and slow.
    Future.delayed(
      const Duration(seconds: 2),
      () => 'Large Latte',
    );

**Future<void>** main() **async** {
  print('Fetching user order...');
  print(**await** createOrderMessage());
}

//Fetching user order...
//Your order is: Large Latte
```

### Async and await

- async 함수를 사용하기 위해서는 async를 함수 앞에 선언
- await 키워드는 async 함수에서만 작동
- async함수를 갖고 있다면, await 키워드를 사용해서 future값이 completed 할 때까지 기다림
    
    ```dart
    print(await createOrderMessage());
    ```
    

### 💫 참고

[Http response 병렬적 문제 개선하기](https://www.notion.so/Http-response-8f1188b1e82244a89b60dc8059712521)