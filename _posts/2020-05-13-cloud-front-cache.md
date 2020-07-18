---
layout: post
title: 'Cloud Front Cahce'
author: jaewan
categories: [aws,devOps,infra]
image: assets/images/cloudfront.png
comments: false
---

# CloudFront Cache

CloudFront를 사용하는 목적 중 하나는 오리진 서버에서 직접 응답해야 하는 요청의 수를 줄이기 위한 것입니다.
모든 요청 중에 캐시에서 제공되는 요청의 비율을 `캐시 적중률`이라고 합니다. 캐싱 방식에는 쿼리 문자열, 쿠키, 헤더등 다양한 방식이 있습니다. 이곳에서는 쿼리 문자열에 대해 확인합니다.

## Allowed HTTP Methods
CloudFront에서 origin에 전달하기 위한 http 메소드 지정

* GET, HEAD
  * Get과 Head만 수용
* GET, HEAD, OPTIONS
  * get,head와 origin에서 별도로 제공하는 option을 포함
* GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
  * 위에 나와있는 모든 메소드를 사용가능

## Cached HTTP Methods
* None (improves caching)
  * 해더값을 기준으로 캐시를 사용하지 않음
* Whitelist
  * 특정 헤더값을 기준으로 캐시를 진행함. 특정 해더를 whilteList에 입력하면 됨
* All
  * 캐싱작업을 진행하지 않고, 모든 요청을 origin에 전달함.


## 쿼리 문자열 파라미터 기반의 콘텐츠 캐싱
`http://d111111abcdef8.cloudfront.net/images/image.jpg?color=red&size=large` 와 같이 쿼리 문자열 파라미터를 기준으로 캐싱이 가능합니다. 쿼리 문자열 기반 캐싱설정은 [여기](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/QueryStringParameters.html)를 참고하세요.
* None (Improves Caching)
  * 쿼리 문자열과 관계없이 캐시를 진행함
* Forward all, cache based on whitelist
  * 특정 파라미터만 기준으로 캐시를 진행함. whiteList에 작성 가능
* Forward all, cache based on all
  *  모든 쿼리를 기준으로 다른 버전의 객체를 만들어 캐싱을 진행함
  * 대/소문자 구분
    * `?color=Red` 와 `?color=red` 는 따로 origin에서 처리됨.
  * 파라미터 순서 구분
    * `?color=red&size=large` 과 `?size=large&color=red`는 따로 Origin에서 처리됨.




## CloudFront Cache 여부 확인

#### X-Cache
Response의 Header 부분에 ```X-Cache```파라미터가 존재합니다.
해당부분의 값에 따라 CloudFront에서 Cache를 사용했는지 확인할 수 있다.

* Cache 사용
  * "Hit from cloudfront"
  * "RefreshHit from cloudfront"
* Cache 미사용 (origin 처리)
  * "Miss from cloudfront"

* 오류 응답
  * "Error from cloudfront"
  * origin의 오류 응답에 대한 캐싱
  * 오류 응답에 대한 자세한 내용은 [여기](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/HTTPStatusCodes.html)를 확인

캐싱에 대한 자세한 내용은 [여기](https://aws.amazon.com/ko/premiumsupport/knowledge-center/cloudfront-custom-object-caching/)를 확인


## 주의사항
* 기존의 Cf를 사용하다가, 특정 behavior를 추가한경우, precedence를 확인해야한다. 만일 /* 과 같은 전역형 path가 있을 경우, 상단에 있는 behavior가 해당 path를 가져가서 처리한다.
* /*과 같이 전역형 path가 이미 존재할때, behavior를 추가한경우, invalidation을 진행하여 해당 path의 캐시 오브젝트를 삭제해 준다.
* cloud front는 behavior가 총 20개까지 가능하다. behavior에 대한 관리가 필수다!