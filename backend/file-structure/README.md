# backend file naming convention

### 기본 원칙

1.1 파일명은 domain 단위로 . 을 붙입니다.
```
member.service.ts
member.module.ts

member.service.mock.ts
```

1.2 테스트 파일은 {domain}.{test-file}.spec.ts로 명명합니다.
```
member.service.spec.ts
member.resolver.spec.ts
```

1.3 테스트를 위한 mock 관련 파일들은 {domain}.mock으로 시작합니다.
```
member.mock.service.ts
member.mock.module.ts
```