# Http response 병렬적 문제 개선하기

매우 간단하지만 http parallel request 를 몰랐을 당시 짰던 코드 ^^ 

**🧛 문제점** 

1. 여러개의 request를 날리는데, for문을 사용해 response가 오면 또 다른 request를 호출하는 방식
2. 시간이 오래 걸림

```dart
for(int i=0;i<krStockCodeList.length;i++){

      var stockCode = krStockCodeList[i];
      String currentDay = DateFormat('yyMMdd').format(DateTime.now());
      String url = "URL"
      http.Response response = await http.get(Uri.parse(url));

      Map<String, dynamic> json = jsonDecode(response.body);
      
      var code = json[krCodeKey].trim();
      var name = json[krNameKey].trim();
      int price = int.parse(json[krPriceKey]);
      double diffRatio = double.parse(json[krDiffRatio]);

      var tmpStock = KrStock(name: name, code: code, price: price, diffRatio: diffRatio);
      retStockList.add(tmpStock);
    }
    return retStockList;
```

---

### Future.wait

- Waits for multiple futures to complete and collects their results.
- don’t care about the order (상관 있다면 forEach() 사용)

[방법 1]

```dart
return Future.wait<KrStock>(krStockCodeList.map((stockCode) =>
      http.get("URI").then((response){
        Map<String, dynamic> json = jsonDecode(response.body);
				// json serializer
				return KrStock.fromJson(json);
      })
```

[방법 2]

```dart
List<Response> list = await Future.wait(krStockCodeList.map((stockCode) => http.get(Uri.parse("URL"))));
      
    return list.map((response){
      Map<String, dynamic> json = jsonDecode(response.body);
 			// json serializer
			return KrStock.fromJson(json)
	   }).toList();
```
