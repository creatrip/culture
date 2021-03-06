# Typescript Code Convention
가독성 있는 Typescript code를 작성할 수 있는 가장 합리적인 convention을 목표로 합니다.

## Table of Contents
1. [Blocks](#Blocks)
2. [Testing](#Testing)
3. [Export/Import](#Export/Import)

## Blocks
- 1.1 if, else, while, do-while, and for statements 와 함께 block 없이 쓰이는 single-line statements는 같은 줄에 사용합니다. single-line statements 다른 줄에 사용하려면 중괄호를 사용합니다. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

```typescript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}
```

- 1.2 함수는 항상 멀티라인으로 선언 합니다.
```typescript
// bad
function foo(): boolean { return false; }

// good
function bar(): boolean {
  return false;
}
```

- 1.3 `if`블록과 `else`블록을 사용하는 멀티라인 블록에서 `else`블록은 `if`블록을 닫는 중괄호와 같은 라인에 적어야 합니다. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)
```typescript
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

- 1.4 만약 `if`블록이 항상 `return`을 실행한다면, 차후에 오는 `else`블록은 불필요합니다. `return`을 포함하는 `if`블록 뒤에 오는 `else if`블록의 `return`은 여러개의 `if`블록으로 분리 될 수 있습니다. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)
```typescript
// bad
function foo(): x | y {
  if (x) {
    return x;
  } else {
    return y;
  }
}

// bad
function cats(): x | y | void {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

// bad
function dogs(): x | y | void {
  if (x) {
    return x;
  } else {
    if (y) {
      return y;
    }
  }
}

// bad
function dogs(x): y | z | void {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}

// good
function foo(): x | y {
  if (x) {
    return x;
  }

  return y;
}

// good
function cats(): x | y | void {
  if (x) {
    return x;
  }

  if (y) {
    return y;
  }
}

// good
function dogs(): y | z {
  if (x && z) {
    return y;
  } else {
    return z;
  }
}

// good
function dogs(): y | z {
  if (x) {
    // ...
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```

**[⬆ back to top](#table-of-contents)**

## Testing
```typescript
function foo() {
  return true;
}
```

1. 깨끗한 테스트가 따르는 다섯 가지 규칙인 `FIRST`를 따른다.
    - Fast: 테스트는 빨라야 한다. 테스트가 느리면 자주 돌릴 엄두를 못 낸다.
    - Independent: 각 테스트는 서로 의존하면 안된다. 한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안 된다.
    - Repeatable: 테스트는 어떤 환경에서도 반복 가능해야 한다. 실제 환경, QA 환경, 버스를 타고 집으로 가는 길에 사용하는 (네트워크에 연결되지 않은) 노트북 환경에서도 실행할 수 있어야 한다.
    - Self-Validating: 테스트는 bool 값으로 결과를 내야 한다. 성공 아니면 실패다. 통과 여부를 알려고 로그 파일을 읽게 만들어서는 안 된다. 통과 여부를 보려고 텍스트 파일 두 개를 수작업으로 비교하게 만들어서도 안 된다.
    - Timely: 테스트는 적시에 작성해야 한다. 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현한다. 실제 코드를 구현한 다음에 테스트 코드를 만들면 실제 코드가 테스트하기 어렵다는 사실을 발견할지도 모른다.

2. Airbnb Testing 가이드 참고.
    - 어떤 프레임워크를 사용하더라도, 테스트 코드를 작성해야 한다.
    - 순수 함수를 여러개 작성하여, 예상치 못한 일이 발생하는 것을 최소화하라.
    - [stub](https://medium.com/@SlackBeck/%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%8A%A4%ED%85%81-test-stub-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-ff9c8840c1b0) 과 [mock](https://eminentstar.github.io/2017/07/24/about-mock-test.html) 을 사용할 때에는 주의하라 - 그것들은 테스트를 불안정하게 한다.
    - Airbnb에서는 [mocha](https://www.npmjs.com/package/mocha) 와 [jest](https://www.npmjs.com/package/jest) 의 사용을 우선시 한다. [tape](https://www.npmjs.com/package/tape) 은 작은 부위, 분리된 모듈에서 가끔 사용된다.
    - 100% 테스트 커버리지는 좋은 목표이지만, 현실적으로 도달하기 힘들다.
    - 버그를 수정할 때, [회귀 테스트](https://ko.wikipedia.org/wiki/%ED%9A%8C%EA%B7%80_%ED%85%8C%EC%8A%A4%ED%8A%B8) 를 작성하라. 회귀 테스트 없이 수정된 버그는 또 다른 버그를 낳을 가능성이 높다.

3. Mocking
    - 테스트 코드는 `**/*tests*/**/*.spec.ts` 경로를 만족한다.
    - mock에 관련된 코드는 `**/*tests*/**/mocks/*.mock.ts` 경로를 만족한다.<br>
      ex) A 모델의 mock 모듈(또는 데이터)은 A/tests/mocks 하위에 존재하게 된다.
    - 테스트에 필요한 mock 데이터는 mock 모듈 또는 mock 데이터를 통해 생성된 것이어야 한다.

```typescript
// user/models/user.ts
class User {
  constructor(
    private readonly name: string, 
    private readonly birth: Date,
  ) {}

  age(): number {
    return (new Date().getFullYear() - this.birth.getFullYear()) + 1
  }  
}
```

```typescript
// user/tests/mocks/user.mock.ts
const birth: Date = new Date(1992, 3, 8);
export const user: User = new User('아무개', birth);
``` 

```typescript
// user/tests/user.spec.ts
// 유저 자체를 테스트하는 경우에는 테스트코드에서 직접 생성
describe('User', () => {
  it('[Success] should success', () => {
    const user: User = new User('아무개', new Date(1992, 3, 8));
    expect(user.age()).toBe(29);
  })
})
```

```typescript
// group/tests/group.spec.ts
// 다른 모듈의 테스트에 user가 사용되는 경우에는 mock을 통해 user를 생성
import { user } from './mocks/user.mock';

describe('Group', () => {
  it('[Success] if add one user to group then group should has only one user', () => {
    const group: Group = new Group()
    group.add(user);
    expect(group.users.length).toBe(1);
  })
})
```

**[⬆ back to top](#table-of-contents)**

## Export/Import

- 클래스를 export 하는 경우 파일명은 PascalCase 를 따른다
```typescript
// Creatrip.ts
class Creatrip {};
export default Creatrip;
```

- 여러 모듈을 export 하는 경우 export default 이름을 파일 이름으로 한다.
```tsx
// Component.tsx
import React from 'react';

export interface ComponentProps {
  // ...
}

function Component(): JSX.Element {
  // ...

  return (
    < />
  );
}

export default Component;
```

- export default 대상을 import 하는 경우라면 export 하는 대상의 이름과 import 이름을 같도록 한다.

```typescript
// bar.ts
// bad (typescript에서 require는 사용하지 않는다)
const component = require('Component');

// bad (유추가 불가능한 전혀 다른 이름으로 불러옴)
import foo from 'bar';

// bad (하나의 파일에 있는 내용들은 한번에 import 해야한다)
import { ComponentProps } from 'Component';
import Component from 'Component';

// bad (import한 대상이 어떤 동작을 하는지 

// good (export 하는 이름 그대로 import 해야한다)
import Creatrip from 'Creatrip';

// good (* as 를 사용하는 경우는 허용한다)
import * as Creatrip from 'Creatrip';

// good (import한 대상의 기능을 명확하게 나타내지 못하는 대상에 대해서는 as를 사용한다)
import { v4 as uuidv4 } from 'uuid';

// good (한번에 import)
import Component, { ComponentProps } from 'Component';
``` 

**[⬆ back to top](#table-of-contents)**
