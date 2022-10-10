## React Life Cycle 

----


1. Mounting : 컴포넌트가 브라우저 상에 나타나는 경우

2. Updating : 컴포넌트의 props가 변경되거나, state가 변경된 경우

3. Unmounting : 컴포넌트가 브라우저 상에서 사라지는 경우

----


1) constructor 
    - 컴포넌트의 state와 같은 속성들을 초기 설정이나 컴포넌트가 만들어지기 전 이루어져야할 작업이 있는 경우
2) getDerivedStateFromProps
    - props로 받은 값을 state로 동기화 하고 싶은 경우
3) render 
    - 어떤 dom을 만들게 될지, 내부에 있는 tag에 어떤 값을 전달해줄지 정의
4) componentDidMount
    - 컴포넌트가 나타난 후에 어떠한 이벤트를 처리하고 싶은 경우
    - 외부 라이브러리 API 처리
5) shouldComponentUpdate
    - 컴포넌트가 업데이트 되는 성능을 최적화 시키고 싶은 경우
    - 반환값 설정 가능 ( true, false )
    - if true) render를 실행시켜 가상 DOM을 그리고, 차이점을 비교하여 변경된 부분을 찾아내어 그려준다.
      if false) render를 실행시키지 않는다.
6) getSnapshotBeforeUpdate
    - rendering을 한 다음에, 결과물이 화면에 나타나기 바로 직전에 호출되는 함수이다.
7) componentDidUpdate
    - 컴포넌트가 업데이트 되고 난 후 호출되는 함수이다.
8) componentWillUnmout
    - 컴포넌트가 사라지는 시점에서 호출되는 함수이다.
    - 설정한 Listener를 제거하는 작업을 주로 한다.


번외

1) componentDidCatch
    - error가 발생할 수 있는 컴포넌트의 부모 컴포넌트에 설정을 해주어야한다.