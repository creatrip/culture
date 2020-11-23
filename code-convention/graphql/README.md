# Typescript Code Convention
가독성 있는 graphql 구조를 작성할 수 있는 가장 합리적인 convention을 목표로 합니다.

## Table of Contents
1. [Relation Mutation 정의](#Relation-Mutation-정의)
2. [DTO 정의](#DTO-정의)


## Relation Mutation 정의
- 1.1 M:N 관계에서의 추가, 삭제 뮤테이션
```
추가 -> linkAandB
삭제 -> unlinkAandB
``` 

- 1.2 1:N 관계에서의 추가 삭제 뮤테이션
```
추가 -> addAToB
삭제 -> removeAFromB 
```

- 1.3 일반 도메인 추가 삭제 뮤테이션
```
추가 -> createA
삭제 -> deleteA
```

**[⬆ back to top](#table-of-contents)**

## DTO 정의
- 2.1 도메인에 대한 DTO를 만든다고 가정했을때 아래와 같은 규칙을 따른다.
```
EX)도메인을 Merchandise라고 가정했을때
CreateMerchandiseArgs{
 data: CreateMerchandiseInput
}

CreateMerchandiseInput {
   name: string
   language: LagnuageType
}
```

- 2.2 여러개를 생성하는 DTO는 아래의 규칙을 따른다.
```
EX)도메인을 Tag라고 가정했을때
BulkCreateTagArgs {
    translation:CreateTagTranslationInput[]
}

CreateTagTranslationInput {
    name:string
    language:LanguageType
}
``` 

**[⬆ back to top](#table-of-contents)**
