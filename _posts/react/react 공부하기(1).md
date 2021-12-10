---
layout: post
title: "React 공부하기"
date: 2021-11-10
excerpt: "`생성:Mount`-`업데이트:Update`-`제거:Unmount` 세가지 유형 Mount : 컴포넌트가 처음 실행될 때 DOM에 생성되고 웹 브라우저 상에서 나타나는 것, constructor : 컴포넌트의 생성자 메서드, 초기 state를 설정할 수 있다"
tags: [react,web]
category: [React] 
comments: false
---

# 리액트 라이프 사이클
1. constructor
2. compenentWillMount
3. render
4. componentDidMount

`생성:Mount`-`업데이트:Update`-`제거:Unmount` 세가지 유형
	* Mount : 컴포넌트가 처음 실행될 때 DOM에 생성되고 웹 브라우저 상에서 나타나는 것
		* constructor : 컴포넌트의 생성자 메서드, 초기 state를 설정할 수 있다

			```js
				constructor(props){
					super(props);
					console.log("constructor");
				}
			```

		* getDerivedStateFromProps : props로 받아온 것을 state에 넣을 때 사용

			```js
				static getDerivedStateFromProps(nextProps, prevState) {
    				console.log("getDerivedStateFromProps");
    				if (nextProps.color !== prevState.color) {
      					return { color: nextProps.color };
    				}
    				return null;
    			}
			```

		* render : 컴포넌트 렌더링
		* React [DOM](#DOM) 및 [refs](#refs) 업데이트
		* componentDidMount
	* Update : `New props:props가 바뀔 때`,`setState():state가 바뀔 때`,`forceUpdate():강제로 렌더링을 시도할 때`,'리렌더링 될 때' 업데이트
		* getDerivedStateFromProps
		* shouldComponentUpdate : props나 state를 변경했을 때, 리렌더링 여부 결정하는 메서드. boolean값을 반환해야한다. 
		* render
		* getSnapshotBeforeUpdate : 컴포넌트에 변화가 일어나기 직전의 DOM state를 가져와서 특정 값을 반환하면 그 다음 발생하기 되는 componentDidUpdate 함수에서 받아와서 사용가능

			```js
				getSnapshotBeforeUpdate(prevProps,prevState){
						console.log("getSnapshotBeforeUpdate");
						if(prevProps.color!==this.props.color){
							return this.myRef.style.color;
						}
						return null;
				}
			```

		* React DOM 및 [refs](#refs) 업데이트
		* componentDidUpdate : 리렌더링 후 화면 반영 뒤 호출되는 메서드, 3번째 파라미터로 getSnapshotBeforeUpdate 에서 반환한 값을 조회가능

			```js
				componentDidUpdate(prevProps, prevState, snapshot)
					console.log("componentDidUpdate",prevProps,prevState)
					if(snapshot){
						console.log("업데이트 되기 직전 색상: ",snapshot);
					}
				}
			```

	* UnMount : DOM에서 컴포넌트가 제거되는 것
		* componentWillUnmount

## Hooks

## DOM

## refs