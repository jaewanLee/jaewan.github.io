---
layout: post
title: 'typeorm-querybuilder'
author: jaewan
categories: [typescript,database,typeorm]
image: assets/images/typeorm.png
comments: false
---

# TypeORM

typeOrm을 사용하며, Query를 하는 방식은 쿼리 상황이나 개인 성향에 따라 다양하게 사용이 가능하다. 이곳에서는 가장 대표적으로 사용되는 findOption과 QueryBuilder에 대해 알아본다.

## findOption
가장 깔끔한 형태이다. typeorm에 내장되어 있는 findOption을 사용하여 원하는 쿼리를 만들고 그 결과값을 entity 모델 형식으로 받을 수 있다

```
 this.find({
			where: {
				id: 6,
				name: "Timber",
				url: IsNull(),
				agentData: Not(IsNull())
			},
			order: { Id: 'DESC' },
		})
```

* find / findOne
  * find 옵션중 findone의 경우 하나의 엔티티만 return하게 되지만 where절에 primary key를 사용하지 않을경우, 실제 쿼리 내용을 살펴보면 전체 쿼리를 가져온뒤, 이중 하나만 return하는 방식이다. 따라서 primary key를 사용하지 않는 쿼리의 경우, findOne이 아닌, find를 사용하고 limit(findOptionsd에서는 take 파라미터를 사용한다.)를 추가하는 방식이 효율 적이다.


자세한 옵션들은 [이곳](https://github.com/typeorm/typeorm/blob/master/docs/find-options.md) 을 참고하여 확인할 수 있다.

## QueryBuilder
쿼리빌더는 find에 비해 복잡한 쿼리는 만들 수 있도록 도와준다.
```
	this.createQueryBuilder('campaign_offer')
		.leftJoinAndSelect('campaign_offer.campaign', 'campaign')
		.where(`campaign_offer.id = :campaignId`, { campaignId: campaignId })
		.andWhere(`campaign_offer.SMS is null`)
		.andWhere(`campaign_offer.created_at < campaign.created_at `)
		.getMany()
```
* getMany / getOne
  * 여기서 마지막 excuting 함수인 getMany()에 주목해볼만 하다. excution함수에는 excute()나 getOne()등 다양한 형태가 존재하여 상황에 맞게 사용하면되는데, getMany()와 getRawMany()처럼 의미가 비슷해보이는 함수가 존자한다. Raw형태로 그대로 결과를 return하길 원한다면 getRawXXX()를 사용하고, Entity형태로 type을 명확히 하여 return값을 받고싶다면 getXXX()형태를 사용하면된다.
자세한 내용은 [이곳](https://github.com/typeorm/typeorm/tree/master/docs) 을 확인하면 된다.