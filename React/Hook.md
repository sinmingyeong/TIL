# 1. 훅이란?
리액트의 state와 생명주기 기능에 갈고리(hook)을 걸어 **원하는 시점에 정해진 함수를 실행되도록 만든 것**

**✅ 훅의 이름은 모두 use로 시작한다.(개발자가 직접 커스텀한 커스텀 훅도 마찬가지)**

# 2. useState
state를 사용하기 위한 훅.
```jsx
import React, {useState} from "react";

function Counter(props) {   //함수컴포넌트 선언
    var count = 0;

    return ( //버튼 클릭 시 카운트 +1
        <div>
         <p>총 {count}번 클릭했습니다.</p>
         <button onClick={()=> count++}>  
            클릭
        </button>
        </div>
    );
}
```
🔼카운트를 함수의 변수로 선언할 시 카운트 값은 증가하지만 재렌더링이 일어나지 않아 새로운 카운트값이 화면에 표시되지 않는다.
> ```jsx
> const [변수명, set함수명] = useState(초깃값);
>```
>useState()를 호출할 때는 파라미터를 선언할 state의 초깃값을 넣어야한다. 초깃값을 넣은 useState()를 호출하면 리턴값으로 배열을 뱉는다.
```jsx
import React, {useState} from "react";

function Counter(props) {   //함수컴포넌트 선언
    var [count, setCount] = useState(0);

    return ( //버튼 클릭 시 setCount()함수를 호출해서 카운트 증가.
        <div>
         <p>총 {count}번 클릭했습니다.</p>
         <button onClick={()=> setCount(count + 1)}>  
            클릭
        </button>
        </div>
    );
}
```
🔼버튼 클릭 시 setCount()함수를 호출해서 카운트 증가시킨다. 그리고 count의 값이 변경되면 컴포넌트가 재렌더링되면서 화면에 새로운 카운트 값이 표시된다.

# 3. useEffect
사이드 이펙트를 수행하기 위한 훅 <br>
리액트에서의 사이드 이펙트는 효과, 영향같은 의미에 가까움. (서버에서 데이터를 받아오거나 수동으로 DOM 변경 등)

**✅클래스 컴포넌트에서 제공하는 생명주기 함수와 동일한 기능을 하나로 통합해서 제공한다.**
>```js
> useEffect(이펙트 함수, 의존성 배열);
>```
> + 이펙트 함수: 몬소리지
> + 의존성 배열: 이 이펙트가 의존하고 있는 배열, 이 배열 안에 있는 변수 중에 하나라도 값이 변경되었을 때 이펙트 함수가 실행된다.
> 
> 이펙트 함수는 컴포넌트가 처음 렌더링된 이후, 업데이트로 인한 재 렌더링 이후에 실행.
>
> 마운트와 언마운트할 때 한번씩만 실행되게 하고싶으면 빈 배열 []을 넣으면 된다.
>
> 의존성 배열을 생략하면 컴포넌트가 업데이트될때마다 호출된다. 

```jsx
import React, {useState} from "react";

function Counter(props) {   //함수컴포넌트 선언
    const [count, setCount] = useState(0);

    useEffect(()=> {    //이펙트 함수만 추가(의존성 배열 없음)
        document.title = `총 ${count}번 클릭했습니다.`;
    });

    return ( //버튼 클릭 시 setCount()함수를 호출해서 카운트 증가.
        <div>
            <p>총 {count}번 클릭했습니다.</p>
            <button onClick={()=> setCount(count + 1)}>  
                클릭
            </button>
        </div>
    );
}
```
🔼document의 title을 업데이트하는 이펙트 훅, **의존성 배열 없이** 사용되어 DOM이 변경된 이후에 해당 이펙트 함수를 실행하라는 의미가 되었다. **컴포넌트가 렌더링될 때마다 이펙트가 실행**된다.

### ✅useEffect()훅의 사용법
```jsx
useEffect(() => {
    //컴포넌트가 마운트 된 이후에 의존성 배열에 있는 변수 중 하나라도 값이 변경되면 실행된다.
    //의존성 배열에 빈 배열을 넣으면 마운트와 언마운트 시 한 번씩만 실행된다
    //의존성 배열 생략 시 컴포넌트  업데이트마다 실행된다.

    return() => {
        //컴포넌트가 언마운트되기 전에 실행된다.
        ...
    }
}, [의존성 변수1, 의존성 변수2, ...]); 
```

# 4. useMemo 
> ### 🚀 메모이제이션(Memoization)
> 비용이 높은(연산량이 많이 드는)함수의 호출 결과를 저장해뒀다가 같은 **입력값으로 함수를 호출하면 이전에 저장해둔 호출 결과를 바로 반환하는 것**이다. ➡️중복 연산을 피하고 결과를 빠르게 얻을 수 있다.

> ```jsx
> cosnt memoizedValue = useMemo (
>    () => {
>        //연산량이 높은 작업을 수행해 결과 반환
>        return computeExpensiveValue(의존성 변수1, 의존성 변수2);
>    },
>   [의존성 변수1, 의존성 변수2]
>);
>```
> 의존성 배열에 들어있는 변수가 변했을 경우에만 새로 create 함수를 호출해 결과값을 반환. 그렇지 않은 경우에는 기존 함수의 결과값을 반환한다. 
> 
> useMemo()로 전달된 함수는 렌더링이 일어나는동안 실행된다. ➡️렌더링하는 동안 실행되면 안되는(useEffect()훅에서 실행되어야 하는 사이드 이펙트 등) 작업을 useMemo() 훅의 함수에 넣으면 안된다.
>
> 의존성 배열을 넣지 않는 것은 아무 의미 엇으므로 빈 배열이라도 넣어야 한다.(컴포넌트 마운트 시에만 함수 실행)
>
# 5. useCallback
useMemo()는 함수의 결과값을 반환하지만 useCallback()은 함수를 반환한다. 

> ```jsx
> cosnt memorizedCallback = useCallback(
>    () => {
>        doSomething(의존성 변수1, 의존성 변수2);
>    },
>    [의존성 변수1, 의존성 변수2]
>);
>```
> 의존성 배열에 있는 변수 중 하나라도 변경되면 메모이제이션이 된 콜백 함수를 반환한다. 
>
> 콜백 훅을 사용하지않고 컴포넌트 내에 함수를 정의하면 렌더링때마다 함수가 새로 정의되어 불필요한 반복작업이 생기기 때문에 사용된다.

# 6. useRef
useRef() 훅은 레퍼런스 객체를 반환한다. 

 **레퍼런스(참조): 특정 컴포넌트에 접근할 수 있는 객체. <br>
 .current라는 속성은 현재 레퍼런스(참조)하고 있는 엘리먼트를 의미한다.**

> ``` jsx
> const refContainer = useRef(초기값);
>```
> 파라미터로 들어온 초깃값으로 초기화된 레퍼런스 객체를 반환한다.
> 
**✅useRef() 훅은 변경 가능한 .current 속성을 가진 하나의 상자다.**

> ```jsx
> function TextInputWithFocusButton(props) {
>   //useRef() 훅 초기값 null    
>    const inputElem = useRef(null);
>
>    const onButtonClick () => {
>    // current는 마운트된 input element를 가르킨다.
>    // 클릭 시 호출되어 실제 엘리먼트에 접근해 focus()함수를 호출한다.
>     inputElem.current.focus();
>   };
>
>   return (    //버튼 클릭 시 input 태그에 포커스.
>    <>
>       <input ref={inputElem} type="text" />
>       <button onClick={onButtonClick}>Focus the input </button>
>   </>
>   );
>}
>```

# 7. 훅의 규칙
1. 훅은 리액트 함수 컴포넌트의 최상위 레벨(Top Level)에서만 호출해야한다.(반복문이나 조건문, 중첩된 함수 안에서 호출하면 안됨.)
2. 컴포넌트가 렌더링될 때마다 매 번 같은 순서로 호출되어야 한다.
3. 리액트 함수 컴포넌트에서 호출하거나 직접 만든 커스텀 훅에서만 호출할 수 있다.