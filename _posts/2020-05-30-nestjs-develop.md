---
layout: post
title: 'NestJs Dev'
author: jaewan
categories: [typescript,backend,nestjs]
image: assets/images/nestjs.png
comments: false
---

# NestJs

## Controller
Controller와 Get 데코레이터 모두 NestJs모듈을 사용한다.
service는 injection을 하지 않고, constructor의 생성자로 넣어준다
```
import { Controller, Get, Param } from '@nestjs/common'
@Controller()
export class itemController {
	constructor(private itemService: itemService) { }
```
## Service
```@service```데코레이터가 아닌 ```@Injectable```데코레이터를 사용한다.
repository를 injection할때도, ```InjectRepository``` 를 사용한다. 이때 꼭 nestJs모듈을 사용하도록 주의한다.

```
import { Injectable } from '@nestjs/common'
import { InjectRepository } from '@nestjs/typeorm'

@Injectable()
export class itemService {

	@InjectRepository(item, 'aka') private itemRepository: itemRepository

	public async getUrl() {
		const urls = await this.itemRepository.geturl()
		if (urls.length > 0) {
			return urls[0]
		} else return
	}
```

## Repository


## Common
세가지 모델들이 완성되면 ```modules``` 폴더에 등록해준다.