# useState

```
const [state, setState] = useState(initialState);
```
- state : 상태 유지값
- setState : 그 값을 갱신하는 함수

```
setState(newState);
```
다음 리렌더링 시에 useState를 통해 반환받은 첫 번째 값은 항상 갱신된 최신 state가 된다.