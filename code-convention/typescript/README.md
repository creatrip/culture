# Typescript Code Convention
가독성 있는 Typescript code를 작성할 수 있는 가장 합리적인 convention을 목표로 합니다.

## Table of Contents
1. [Blocks](#Blocks)

## Blocks
- 1.1 Use braces with all multiline blocks. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

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

// bad
function foo(): boolean { return false; }

// good
function bar(): boolean {
  return false;
}
```

- 1.2 If you're using multiline blocks with `if` and `else`, put `else` on the same line as your `if` block's closing brace. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)
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

- 1.3 If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)
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
function dogs(x): y | z | void {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```

**[⬆ back to top](#table-of-contents)**
