---
layout: post
title: 'ant-design'
author: jaewan
categories: [typescript,react,front-end,ant-design]
image: assets/images/antdesign.jpg
comments: false
---

# ant design
ant design은 react css framework로 bootstrap이나 materia-ui와 비슷한 개념이다.
typescript와 호환성이 좋고, docs가 잘 정리되어있어, 간단한 admin page등을 만들기에 적합하다. env support는 [여기](https://ant.design/docs/react/introduce)를 참고하면 된다.

## install
```
yarn add antd
```

## 사용
설치후 _app.tsx에 넣어준다. 이번 프로젝트에서는 nextjs와 함께 사용하기때문에 globalCSS방식으로 추가하였다.

global.css
```
@import '~antd/dist/antd.css';

```
_app.tx
```
import { AppProps } from 'next/app'
import "../src/assets/css/global.css"

function MyApp({ Component, pageProps }: AppProps) {
	return <Component {...pageProps} />
}

export default MyApp
```
index.tsx
```
import React, { Component } from 'react'
import { Button } from 'antd'

class IndexPage extends Component {
	render() {
		return (
			<div>
				<Button type="primary">
					ant button
				</Button>
				IndexPage
			</div>
		)
	}
}

export default IndexPage
```
 자세한 방법은 [여기](https://ant.design/docs/react/use-in-typescript)를 참고하면 된다.