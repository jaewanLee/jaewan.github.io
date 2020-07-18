---
layout: post
title: 'next-typescript init setting'
author: jaewan
categories: [typescript,react,front-end,next]
image: assets/images/nestjs.jpg
comments: false
---

# next-typescript init setting

## project 생성
nextjs 프로젝트는 `create-next-app` 명령어를 사용하여 init해준다.
```
yarn create next-app
```
이후 생성되는 커멘트에 따라 project의 이름과 기본값을 설정한다.

## react 설치
```
npm install next react react-dom
```

## tsconfig 생성
```
tsc --init
```
생성된 tsconfig.json 에 원하는 설정들을 넣어준다.

## dev 실행
```
npm run dev
```
next 프레임워크를 dev로 실행할경우, tsconfig 파일을 인식하여 ts설정을 진행한다.
이때, 필요한 moduled이나 lib이 없을경우 command-line을 통해 알려주며, 이를 설치하고 재 실행시키면 된다.

## tsc파일 생성
pages 디렉토리에 기본으로 생성되어 있는 js파일을 삭제하고, index.tsc파일을 생성한다.
```
import React, { Component } from 'react'

class IndexPage extends Component{
	render(){
		return (
			<div>
				IndexPage
			</div>
		)
	}
}

export default IndexPage
```

## 확인
```
yarn dev
```
기본으로 3000포트에서 실행되며 실행여부를 확인한다.