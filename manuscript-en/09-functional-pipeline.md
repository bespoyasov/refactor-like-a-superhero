# Functional Pipeline

As we saw in the previous chapter, abstraction helps to divide the program by “levels of detail.” We pay attention to the most important details at each level and use the most appropriate terms.

In user applications, one of such levels describes the _business logic_—the domain processes that make the application unique and gain profit. Those, in other words, are the problems that business wants the developers to solve.

| For example 👀                                                                                                                                |
| :-------------------------------------------------------------------------------------------------------------------------------------------- |
| For an online store, the business logic would be creating orders and checking out. For transport company—route and traffic load optimization. |

Business logic speaks in the language of the domain. It describes workflows as sequences of events and consequences: “When a user inputs a discount coupon, the application checks its validity and reduces the order price.”

Such language is “far from code.” Because of the high abstraction level, translating business workflows into code can be difficult. Functional pipeline, which we'll discuss in this chapter, helps to describe business workflows in code more precisely and closer to reality.

## Data Transformations

Business workflows are data transformations. For example, applying a discount to an order can be expressed as a transition from one data state to another:

```
“Coupon Application”:

[Created Order] + [Valid Coupon] -> [Discounted Order]
```

In larger applications, transformations can be more extended, and the data can go through several steps:

```
“Selecting Product Recommendations”:

[Product Cart] + [Shopping History] ->
  [Product Categories] + [Recommendation Weights] ->
  [Recommendation List]
```

In poorly organized code, business workflows don't resemble such chains. They are complicated, non-obvious, and often don't speak the language of the domain. As a result, instead of a clear workflow description, we end up with something like:

```
“Selecting Product Recommendations”:

[Product Cart] + ... + [Magic 🔮] -> [Recommendation List]
```

In well-organized code, the workflows look linear, and the data in them go through several steps one at a time. The final state of the data is the workflow's desired result.

<figure>
  <img src="../images/09-functional-pipeline.png" width="800" alt="A linear chain of labeled rectangles connected by arrows from left to right">
  <figcaption><em>Data goes through a chain of different states. The output is the desired workflow result</em><br><br></figcaption>
</figure>

This kind of code organization is called a _functional pipeline_. When refactoring business logic, we can focus on it to make the workflows in the code clearer and more apparent.

## Data States

In “Domain Modeling Made Functional,” Scott Wlaschin describes how to design a program based on business workflows and their data.[^dmmf] The suggestion is to represent the workflow steps as separate functions. We can use this idea as a basis for refactoring.

To do this, we first need to highlight all the stages that data passes through. These stages will help us understand how to divide the workflow and what should consider in the code.

Let's analyze this approach with the example of an online store. Suppose the `makeOrder` function compiles an order, applies discount coupons, and adds promo products:

```js
function makeOrder(user, products, coupon) {
  if (!user || !products.length) throw new InvalidOrderDataError();
  const data = {
    createdAt: Date.now(),
    products,
    total: totalPrice(products),
    discount: selectDiscount(data, coupon),
  };

  if (!selectDiscount(data, coupon)) data.discount = 0;
  if (data.total >= 2000 && isPromoParticipant(user)) {
    data.products.push(FREE_PRODUCT_OF_THE_DAY);
  }

  data.id = generateId();
  data.user = user.id;
  return data;
}
```

The function isn't big, but it does quite a lot:

- It validates the input data;
- Creates an order object;
- Applies a discount from the passed coupon;
- Adds promo products under certain conditions.

At a glance, it's hard to distinguish each of these actions in the function code. There are so many details that it's difficult to see the individual workflow steps.

| Not only that 💊                                                                                                                                                                                                                       |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The order object changes within the function chaotically. And although formally, its encapsulation isn't broken—the object is created and changed in the same function—it feels like `makeOrder` is “trying to do someone else's job.” |

Let's highlight the workflow steps and data states that appear in them:

```
“Show order in UI”:

- “Validate Input Data”:
  [Raw Unvalidated Input] -> [Validated User] + [Validated Product List]

- “Create Order”:
  [User] + [Product List] -> [Created Order]

- “Apply Discount Coupon”:
  [Order] + [Coupon] -> [Discounted Order]

- “Apply Promo”:
  [Order] + [User] -> [Order with Promo Products]
```

We aim to make the function code look like this list during refactoring. We can start by grouping the code into “sections,” each of which will represent a different workflow step:

```js
function makeOrder(user, products, coupon) {
  // Validate Input:
  if (!user || !products.length) throw new InvalidOrderDataError();

  // Create Order:
  const data = {
    createdAt: Date.now(),
    products,
    total: totalPrice(products),
  };
  data.id = generateId();
  data.user = user.id;

  // Apply Discount:
  const discount = selectDiscount(data, coupon);
  data.discount = discount ?? 0;

  // Apply Promos:
  if (data.total >= 2000 && isPromoParticipant(user)) {
    data.products.push(FREE_PRODUCT_OF_THE_DAY);
  }

  return data;
}
```

Grouping the steps will help find abstraction problems in the code: if we can think of a meaningful name for a step, we can probably extract its code into a function. In the example above, the comments with the step names fully reflect their intent. Let's extract the steps into separate functions:

```js
// Create Order:
function createOrder(user, products) {
  return {
    id: generateId(),
    createdAt: Date.now(),
    user: user.id,

    products,
    total: totalPrice(products),
  };
}

// Apply Discount:
function applyCoupon(order, coupon) {
  const discount = selectDiscount(order, coupon) ?? 0;
  return { ...order, discount };
}

// Apply Promos:
function applyPromo(order, user) {
  if (!isPromoParticipant(user) || order.total < 2000) return order;

  const products = [...order.products, FREE_PRODUCT_OF_THE_DAY];
  return { ...order, products };
}
```

The `makeOrder` function then would look like this:

```js
function makeOrder(user, products, coupon) {
  if (!user || !products.length) throw new InvalidOrderDataError();

  const created = createOrder(user, products);
  const withDiscount = applyCoupon(created, coupon);
  const order = applyPromo(withDiscount, user);

  return order;
}
```

After the changes, the workflow steps are encapsulated in separate functions. These functions only change the order object, keeping it valid. The `makeOrder` function doesn't change data uncontrollably anymore but only calls those functions. It makes invalid orders less likely and the testing of data transformations easier.

The code of `makeOrder` now resembles the list of workflow steps we've started with. The details of each step are hidden behind the name of the corresponding function. The name describes the entire step, which makes the code easier to read.

Also, when adding a new step to the workflow, we now only have to insert a new function in the right place. The other conversions remain unchanged:

```js
function makeOrder(user, products, coupon, shipDate) {
  if (!user || !products.length) throw new InvalidOrderDataError();

  const created = createOrder(user, products);
  const withDiscount = applyCoupon(created, coupon);
  const withPromo = applyPromo(withDiscount, user);
  const order = addShipment(withPromo, shipDate); // New workflow step.

  return order;
}
```

And when we remove a step, it's easier to find the function to delete—deleting a function call guarantees removing all the code related to that step in the process.

| By the way 🕵️                                                                                                                                                                                                                     |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| It's not always easy to spot a pipeline. Business logic can be “scattered” across the codebase. In such cases, it can be helpful to draw a diagram of how the application parts communicate with each other to discover patterns. |

## Unrepresentable Invalid States

Some business workflows only need data in certain states to work. For example, we don't want to ship an order until it's paid or if it misses the delivery address. Such orders are invalid for this workflow.

We can make the code more reliable if we “forbid” the passing of invalid data. To do that, we can design the code in such a way that makes it harder or impossible to pass invalid data.

For example, we can do this using types in languages with static typing. We can describe each data state as a separate type and specify what types are valid for a workflow. _This adds domain constraints directly to the signature_ of functions and methods.

| However 👀                                                                                                                                                                                                                                                                                                                   |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Clearly, we have to remember about constraints of a particular language. For example, in TypeScript, it's harder to achieve “validation in signature,” and it still will be limited by the JS-runtime. But even without the “true validation,” this technique helps to reflect more domain knowledge directly into the code. |

For example, let's look at the `CustomerEmail` type, which describes the email address of the store's user:

```ts
type CustomerEmail = {
  value: EmailAddress;
  verified: boolean;
};
```

The type has a `verified` flag that shows if the email is verified. The problem with the flag is that it doesn't explain under _what conditions_ it'll be `true`. There's not enough knowledge in type about email verification.

---

**❗️ The code has to compensate for this lack of knowledge somehow. Most often, with extra data checks at runtime**

---

For example, imagine a link in the store's UI to restore the user account. When clicked, it should send the user to the “Reset Password” page, but only if their email is verified:

```ts
function restoreAccount(email: CustomerEmail): void {
  if (email.verified) {
    // Send the user to the “Reset Password” page.
  } else {
    return;
  }
}
```

With the current `CustomerEmail` implementation, the `restoreAccount` function accepts invalid data half the time.

It can be fine if the type contains only one such flag. But the more flags there are, the more different states the type has simultaneously and the more probable the errors due to the inconsistent data.

We can solve this by separating different data states into different types:

```ts
// For unverified emails, we use one type:
type UnverifiedEmail = {
  /*...*/
};

// ...And for verified emails, we use another:
type VerifiedEmail = {
  /*...*/
};

// The “any email” can be used
// when the verification isn't important:
type CustomerEmail = UnverifiedEmail | VerifiedEmail;
```

Then for different workflows, we can require different data types:

```ts
// If the function cares about the email verification,
// it can require a specific type in its signature:

function restorePassword(email: VerifiedEmail): void {}
function verifyEmail(email: UnverifiedEmail): void {}

// If the function can handle any email, it can use the common type.
// This way, we can see the requirements for email verification
// right in the function signature:

function isValidEmail(email: CustomerEmail): boolean {}
```

Now, the function signatures are more accurate in describing the domain because they convey more knowledge about it. The functions `restorePassword` and `verifyEmail` warn about their requirements and constraints. The function `isValidEmail` says that it's ready to handle any email, and verification isn't essential.

| However 🚧                                                                                                                                                |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------- |
| In the case of TypeScript, type aliases may not be enough. We may want to ensure that we can't create a non-verified email with the `VerifiedEmail` type. |
| For this, we can use type branding[^typebranding] or agree to create entities only with special classes or factories.                                     |
| However, for descriptive purposes—transferring knowledge about the domain—aliases may suffice just fine.                                                  |

## Data Validation

The functional pipeline relies on linear code execution. Steps within a workflow execute one after the other and pass data down through the chain of transformations.

For this idea to work, data within the workflow must be safe and not break the pipeline. However, we can't guarantee that any “external” data is safe. So in the code, we want to separate zones where data can be trusted and where it can't.

| Islands of security 🏝                                                                     |
| :---------------------------------------------------------------------------------------- |
| Business workflows ideally should become “islands” where the data is verified and secure. |

DDD has an analog for such islands—_bounded contexts_.[^boundedcontext][^ddd][^dmmf] Simply put, a bounded context is a set of functions that refer to some part of an application.

According to DDD, validating data is more convenient _at the boundaries_ of contexts, for example, at the context input. In this case, “inside” the context, we don't need additional checks because the data is already validated and safe.

<figure>
  <img src="../images/09-bounded-context.png" width="800" alt="A rectangle with ingoing and outgoing arrows and thick boundaries; inside the rectangle, there's a linear chain of smaller rectangles">
  <figcaption><em>All validation occurs at the boundaries; the data inside the context is considered valid and safe</em><br><br></figcaption>
</figure>

We can use this rule in our code to get rid of unnecessary data checks at runtime. By validating the data at the beginning of the workflow, we can assume later that it meets our requirements.

Then, for example, in the `CartProducts` component, instead of ad-hoc checks for the existence of products and their properties inside the render function:

```js
function CartProducts({ items }) {
  return (
    !!items && (
      <ul>
        {items.map((item) =>
          item ? <li key={item.id}>{item.name ?? "—"}</li> : null
        )}
      </ul>
    )
  );
}
```

...We would check the data once at the beginning of the workflow:

```js
function validateCart(cart) {
  if (!exists(cart)) return [];
  if (hasInvalidItem(cart)) return [];

  return cart;
}

// ...

const validCart = validateCart(serverCart);
```

...And later would use it without any additional checks:

```js
function CartProducts({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### Missing States

Often input validation helps us discover data states we did not notice earlier. For example, the code of the `CartProducts` component in the previous snippet has become simpler, and the flaws in it have become easier to spot:

```js
// If we render a valid but empty cart,
// the component will render an empty list:

const validEmptyCart = [];
<CartProducts items={validEmptyCart} />;

// Results in <ul></ul>
```

The “Empty cart” state is valid but represents an edge case. Together with input validation, the functional pipeline makes such cases more noticeable because they fall out of “regular” code execution. And the more prominent the edge cases are, the sooner we can detect and handle them:

```js
// To fix the problem with the empty list, we can split
// the “Empty cart” and “Cart with products” states
// into different components:

const EmptyCart = () => <p>The cart is empty</p>;
const CartProducts = ({ items }) => {};

// Then, when rendering, we can first handle all the edge cases,
// and then proceed to Happy Path:

function Cart({ serverCart }) {
  const cart = validateCart(serverCart);

  if (isEmpty(cart)) return <EmptyCart />;
  return <CartProducts items={cart} />;
}
```

| However 💡                                                                                                                                                                                                                                 |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We remember that refactoring must not change the code functionality, so it's better to fix bugs separately. In one of the last chapters, we will discuss how to address problems found in the code but not mix refactoring with bug fixes. |

Such edge case handling, as in the `Cart` component, helps us detect more potential edge cases in the earlier stages of development. Considering these edge cases makes the program more reliable and accurate in describing the business workflows.

| By the way 👀                                                                                                                                                                               |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Handling edge cases before working with Happy Path might remind you of the technic called “Early return.” We will discuss it more closely in the chapter on conditions and code complexity. |

### DTO and Deserialization

Validation at the beginning is also useful if the data can get corrupted by serialization or deserialization.[^serialization]

Typically, information between parts of a system is transferred as a _Data Transfer Object, DTO_.[^dto][^ddd] These are “packages of information” that travel from one part of an application to another—from server to client, for example.

The structure and format of DTOs are intentionally simple: only strings, numbers, booleans, arrays, and objects. For example, JSON, which is often used to communicate between the server and the client, has no complex types or structures.

During “translation” between complex domain types and intentionally simple DTOs, something can go wrong, and the data can become invalid. This data can break the workflow if not checked before use.

| In detail 📚                                                                                                                                                                                                  |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| You can read more about the functional pipeline, business workflows, bonded contexts, data validation, and DDD in “Domain Modeling Made Functional” by Scott Wlaschin.[^dmmf] Great book, highly recommended. |

## Data Mapping and Selectors

We might need the same data for different purposes. For example, the UI may render a shopping cart differently depending on the user's settings.

The functional pipeline suggests “preparing” data for such situations in advance. For example, we might want to select the required fragments from the original data in advance, transform some data sets into others or even merge several data sets into one.

| Beware the simplification 🚧                                                                                                                                                     |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| From the description above, you might remember some terms: mapping, projection, slice, lens, and mapping.[^mappers][^projections][^slices][^lenses]                              |
| I decided not to use them in this book to avoid introducing too many new concepts. Instead, I will use the word “selector” in the text as a general synonym for all these terms. |

Data selectors help to decouple modules that use similar but slightly different data. For example, let's look at the `CartProducts` component, which renders a shopping cart:

```js
function CartProducts({ serverCart }) {
  return (
    <ul>
      {serverCart.map((item) => (
        <li key={item.id}>
          {item.product.name}: {item.product.price} × {item.count}
        </li>
      ))}
    </ul>
  );
}
```

Right now, it relies on the cart data structure, which comes from the server. If the structure changes, we'll have to change the component too:

```js
// If products start to arrive separately, we'll have to search
// for a particular product during the rendering.

function CartProducts({ serverCart, serverProducts }) {
  return (
    <ul>
      {serverCart.map((item) => {
        const product = serverProducts.find(
          (product) => item.productId === product.id
        ).name;

        return (
          <li key={item.id}>
            {product.name}:{product.price} × {item.count}
          </li>
        );
      })}
    </ul>
  );
}
```

Data selectors can help us decouple the server response and the data structure we use for rendering. For example, we can represent such a selector as a function:

```js
// The `toClientCart` function “converts” the data into a structure,
// which the application components will use.

function toClientCart(cart, products) {
  return cart.map(({ productId, ...item }) => {
    const product = products.find(({ id }) => productId === id);
    return { ...item, product };
  });
}
```

Then, we will convert the data using this function before rendering the component:

```js
const serverCart = await fetchCart(userId)
const cart = toClientCart(serverCart, serverProducts)

// ...

<CartProducts items={cart} />
```

The component then will rely on the data structure that is defined _by us_, not a third-party:

```js
function CartProducts({ items }) {
  return (
    <ul>
      {items.map(({ id, count, product }) => (
        <li key={id}>
          {product.name} {product.price} × {count}
        </li>
      ))}
    </ul>
  );
}
```

So if the API response changes, we won't need to update all the components that use that data. We would only need to update the selector function.

| By the way 🔌                                                                                                                                                                     |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| This is especially useful if the server often breaks backward compatibility with client code. This technique can be considered a special case of the “Adapter” pattern.[^adapter] |

It's also convenient to use selectors if the UI has different representations for the same data. For example, in addition to the cart, the store can have a list of all available products with some of them marked as “In Cart”.

To render such a list, we can reuse already existing data but “prepare” them in a slightly different way:

```js
// We use the existing server data,
// but create a different structure:

function toClientShowcase(products, cart) {
  return products.map((product) => ({
    ...product,
    inCart: cart.some(({ productId }) => productId === product.id),
  }));
}
```

The `Showcase` component doesn't need to know anything about the server response. It only works with the selector's result:

```js
function Showcase({ items }) {
  return (
    <ul>
      {items.map(({ product, inCart }) => {
        <li key={product.id}>
          {product.name} <input type="checkbox" checked={inCart} disabled />
        </li>;
      })}
    </ul>
  );
}
```

This approach helps to separate the responsibility between the code: components only care about the render, and selectors “convert” the data.

Components become less coupled to the rest of the code because they rely on structures we fully control. It makes it easier, for example, to replace one component with another if we need to update the UI.

Testing data transformations becomes easier because any selector is a regular function. We don't need a complex infrastructure to test it. Testing transformations within a component, for example, would require its rendering.

We have more control over the data we want (or don't want) to include in the selector's result. We can keep only the fields that a particular component uses and filter out everything else.

It's usually best to apply selection right after the data is validated. At that point, we _already know_ that the data is safe, but _don't rely_ on its structure anywhere in the code yet. This gives us the ability to convert the data for a particular task.

| Note 🔗                                                                                                                                                                                   |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The data in this example goes through the chain of states: “Raw Server Data” → “Valid Data” → “Prepared for a Task” → “Displayed in the UI.”                                              |
| Functional pipeline helps to describe _any task_ as a similar chain. It makes the decomposition easier because chains help build a clear mental model of the workflow we express in code. |

[^dmmf]: “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
[^typebranding]: “Branding and Type-Tagging” by Kevin B. Greene, https://medium.com/@KevinBGreene/surviving-the-typescript-ecosystem-branding-and-type-tagging-6cf6e516523d
[^boundedcontext]: “Bounded Context in DDD” by Martin Fowler, https://www.martinfowler.com/bliki/BoundedContext.html
[^dto]: Data Transfer Object, DTO, Wikipedia, https://en.wikipedia.org/wiki/Data_transfer_object
[^ddd]: “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
[^projections]: Projection operations, C# Guide, https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/projection-operations
[^mappers]: “Data Mapper” by Martin Fowler, https://martinfowler.com/eaaCatalog/dataMapper.html
[^slices]: API Slice Overview, Redux Toolkit Docs, https://redux-toolkit.js.org/rtk-query/api/created-api/overview#api-slice-overview
[^lenses]: “Lenses in Functional Programming” by Albert Steckermeier, https://sinusoid.es/misc/lager/lenses.pdf
[^adapter]: Adapter Pattern, Refactoring Guru, https://refactoring.guru/design-patterns/adapter
[^serialization]: Serialization, Wikipedia, https://en.wikipedia.org/wiki/Serialization
