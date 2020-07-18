---
layout: post
title: 'NestJs Swagger'
author: jaewan
categories: [typescript,backend,nestjs]
image: assets/images/nestjs.png
comments: false
---

# Nest.Js - Open API (Swagger)

Nest에서는 RESTful APIs Module로 Swagger 모듈을 제공한다. 데코레이터들을 사용하여 손쉽게 Swagger 제작이 가능하고, controller변화에 따라 swagger도 변화하기 때문에 유지/관리가 용이하다.

### Install
```
$ npm install --save @nestjs/swagger swagger-ui-express
```

### Bootstrap
```
//main.ts

import { NestFactory } from '@nestjs/core';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  //Swagegr 모듈 옵션
  const options = new DocumentBuilder()
    .setTitle('Cats example')
    .setDescription('The cats API description')
    .setVersion('1.0')
    .addTag('cats')
    .build();
  const document = SwaggerModule.createDocument(app, options);
  SwaggerModule.setup('api', app, document);

  await app.listen(3000);
}
bootstrap();
```

위와 같이 모듈 설정을 완료하면 `http://localhost:3000/api`에서 바로 swagger 문서를 확인할 수 있다.

##DTO
SwaggerModule은 `@Body()`, `@Query()`, and `@Param()`과 같은 Decorator들을 확인하고 해당 Decorator에 사용된 DTO를 기반으로 Swagger 문서가 작성된다.
```
//@Body() 데코레이터를 read하여, CreateCatDto Type을 Swagger 문서화한다.
@Post()
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

명시적으로 Controller에 api 요청사항을 작성하기 위해 controller에 직접 decorator를 사용할 수 있다.
* @apiOperation({operationId : "컨트롤러 이름", description : "controller 설명"}) - controller에 대한 상세 설명
* @ApiParam({scheme}) -  ApiParameter 설정 ( path에 /:val/로 값을 받는 형태)
* @ApiQuery({scheme}) - ApiQuery 설정 ( /?val=x 로 값을 받는 형태 )
* @ApiBody({scheme}) - Body 설정
* @ApiResponse({status : 200 , type: responsetype }) - apiResponse 설정

```
	@Post("/v1/user/:id")
	@ApiOperation({ operationId: "createUser" })
	@ApiBody({ type: CreateUserRequest })
	@ApiParam({ name : "id" , type: "Number" })
	@ApiResponse({
		status: 200,
		type: CreateUserResponse
	})
	async createUser(@Body() request: CreateUserRequest, @Param() id : number): Promise<CreateUserResponse> {
		return {...} as CreateUserResponse
	}
```




## @ApiProperty()
DTO의 파라미터들에 정보를 넣기 위해서는 각 파라미터에 `@ApiProperty()` 데코레이터를 붙여주면 된다.
```
import { ApiProperty } from '@nestjs/swagger';

export class CreateCatDto {
  @ApiProperty()
  name: string;

  @ApiProperty()
  age: number;

  @ApiProperty()
  breed: string;
}
```
`@ApiProperty()`데코레이터에는 다양한 옵션값을 넣을 수 있다.
* description : 설명
* minimum : 최소값
* default : 기본값
* type : 파라미터 타입 (type이 array인 경우 - `type:[string]`)
...

```
import { ApiProperty } from '@nestjs/swagger';

export class CreateCatDto {
  @ApiProperty()
  name: string;

  @ApiProperty({
  description: 'The age of a cat',
  minimum: 1,
  default: 1,
  })
  age: number;
  ...
}
```
