# 리덕스 세션 정리

## 리덕스를 써야하는 이유

useState()가 불편해서 사용하는 상태 관리 라이브러리

useState()를 사용했을 경우 컴포넌트간의 이동이 불필요하게 많다.

예를 들어 A 컴포넌트(B컴포넌트의 부모 컴포넌트), B 컴포넌트(A 컴포넌트의 자식 컴포넌트이자, C 컴포넌트의 부모 컴포넌트), C 컴포넌트(B 컴포넌트의 자식 컴포넌트)가 있다.

A 컴포넌트에 있는 state 값을 C 컴포넌트에 불러오려면 반드시 B 컴포넌트를 거쳐가야한다.

이와 같이 컴포넌트 사이에서 props를 주고 받는게 상당히 귀찮아진다.

이와 같은 귀찮음을 해결하기 위해 Redux라는 상태 관리 라이브러리를 사용한다.

Redux는 각각의 컴포넌트에 있는 props를 어느 한 구석에 모아놓고 사용하고자 할 때 쉽게 끌어올 수 있게 해준다.

단, 리덕스를 사용할 때와 useState()를 사용할 때가 존재한다는 점만 알아두자!

(여러 컴포넌트에서 state를 관리할 때는 리덕스, 하나의 컴포넌트에서만 state를 관리할 때는 useState())

## 리덕스를 사용하는 방법

리덕스를 쓰기 위해서는 리덕스의 부품들을 다 만들어둬야 한다. (ex: reducer, action type, action creator)

1. 구성요소의 데이터 형태로 기억해라!

- action type : number라는 값에 +1을 할 때 action 이라는 쪽지를 Redux에 보내주어야 동작하는데 이 쪽지를 날려주는 역할을 한다. (형태는 object type (객체))
```
const PLUS = "counter/PLUS"; // 행동을 문자열로 적어놓음
```

- action creator : action type을 큰 객체로 만들어주는 함수
```
const plus = () => {
    return { type: PLUS }; // action type의 객체를 리턴 (action type의 key인 'type : PLUS'를 선언)
}
```

- reducer : action 쪽지를 받아서 그에 맞는 행동을 하고 그 쪽지에 있는 결과를 리턴해준다.
```
const counter = () => {
    switch (key) {
        case value:
        break;

        default:
        break;
    }             // 조건에 따라 새로운 상태값을 리턴하기 때문에 switch같은 조건문이 들어가야 함
}
```

리덕스를 사용하기 위해서는 이 세가지가 필수로 구성되어야 하는데 action type, action creator, reducer를 한개의 파일 안에 작성하면 된다.

그리고 값을 export 시킬 때는 reducer를 export 시키면 된다.
```
const PLUS = "counter/PLUS";

const plus = () => {
    return { type: PLUS };
    }

const counter = () => {
    switch (key) {
        case value:
        break;

        default:
        break;
    }             
}

export default counter;
```

* type과 action creator가 생긴 이유는 값의 중복으로 인한 혼란을 줄이기 위해서 *

## reducer

reducer는 인자 자리 (props 자리)에서 두가지를 꺼내올 수 있다. 그 이유는? 그냥 리덕스 document에 써있기 때문

```
const counter = (state, action) => {
    switch (action.type) {  // switch 옆에는 action의 type이 들어감 (action creator 객체의 action type key)
        case PLUS:          // action type (쪽지)이 들어감
            return {};

        default:
            return state;   // 아무런 action이 일어나지 않을 때에는 state의 초기값을 리턴한다.
    }
}
```

## Redux Store

각 컴포넌트의 state 값들을 넣어놓는 창고와 같은 역할

```
const initialState = 0;

const counter = (state = initialState, action) => {
    switch (action.type) {  
        case PLUS:         
            return {};

        default:
            return state;   
    }
}
```

counter라는 reducer 안의 initialState라는 창고에 있는 값을 호출

## useDispatch

Store에 있는 action을 실행시키기 위해 사용됨 (사용 할 때)

```
----------------- counter.js --------------------------

const PLUS = "counter/PLUS";

export const plus = () => {
    return { type: PLUS };
}

const initialState = 0;

const counter = (state = initialState, action) {
    switch (action.type) {
        case PLUS:
        return state + 1;

        default:
        return state;
    }
}

----------------- CounterHome.js ----------------------

import {useDispatch} from "react-redux";

const CounterHome = () => {
    const dispatch = useDispatch();

    const onClickAddButtonHandler = () => {             // 액션을 실행시키기 위한 함수
        //dispatch({ type: " 리덕스 모듈/액션 타입 명 "}) // 원래 형식은 이렇지만 오타의 위험성이 있어서
        dispatch ({ type: plus() });                    // action creator 함수명을 직접 가져옴
    };
    
};
```

동작 순서 : dispatch가 reducer로 actio type을 보내고 type 값에 맞는 case를 reducer가 실행한다. 

* action type, action creator, reducer를 다 만들고 동작을 하기 위해선 dispatch부터 시작된다.

* dispatch로 실행이 되면 Store에 있던 기존의 state가 실행된 state 값으로 대체된다.

## useSelector

Store에 reducer를 사용해 넣어준 값을 불러오기 위해 사용됨 (가져올 때)

```
import {useSelector} from "react-redux";

const CounterHome = () => {

   const number = useSelector((state) => state);  // Store에 있는 state 인자를 받아옴

}
```

## Redux thunk

### middleware
----------------------------------------------------------------------------

 dispatch가 실행되고 reducer로 type이 전달되기 전에 어떠한 작업을 하기 위한 역할

----------------------------------------------------------------------------

