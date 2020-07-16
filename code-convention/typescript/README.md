# Typescript Code Convention
가독성 있는 Typescript code를 작성할 수 있는 가장 합리적인 convention을 목표로 합니다.

## Table of Contents
1. [Blocks](#Blocks)

## Blocks
- 1.1 멀티라인 블록에서는 항상 중괄호를 사용합니다. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

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
