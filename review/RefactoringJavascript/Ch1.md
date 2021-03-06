# 1장 리팩토링이란?

------

리팩토링은 코드를 변경하는 것이 아니다.

정확하게는 코드를 변경하는 것은 방법중 하나지만, '코드 동작'을 바꿔선 안 된다.



------

### 1.1 코드 동작이 변하지 않는다는 것을 어떻게 보장할 수 있을까?

- 구현 세부 사항
- 불특정하고 검증되지 않은 동작
- 성능

테스트와 버전 관리를 사용, 즉 코드 변경과 안정성을 보장하는 자동화 도구를 사용

##### 리팩토링한 코드가 제대로 실행되지 않은 때 변경 전 코드로 되돌릴 수 있도록 반드시 버전 관리를 사용하여 단계적으로 리팩토링을 진행한다.



### 1.2 구현 세부 사항에 관심을 두면 어떨까?

------

**구현 세부 사항**이란, **코드가 소비되는 코드에 의해 생성되는 동작**을 말한다.

```Javascript
function byTwo (number) {
	return number * 2;
}

function byTwo (number) {
    return number << 1;
}
```

위에 두 함수 모두 동작을 할 것이다. 둘다 같은 결과를 반환할 것이고, 함수 내부 동작보단 해당 결과 값에 더욱 관심을 갖을 것이다.

하지만, 두 번째 함수는 값이 너무 커지면 쉬프트 연산자로 부터 문제가 생길 것이다. 그리고 우리는 구현 세부 사항에 신경을 쓰는 것이 아닌 당연히 결과값이 잘 못된 것이라고 생각할겁니다.

이 사례로 **테스트 케이스가 처음 생각한 것보다 더 많은 경우를 포함해야 한다는 것**을 알 수 있다.



### 1.3 불특정하고 검증되지 않은 동작에 관심을 두면 어떨까?

------

코드 구체화 및 검증은 해당 동작에 관심을 보이려는 노력.

실행 테스트나 수동 절차, 동작법에 대해 최소한의 설명이 없으면 기본적으로 해당 코드를 검증할 수 없는 것을 의미

```Javascript
//테스트나 문서 및 관리 운용에 대한 지원이 없는 코드
function doesThings (args, callback) {
    doesOtherThings(args);
    doesOtherOtherThings(args, callback);
    
    return 5;
}
```

위 함수의 동작이 바뀌었다면 관심을 가졌을까? 대답은 No에 가까울 것이다.

그렇다면, 위 함수를 이해할 수 없으니 중요하지 않다고 판단해도 될까? 이 대답은 확실히 No다.

##### 자동이나 수동 여부에 상관없이 테스트를 거치지 않은 코드는 리팩토링할 수 없다.



### 1.4 성능에 관심을 두면 어떨까?

------

보통 초기에는 성능보단 기댓값을 가져다주는 입력 값에 더 관심을 두곤 한다. 즉, **사용자는 성능보단 동작과 기댓값에 더 중점을 둔다.**

좋은 결과는 적절한 시간 안에 입력으로 기댓값을 산출할 수 있다는 의미이고, 반대라면 **구현부를 변경**해야 합니다. **변경하기 전에 입력과 결과에서 검증**을 걸쳐야 코드의 리팩토링과 구현부 변경에 확신 할 수 있다. 검증을 안 한다면 이 모든 것 (입력과 결과)가 제대로 동작하지않아 무용지물이 될 수 있다.

위에 내용은 리팩토링보단 프로그램의 정확성처럼 성능 특성으로 검증하는 것으로 비기능적 검증, 성능용 테스트로 볼 수 있다.

결국 성능을 고려하는 것은 우리가 기대하는 기대치와 테스트를 경정하기 전까지는 부차적인 문제이다.



### 1.5 동작이 변하지 않는다면 리팩토링의 요점은?

------

요점은 **행동을 유지하면서 품질을 향상하는 것**이다.



### 1.6 품질 균형 잡기와 일 끝마치기

------

- 잘 만들었지만 실효성이 떨어지는 코드

- 많은 기능을 지원하지만 버그들과 완성되지 않은 기획으로 가득 차 있는 코드

소규모 프로젝트에서 가시적이고 설명 가능한 [기술적 부채](http://www.itworld.co.kr/news/108316)는 어느 정도 허용될 수 있지만, 큰 프로젝트에서는 품질을 최우선으로 진행해야 한다.



### 1.7 품질이란 무엇이며 리팩토링과 어떤 관련이 있을까?

------

##### 코딩 원칙

- SOLID : 단일 책임 , 개방 / 폐쇄, 리스코브의 대체, 인터페이스 분리 및 종속성 반전
- DRY  : 반복하지 말 것
- KISS : 정말 간단해야 할 것
- GRASP : 일반적인 책임 할당 소프트웨어 패턴
- YAGNI : 나중에 필요 없어질 것을 고려 (길게 봐라)

하지만, 확실한 품질의 기준은 없다. 자신만의 '소프트웨어 품질 원칙'을 구성해봐라.

리팩토링 맥락에서 품질은 **목표**이다.

이 책을 읽기 전에 필자는 패닉에 빠져 있었다. 왜냐하면, 개발을 하면서 한 개를 알면 세 개를 더 알아야 하는, 특히 프론트엔드 개발 쪽은 정말 새로운게 자고 일어나면 생겨있는 시점에서 내가 짜는 코드는 항상 **쓰레기**라고 생각을 했기 때문에 뭔가 하나를 잡고 진행을 할 수 없었다.

> ##### 매우 다양한 소프트웨어 문제를 해결하고 이것을 처리할 도구도 매우 많기에, 여러분이 첫 번째로 떠올리는 답은 대부분 최선이 아닐 것입니다. 그리고 항상 최선의 답만을 적어 내려는 것은 실용적이지 못합니다.

뒷 통수를 한 대 맞은 기분이였다. 위에도 말했듯 품질의 정확한 기준은 없다. 내가 짜는 코드가 항상 Best of Best면 좋겠지만, 안되고, 요구사항에 따라 다이나믹하게 변할 수 있기때문이라고 생각한다. 테스트가 왜 필요하고, 리팩토링이 왜 필요한지 말을 하라고 한다면 **나는 항상 최선의 품질을 목표하며 코딩을 하지만, 품질엔 명확한 기준이 없고 내 코드에 품질에대한 객관적인 점수를 할 수 없다. 내 코드는 계속해서 더 좋아질 수 있다. 이를 위해서 필요한게 테스트와 리팩토링이다.**



### 1.8 탐험으로서 리팩토링

------

리팩토링은 일반적으로 코딩에 자신감을 갖게 하고 작업 중인 내용을 잘 이해하는데 도움을 줍니다.



### 1.9 어떤 것이 리팩토링이고 어떤 것이 아닐까?

------

##### 리팩토링이 아닌 것들

- 계산기 애플리케이션에 제곱근 기능 추가
- 처음부터 앱 및 프로그램 만들기
- 새 프레임워크에서 기존 앱 및 프로그램 재구축
- 애플리케이션 또는 프로그램에 새 패키지 추가
- 이름 대신 성과 이름으로 사용자 주소 지정
- 지역화
- 성능 최적화
- 다른 인터페이스를 사용하도록 코드 변환

**테스트를 거치지 않은 코드 변경은 동작을 보장할 수 없으므로 리팩토링이 아니라 단지 코드만 변경하는 것입니다.**

### 1.10 마무리

------

이 장에서는 리팩토링의 개념과 함께 간단한 예제로 어떤 것이 리팩토링이 아닌지 알아보았습니다.

프레임워크나 라이브러리는 품질에 대한 사항에서 답이 될 수 없습니다. 

나는 리팩토링이 그냥 무작정 뜯어 고치는 것인줄 알았는데, 이 책을 읽어보니 리팩토링도 리팩토링만의 규칙들이 있었으며 이 규칙을 벗어나는 것들은 리팩토링이 아닌 것을 알 수 있었다. 1.9의 예시를 읽고서 이 책의 앞에 말한 여러 어려운 말들이 어떤 것들인지 이해를 할 수 있었다.

2장에서는 자바스크립트 자체에 대한 배경지식을 설명한다고 한다. 개념에 대한 이해들이 당장에 실무에 도움이 되지는 않겠지만, 결국 전반적으로 꼭 필수적인 것 같다.