---
layout: post
title: 'NestJs Provider'
author: jaewan
categories: [typescript,backend,nestjs]
image: assets/images/nestjs.png
comments: false
---

## Provider (NestJs)

기본 nest class는 provider로 취급된다. provider들은 기본적으로 의존성 주입(`Inject dependency`)를 기본 개념으로 갖게됩니다. 이말은 NEST의 runtime에 object들이 다양한 관계를 형성할 수 있다는 의미가 됩니다. Provider는 간단하게 `@Injectable()` 데코레이터를 사용하여 해결이 가능합니다. `@Injectable()`데코레이터는 메타데이터에 값을 등록하여, Nest에게 이 class가 NestClass라는 것을 알립니다. 이후 controller를 비롯한 nest의 기능에서 사용이 가능합니다.

![Controller-예시](https://docs.nestjs.com/assets/Components_1.png)
가령, Controller는 http,i/o등을 위해 value와 Componet,Factory등의 다양한 기능들을 필요로 하고, 이를 각각의 provider 즉, nestJs의 class로 만들어, 이를 조합하여 controller에서 사용할 수 있도록 합니다.

## Service example
CatController에서 사용하기 위한 CatService를 통해 provider의 예시를 살펴봅시다. 아래와 같이 CatService와 CatController가 존재합니다. CatService에는 `@Injectable()`데코레이터로 NestJs Class 즉, Provider임을 명시합니다. CatService는 CatController의 `constructor`를 통해 inject됩니다. 이 방식은 catService의 선언과 initialize를 진행해줍니다.

```
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

CatController
```
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

## Property Base Injection
만일 여러개의 provider를 Injection하는 경우, 모든 provider를 super()에서 초기화하게 된다면 굉장히 느려질것입니다. 이런 경우에는 property에 `@Inject`데코레이터를 사용하여 가능합니다.
```
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```
주의하셔야 할 점은 다른걸 상속받지 않는 prodiver에서는 사용이 불가능합니다.


## Provider registration
이렇게 생성된 Provider들은 `app.module.ts`에 등록하여 Nest가 Inject가능하도록 합니다.
```
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```


### DI fundamental
DI(dependency injection)은 IOC(inversion of control)테크닉으로 코드상으로 명시적으로 의존성을 인스턴스화 하지 않고, 이를 IOC container(Nest에서는 nestJS runtime system)
에게 대리하는 것이다. 가령 NestIOCContainer인 `app.modules.ts`에 `Catcontroller`와 `CatService`를 등록하는 아래 코드는 다음과 같은 프로세스로 진행된다.
CatService.ts
```
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  findAll(): Cat[] {
    return this.cats;
  }
}
```
CatController.ts
```
import { Controller, Get } from '@nestjs/common';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

app.modules.ts
```
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```
1. CatService의 `@Injectable` 데코레이터를 선언하여 해당 class가 NestIoC container에서 관리가 가능하도록 한다.
2. CatsController에서 constructor injection을 통해 CatService에 대한 의존성을 선언한다.
3. app.module.ts에 `CatsService`의 토큰을 등록한다.

이렇게 등록된 provider는 runtime시 아래와 같은 프로세스로 진행된다.
* Nest IoC conatiner에서 CastController 인스턴스를 생성할때, 먼저 다른 dependency를 검토한다.
* CatService를 발견하게되고, CatService Token을 확인하여 CatServiceClass를 return한다.
* singleton scope(defulat)일 경우, CatService instance를 생성하여 cacheing하고 return한다. 만일 이미 존재할 경우 생성되어있는 instance를 return한다.

