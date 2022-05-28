# styled-Components

단순한 부분으로 인해 시간을 많이 잡아먹음

일반적인 css 태그의 경우 
```
<div style = {{fonst-size : "30px"}}>

</div>
```

styled-Components의 경우
```
const Div = styled.div`
fontSize : 30px;
`
```

# props 사용해서 style 주기

```
import styled from "styled-components";

...
<MyStyled bg_color={"red"}/>
...

const MyStyled = styled.div`
    background-color: ${(props) => (props.bg_color)}
    
    // ${(props) => { return props.bg_color; }} 를 위와 같이 표현가능

    &:hover{
        background-color: yellow;
    }
    // &를 사용하여 나 자신에 대한 style을 지정할 수 있다.
`;
```



