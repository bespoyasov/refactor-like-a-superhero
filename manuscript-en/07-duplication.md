# Code Duplication

The main goal of refactoring is to make the code more readable. One way to achieve this is to reduce the amount of noise in it.

Code duplication makes the code noisy because it doesn't contain any useful information. However, not all duplication is evil, and it even can be a development tool. So when refactoring, we should understand what kind of duplication we're dealing with.

In this chapter, we'll discuss what to look for when searching for duplication and how to know when it's time to get rid of it.

## Not All Duplication is Evil

If two pieces of code have the same purpose, contain the same set of actions, and process the same data, that is _direct_ duplication. We can safely get rid of it, for example, by extracting the repeating code into a variable, function, or module.

But there are cases when two pieces of code seem “similar”, but later turn out to be different. If we merge them too early, it'll be harder to split such code later. In general, it's much easier to merge identical code than to split modules merged earlier.

When we're not sure that we're facing two _really_ identical pieces of code, we can mark these places with special labels. In these labels, we can write an assumption about what's duplicated there.

```js
/** @duplicate Applies a discount coupon to the order. */
function applyCoupon(order, coupon) {}

/** @duplicate Applies a discount coupon to the order. */
function applyDiscount(order, discount) {}
```

Such labels will only be useful if we conduct regular audits. During the audits, we should check if we have learned something new about possible duplicates which either confirms that they're identical or disproves it by showing the difference between them.

| By the way ⏰                                                                                                                                                                                                                                                                                                           |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| I consider regular audits in my projects as part of paying the technical debt. I make to-do lists for such periodic tasks. Within the list, I specify what needs to be done within the task. The technique of regular audits and its benefits are well described in “Jedi Techniques” by Maxim Dorofeev.[^jeditechnics] |

If during an audit of a label it becomes clear that the duplication described in it is _direct_, we can proceed with refactoring code with that label. If the code turned out to be different, we can just remove the label. This approach helps not to rush with code generalization, but at the same time not to forget places where possible duplication exists.

## Variables for Data

Duplicated data or calculation results are convenient to put into variables. This helps, for example, with unraveling complex conditions or highlighting the steps of data transformations.

```js
// If conditions are close to each other:

if (user.age < 18) toggleParentControl();
// ...
if (user.age < 18) askParents();

// ...We can extract the expression into a variable:

const isChild = user.age < 18;

if (isChild) toggleParentControl();
// ...
if (isChild) askParents();
```

Sometimes the data duplication can be less obvious and more difficult to spot:

```js
// The second condition is turned “inside out”
// and uses the `years` variable instead of the object field.

if (user.age < 18) askParents();
// ...
const { age: years } = user;
if (years >= 18) askDocuments();

// But we can still get rid of it,
// by extracting the expression into a variable:

const isChild = user.age < 18;

if (isChild) askParents();
// ...
if (!isChild) askDocuments();
```

| More info 🔬                                                                                                    |
| :-------------------------------------------------------------------------------------------------------------- |
| We'll talk about simplifying and unraveling complex conditions in more detail in one of the following chapters. |

## Functions for Actions

Duplicated actions and data transformations can be extracted into functions and methods. To detect them, we can use the “sameness check”:

- Actions are duplicated if they have the same goal—the desired result;
- Have the same scope—the part of the application they affect;
- Have the same direct input—arguments and parameters;
- Have the same indirect input—dependencies and imported modules.

In the example below, the code snippets pass this test:

```js
// - Goal: to add a field with the absolute discount value to the order;
// - Scope: the order object;
// - Direct input: order object, relative discount value;
// - Indirect input: function that converts percents to absolute value.

// a)
const fromPercent = (amount, percent) => (amount * percent) / 100;

const order = {};
order.discount = fromPercent(order.total, 50);

// b)
const order = {};
const discount = (order.total * percent) / 100;
const discounted = { ...order, discount };

// Actions in “a” and “b” are the same,
// so we can extract them into a function:

function applyDiscount(order, percent) {
  const discount = (order.total * percent) / 100;
  return { ...order, discount };
}
```

In the other example, the goal and direct input are the same, but the dependencies are different:

```js
// The first snippet calculates discount in percent,
// the second one applies the “discount of the day”
// by using the `todayDiscount` function.

// a)
const order = {};
const discount = (order.total * percent) / 100;
const discounted = { ...order, discount };

// b)
const todayDiscount = () => {
  // ...Match the discount to today's date.
};

const order = {};
const discount = todayDiscount();
const discounted = { ...order, discount };
```

In the example above, fragment “b” has the `todayDiscount` function among its dependencies. Because of it, the action sets differ enough to be considered “similar” but not “the same”.

We can use `@duplicate` labels and wait a bit to get more information about how they should work. When we know exactly how these functions should work, we can “generalize” the actions:

```js
// The generalized `applyDiscount` function will take
// the absolute discount value:

function applyDiscount(order, discount) {
  return {...order, discount}
}

// Differences in the calculation (percents, “discount of the day”, etc.)
// are collected as a separate set of functions:

const discountOptions = {
  percent: (order, percent)  => order.total * percent / 100
  daily: daysDiscount()
}

// As a result, we get a generalized action for applying a discount
// and a dictionary with discounts of different kinds.
// Then the application of any discount will now become uniform:

const a = applyDiscount(order, discountOptions.daily)
const b = applyDiscount(order, discountOptions.percent(order, 40))
```

| More info 🔬                                                                                                |
| :---------------------------------------------------------------------------------------------------------- |
| We will talk in detail about generalized algorithms, their use, and parameterization in a separate chapter. |

[^jeditechnics]: “Jedi Technics” by Maxim Dorofeev, Translated summary, https://bespoyasov.me/blog/jedi-technics/
