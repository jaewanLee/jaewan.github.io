---
layout: post
title: 'ClassTransformer'
author: jaewan
categories: [typescript]
image: assets/images/typescript_default.jpg
comments: false
---

# ClassTransformer

javascript에는 `{}` 노테이션을 사용한 plain Object와 `Class`를 사용한 instance object가 존재합니다. ClassTransformer는 이 둘간의 변환작업을 진행해주는 lib이다.

### 기본 사용

##### User.json
```
[{
  "id": 1,
  "firstName": "Johny",
  "lastName": "Cage",
  "age": 27
},
{
  "id": 2,
  "firstName": "Ismoil",
  "lastName": "Somoni",
  "age": 50
},
{
  "id": 3,
  "firstName": "Luke",
  "lastName": "Dacascos",
  "age": 12
}]
```
##### User Class
```
export class User {
    id: number;
    firstName: string;
    lastName: string;
    age: number;

    getName() {
        return this.firstName + " " + this.lastName;
    }

    isAdult() {
        return this.age > 36 && this.age < 60;
    }
}
```

위와 같은 User.json의 각각의 user Object를 ` plainToClass` 메서드를 활용하여 instance화가 가능하다.
```
fetch("users.json").then((users: Object[]) => {
    const realUsers : User[] = plainToClass(User, users);

});
```
### 참고
* [npm](https://www.npmjs.com/package/class-transformer)
* [github](https://github.com/typestack/class-transformer)