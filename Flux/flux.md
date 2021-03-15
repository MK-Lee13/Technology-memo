# <span style="font-weight: bold;">FLUX</span>

## <span style="font-weight: bold;">디자인패턴</span>
디자인 패턴이란 프로그램 개발 내에서 반복적으로 일어나는 문제들의 대한 해결 방법 중 하나입니다.<br>
개발 하는 과정에서 발견된 노하우를 축적하여 이름을 붙이고, 이를 재이용하기 좋은 형태로 특정의 규약을 묶어서 정리한 것입니다.<br>
개발 상황에서 구조적인 문제를 해결하는 방식을 제시해줍니다.


## <span style="font-weight: bold;">MVC 디자인패턴</span>
![MVC](./MVC.png)

FLUX 디자인 패턴을 언급하기 전에 <span style="font-weight: bold;"> MVC 디자인 패턴</span> 에 대해서 먼저 언급하고 지나가도록 하겠습니다.<br>
MVC 는 Model, View, Controller 의 약자로서 하나의 어플리케이션이 있다고 가정 할 때<br>
해당 어플리케이션의 구성요소를 Model, View, Controller 3가지 역활로 구분한 디자인 패턴입니다.

### <span style="font-weight: bold;">Model</span>
- 모델이란 어떠한 동작을 수행하는 코드를 의미합니다. 이때 사용자에게 어떻게 보일지에 대해서는 고려하지 않습니다.<br>
사용자의 요청(query를 날린다)에 대한 상태정보를 제공하거나 상태를 수정하는데 이는 데이터를 의-미하며 Database의 Data를 조작하는 Layer입니다. 

### <span style="font-weight: bold;">View</span>
- 뷰는 말 그대로 사용자에게 보여지는 인터페이스를 의미합니다.

### <span style="font-weight: bold;">Controller</span>
- 컨트롤러는 모델과 뷰간의 인터랙션을 관리합니다. 모델에게 데이터 상태를 변경하도록 지시하고, 사용자에게 제공할 화면을 뷰에게 요청하는 작업을 컨트롤러가 맡게 되는 것 입니다.<br>
그럼으로 모델이 뷰에게 직접적으로 접촉할 수 없습니다.

## <span style="font-weight: bold;">한계</span>
어플리케이션의 규모가 커지면 커질수록 한계가 명확하게 드러나게 됩니다.<br>
뷰는 화면을 구성하는 하나의 단위이며 이는 복수가 될 수 있습니다. 또한 하나의 뷰에 복수의 모델이 연결되는 경우도 있습니다.<br>
이처럼 복잡한 화면과 데이터의 구성이 필요하게 된다면 이를 처리해야하는 컨트롤러는 복잡한 구조를 가지게 됩니다.<br> 
그렇게 될 경우 컨트롤러의 크기가 비대해져 하나의 기능 추가에도 많은 오류가 발생할 가능성을 가지게 됩니다. 

## <span style="font-weight: bold;">FLUX 디자인패턴</span>
이러한 MCV 디자인 패턴의 한계점으로 인해 나타난 가장 대표적인 버그가 페이스 북 알림 버그 입니다.<br>
어플리케이션의 규모가 커지면서 복수개의 모델이 뷰와 연결 되어 있고 또한 복수개의 뷰가 모델과 연결 되어 있고 모델이 모델과 연결되어 있고 <br>
굉장히 복잡한 구조를 뛰게 되면서 어플리케이션의 데이터 플로우를 해석할 수 없게 되자<br> 
페이스 북에서 다른 종류의 디자인 패턴을 시도 하였는데 이것이 바로 단방향 데이터 흐름 <span style="font-weight: bold;">FLUX 디자인 패턴</span> 입니다.<br>
이러한 FLUX 디자인 패턴의 특징은 데이터가 단방향으로만 흘러가고, 데이터가 변경이 되면 다시 처음으로 돌아가서 흐르는 방식으로 되어있습니다.

## <span style="font-weight: bold;">역활</span>
![FLUX](./flux.png)

위의 그림은 FLUX 디자인 패턴의 전반적인 다이어그램입니다. 최종적인 목표는 해당 다이어그램을 이해하는 것으로 보면 되겠습니다.<br>
다이어그램을 보면 데이터 흐름이 한방향으로 흐르기 때문에 데이터 변화에 대한 예측이 쉽습니다.<br>
또한 FLUX는 Action, Dispatcher, Store, View 이렇게 4가지 역활 구성원들이 있습니다.

### <span style="font-weight: bold;">Action</span>
- Dispatcher에서 콜백 함수가 실행되면 Store가 업데이트 되는데, 이때 해당 콜백 함수를 실행할 때 데이터가 담겨있는 객체를 인수로 전달하게 됩니다.<br>
이 전달 객체를 Action이라고 하며, 모든 Action은 Store 가 Dispatcher에 등록한 콜백을 통해 모든 Store에 전송됩니다.<br> 
이때 Action Creator 가 Action을 생성하고 Type을 설정하거나 Dispatcher에게 제공하는 역활을 합니다. 

### <span style="font-weight: bold;">Dispatcher</span>
- Dispatcher는 FLUX 디자인 패턴의 모든 데이터 흐름을 관리하는 역활을 합니다.<br> 
Action이 발생하게 되면 Dipatcher로 전달이 되는데, Dispatcher과 전달된 Action을 보고 등록된 콜백함수를 실행하여 Store에 데이터를 전달합니다.

### <span style="font-weight: bold;">Store</span>
- Store는 어플리케이션의 상태 저장소 역활을 합니다. 즉, 어플리케이션의 모든 상태 변경을 Store가 관리한다고 생각하시면 됩니다.<br>
Dispatcher로 부터 메시지를 수신 받기 위해서는 Dispatcher에 콜백함수를 등록하여야 합니다.<br>
상태가 변경이되면 View에 변경된 사실을 전달합니다.

### <span style="font-weight: bold;">View</span>
- FLUX의 View는 Controller 와 View 역활을 하는데, 화면에 보여주는 역활과 Store 에서 상태 변경이 이루어진것을 전달 받으면<br> 
이를 View에게 해당 내용을 전달해 새로 Rendering을 하게 하는 역활을 합니다. 

## <span style="font-weight: bold;">예시</span>
해당 디자인 패턴 플로우를 설명을하자면<br>
1. 가장 먼저 View가 사용자의 입력이 들어오면 Action Creator를 준비시킵니다.
2. 이후 Action Creator가 Action을 가공해 Dispatcher에게 넘겨줍니다.
3. Dispatcher는 전달받은 Action을 순차적으로 Store에게 전달하고, Store는 전달받은 Action을 통해 상태 변경을 합니다.
4. 이후 Store 의 상태를 Observing 하고 있는 View에게 상태 변경을 알립니다.
5. Store에게 상태 변경을 받으면 이를 통해 View가 새로 Rendering을 시도합니다.
- 이러한 흐름을 반복하게 됩니다.

## <span style="font-weight: bold;">결론</span>
React 와 Vue를 사용하면서 Redux를 이용하거나 Vuex를 이용하여 단방향 데이터 흐름을 구현을 한적이 있는데
항상 결과는 메모리 릭으로 끝이 나서.... (물론 제가 데이터 플로우를 잘못 설계해서 그렇지만 ㅠㅠ)
과유불급이라는 말이 필요한 디자인 패턴이라고 저는 생각합니다.(물론 저의 고찰입니다)