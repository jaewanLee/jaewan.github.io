---
layout: post
title: 'NestJs Pipe'
author: jaewan
categories: [typescript,backend,nestjs]
image: assets/images/nestjs.png
comments: false
---

# Pipe (NestJs)

## 특성
pipe에는 두가지 특성이 있다.
* transformation: input data를 원하는 형태로 바꿀수 있다. (string to integer)
* validation: input data가 valid한지 확인 할 수 있다.
두가ㅣㅈ 겨웅 모두, pipes는 controller의 `arguments` 에서 작동한다. Nest는 해당 method가 실행되기 직전에 끼어들고 pipe는 arguments를 받는다. 변환이나 검증 모두 그때그떄 일어난다.

## Built-in pipes
* ValidationPipe
* ParseIntPipe
* ParseBoolPipe
* ParseArrayPipe
* ParseUUIDPipe
* DefaultValuePipe

```
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```
위와 같이 build-int pipes를 파라미터에 사용하게되면 id값을 int로 치환해준다. 또, id값에 'abc'와 같은 string이 들어온다면 400(validation error)을 돌려준다.

## Custom pipes
```
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```
custom pipe는 반드시 `pipeTransform`을 상속받아서 `transform()` method를 구현해야하낟.
`transform()`은 두개의 parameter를 사용한다.
* value: 실제 파라미터가 작업하는 method의 argument값 이다.
* metadata: 현재 진행중인 method의 argument의 metadata이다. meta data는 아래와 같은 형태로 되어있따.

```
 export interface ArgumentMetadata {
 type: 'body' | 'query' | 'param' | 'custom';
 metatype?: Type<unknown>;
 data?: string;
}
```
