---
layout: post
title: 'NestJs Module'
author: jaewan
categories: [typescript,backend,nestjs]
image: assets/images/nestjs.png
comments: false
---

# Module (nestjs)

모든 application은 최소 하나의 모듈을 갖고있으며, 그 중 rootModule은 nestJS가 application graph(모듈과 provider,dependency등을 표현한 내부 데이터 구조)를 만드는 시작점이 됩니다.
![이미지](https://docs.nestjs.com/assets/Modules_1.png)

모듈은 아래와같은 4가지 property를 갖고있습니다.
* providers : Nest Injector에 의해 생성되고, 이 module내에서 공유되는 class
* controllers : 이 module내의 컨트롤러 set
* imports : 이 module내에서 필요한 provider들의 list
* exports : 이 module에서 제공되는 `providers`의 set으로 이 module을 import한 다른 module에서 사용 가능한 providers이다.

### Feature modules
이전에 우리가 보았던 `catController`와 `catService`는 기능이 유사하기때문에 이 둘을 하나의 Module에 묶을 수 있다.

CatModules
```
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

이후 `CatModule`을 root module인 `app.module.ts`에 추가합니다.
```
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```