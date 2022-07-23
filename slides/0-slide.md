# The Unsoundness of TypeScript

<!-- migrating to typescript -->

---

# What is Soundness?

When a Type System ensures that the static types is equal to the runtime values

**Unsoundness is the lack of it!**

---

# A example

Functions that either returns an error or the correct value

```ts
const findModel = async (_id: string) => {
  const model = await Model.findOne({ _id });

  if (!model) {
    return {
      model: null,
      error: 'Model not found',
    };
  }

  return {
    model,
    error: null,
  };
};
```

---

```ts
const anotherFunction = async () => {
  const _id = await getId();

  const { error, model } = await findModel(_id);

  if (error) {
    return {
      error,
    };
  }

  model;
  // ^? IModel | null

  model._id;
  // Error: Object is possibly 'null'
};
```

---

# Why?

**Interop with JavaScript**

The idea of TypeScript is not to be a new language, is to add types to JavaScript.

<!-- Not the same as ReasonML, ReScript e PureScript -->

<!-- The Seven Sources of Unsoundness in TypeScript -->

---

# Workarounds

You need to tell the compiler that you are **sure** about the type of a variable

---

## Using `as`

We can use `as` to change any type

```ts
const nonNullModel = model as IModel;

nonNullModel._id;
// No errors 🥳
```

---

## Using Non-null assertion operator

In TypeScript we can change any type with `as`

```ts
model!._id;
// No errors 🥳
```

---

## Wait to be fixed

And take the opportunity to make a cake to celebrate when it is read for review:

![image](https://i.imgur.com/W3XfSoa.png)

---

# A dilemma

At the same time that the TypeScript has a powerful type system

It struggle with some obvious type in runtime

---

# Thanks

### Feedback? Questions?

---

# References

- https://www.typescriptlang.org/docs/handbook/type-compatibility.html
- https://effectivetypescript.com/2021/05/06/unsoundness/
