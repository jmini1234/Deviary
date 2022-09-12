# 상태관리 라이브러리 (Provider)

Flutter에서는 상태가 굉장히 중요, 데이터가 여러 화면에서 조작될 수 있기 때문에 효과적인 상태 관리 방법이 필요함

- Provider 패턴
    - Provider
        
        크게 `소비`와 `생성` 부분으로 나뉜다. 
        
        - **ChangeNotifier**
            - notifyListeners()를 기다리다가 호출되면 `자신의 자식을 재빌드`하여 UI를 업데이트
            - 값이 변경되면 리스너에게 notify 할 수 있는 클래스
            
            ```dart
            //view_model/home_page_view_model.dart
            
            class HomePageViewModel extends ChangeNotifier{
                List<StockViewModel> stocks = [];
                List<StockViewModel> allStocks = [];
            
                Future <void> load() async{
                  var result = await Future.wait([Webservice().getStocks()]);
                  stocks = result.map((stock) => StockViewModel(stock: stock)).toList();
                  // debugPrint("$stockslist");
                  notifyListeners();
                }
            ```
            
        
        - **ChangeNotifierProvider<Class> (생성)**
            - Class에 대한 Provider 생성
            - 하위 위젯에 **ChangeNotifier**를 제공해주는 클래스
            
            ```dart
            // main.dart
            return MaterialApp(
                  title: 'Stocks',
                  home : ChangeNotifierProvider<HomePageViewModel>(
                    create : (_) => HomePageViewModel(),
                    child : HomePage()
                  )
                );
            ```
            
        
        - **Consumer (Provider.of~)**
            - provider의 값을 받아서 실제로 사용하는 부분
            - 빌드가 되는 부분엠나 감싸야 비효율적인 리빌딩을 막을 수 있음
            
            ```dart
            // pages/home_page.dart
            @override
              Widget build(BuildContext context){
            		//데이터만 변경하고, UI를 변경하지 않는 곳에서는 listen:false
                final vm = Provider.of<HomePageViewModel>(context, listen: false);
            ```
