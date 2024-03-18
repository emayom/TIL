# Context API는 "상태 관리" 도구인가?
[이전 글](https://github.com/emayom/TILs/blob/main/react/data-flow-in-react.md)에서는 리액트의 특징 중 하나인 단방향 데이터 흐름에 대해 언급했습니다.  
이번 글에서는 'Context API는 "상태 관리" 도구인가?'라는 원초적인 의문에 결론을 내리는 과정을 담아볼까 합니다.  

## 상태 관리(State Management)의 이해 
리액트를 사용하여 애플리케이션 개발을 하다보면 "상태 관리"는 피할 수 없는 중요한 부분입니다.  
컴포넌트의 계층 구조(tree)에 따라 위에서 아래로 흐르는 리액트의 **단방향 데이터 흐름**은 계층 구조가 깊어질수록 복잡한 상태를 관리하거나 여러 컴포넌트 간 상태를 공유하는 것이 더욱 어려워진다는 문제점을 안고있습니다. 

리액트에서는 상태 관리에 대한 명확한 가이드라인을 제시하고 있지 않기에 다양한 상태 관리 패턴을 기반의 라이브러리들(e.g. Redux, Recoil, Jotai, Zustand, Valtio, React Query, etc.) 이 등장하였으며, 현재는 상태 관리 _춘추전국시대_ 로 불릴 만큼 다양한 상태 관리 라이브러리가 현존하고 있습니다. 덕분에 라이브러리 각각의 접근 방식과 장단점을 파악하여, 애플리케이션 요구 사항에 적합한 상태 관리 라이브러리를 선택할 수 있는 역량까지 프론트엔드 개발자로서의 자질으로 자리 잡고 있는 듯합니다.

### 상태(State)란 
상태란 1) 시간이 지남에 따라 변하여 2) 렌더링 결과물(UI)에 영향을 주며, 3) **컴포넌트 내부적으로 관리되는 메모리**입니다.   
상태는 크게 **전역 상태(Global State)**, **지역 상태(Local State)** 로 분류할 수 있지만, 역할에 따라서는 UI 상태(UI State), 서버 상태(Server State), URL 상태(URL State), 폼 상태(Form State) 등의 다양한 유형으로 구분할 수 있습니다.  

<br/>

## Context API: 상태 관리 도구인가?  
> Context API는 [React v16.3.0](https://github.com/facebook/react/blob/main/CHANGELOG.md#1630-march-29-2018)부터 공식적으로 도입되었으며, 별도의 의존성 없이 리액트 애플리케이션 내부에서 사용할 수 있습니다.  

"상태 관리"라는 개념을 공부하다 보면 비교적 러닝 커브가 낮고, 추가적인 라이브러리 없이 사용할 수 있는 Context API를 useState, useReducer와 함께 사용하며  _"입문용 상태 관리 도구"_  정도로 설명하는 글들을 쉽게 찾아볼 수 있습니다. 그러나 머지않아 'Context API를 과연 상태 관리 도구로 설명하는 것이 옳은 것인가?'에 대한 의문이 생기기 시작할 것입니다. **사실상 상태의 "관리"는 useState, useReducer hook이 수행하고 있으니 말이죠.**  

그렇다면 Context API를 뭐라고 설명하는 것이 좋을까요?   
아무래도 아직까지는 <u>**Prop Drilling을 피하기 위한 의존성 주입 도구**</u> 정도가 적절하지 않을까 싶습니다.  
실제로 [리액트 공식 문서](https://react.dev/learn/passing-data-deeply-with-context)에서는 Context API를 상태 끌어올리기(Lifting state up)로 발생 가능한 Prop Drilling을 회피하며,  컴포넌트 트리의 깊은 곳에 있는 컴포넌트들에게 "props를 전달하기 위한 대안" 정도로 설명하고 있습니다. context는 아무것도 관리하지 않습니다. 전달받고, 전달할 뿐이죠. 

결론적으로 Context API는 상태 관리 도구는 아닙니다. 하지만, useState, useReducer hook과 함께 사용한다면 Redux와 유사한 방식으로 전역 상태를 관리하고, 관심사를 분리하는 것에 사용할 수 있습니다. 만약 단순 전역 상태 관리를 위한다면 충분히 목적을 달성할 수 있습니다.  

### Context를 사용하기 전에,
Context를 상태 관리를 위한 용도로 사용하기 전에 고려해야 하는 몇 가지 사항이 있습니다.  

1. **자주 변경되는 값을 관리하기에 적합하지 않습니다.**  
Context API는 Redux가 아닙니다. <u>Context API는 Provider의 value가 바뀔 때마다 context를 구독하는 하위의 모든 컴포넌트들은 다시 렌더링 되도록 동작합니다.</u> 때문에 context의 값이 자주 변경되는 경우에는 성능 저하가 발생할 수 있으므로, useMemo나 useCallback을 활용하여 별도로 성능을 최적화하는 것이 필요할 수 있습니다. 또한, 하나의 context에 연관성이 없는 value을 함께 저장한다면 적절하게 Context Provider를 분리하여 관리해야 합니다. 

2. **깊이 중첩된 Context는 코드의 가독성과 유지보수성이 저하될 수 있습니다.**
    ```jsx
    <ThemeContext.Provider>
        <UserContext.Provider>
        <SomeOtherContext.Provider>
            <YetAnotherContext.Provider>
            <OneMoreContext.Provider>
                <AndOneMoreContext.Provider>
                <AndSoOnContext.Provider>
                    <AndSoForthContext.Provider>
                    <App/>
                    </AndSoForthContext.Provider>
                </AndSoOnContext.Provider>
                </AndOneMoreContext.Provider>
            </OneMoreContext.Provider>
            </YetAnotherContext.Provider>
        </SomeOtherContext.Provider>
        </UserContext.Provider>
    </ThemeContext.Provider>
    ```
    무분별하게 Context를 추가하다 보면, 깊이 중첩된 Context로 인해 Provider Hell(Context Hell)이 만들어질 수 있습니다.  
    이는 코드의 가독성과 유지 보수성을 저하시킬 수 있으므로 주의해야 합니다.

3. **컴포넌트의 재사용이 어려워집니다.** 

<br/>

## 마치며 
이번 글은 'Context API를 상태 관리의 용도로 사용해서는 안 된다.'가 아니라 '어떤 용도로 생겨났는지, 그리고 상태 관리의 용도로 사용했을 때에 고려해야 하는 점들은 무엇인지'에 대해서 정확하게 언급하는 글이 되었으면 하는 목적으로 작성했습니다.  

Context API를 useState, useReducer와 함께 사용하여 전역 상태를 관리하고, 관심사를 분리하기 위한 목적으로 사용할 수 있습니다. 하지만, 단순 전역 상태 관리 이상 고수준의 상태 관리가 목적이라면 Context API로 한계가 있을 수 있기 때문에 여타 라이브러리를 사용하는 것이 더 적합할 수 있습니다. 

마지막으로, 상태 관리에 대한 다양한 글들을 서치하며 [우리 팀이 Zustand를 쓰는 이유](https://velog.io/@greencloud/%EC%9A%B0%EB%A6%AC-%ED%8C%80%EC%9D%B4-Zustand%EB%A5%BC-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0)라는 글을 읽게 되었습니다.   
상태 관리 도구를 선택하게 된 고민과 의사결정 과정이 잘 드러나 참고하기 좋은 글이라는 생각이 들었습니다.  

위의 글과 같이 상태 관리 라이브러리를 선택하기 이전에 '전역으로 관리할 상태가 얼마나 되는가?', '얼마나 많은 곳에서 참조하게 되는가?', '관리하고자 하는 상태의 유형은 무엇인가?'와 같은 고민들을 선제하여 **필요성을 정확하게 인지한 이후**, 애플리케이션 요구 사항에 적합한 상태 관리 라이브러리를 선택하기 위해 '접근 방식에 따른 상태 관리 유형', '유형별 장단점', '라이브러리의 업데이트 추이' 등을 파악하고 비교하는 과정이 수반되어야 한다는 점을 다시 확인할  수 있었습니다. 

<br/>

> #### References
> - [Why React Context is Not a "State Management" Tool (and Why It Doesn't Replace Redux)](https://blog.isquaredsoftware.com/2021/01/context-redux-differences/#:~:text=Therefore%2C%20Context%20is%20not%20a,based%20on%20React%20component%20state.)  
> - [uberVU - props vs state](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md#state)
> - [리액트 상태 관리 가이드](https://www.stevy.dev/react-state-management-guide/)
> - [프론트엔드 상태관리 실전 편 with React Query & Zustand](https://www.youtube.com/watch?v=nkXIpGjVxWU)