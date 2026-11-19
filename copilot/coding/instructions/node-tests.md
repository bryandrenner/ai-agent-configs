---
description: 'Guidelines for writing automated tests'
applyTo: '**/*.test.ts,**/*.spec.ts'
---

# Guidelines for writing automated tests

## Unit and component tests with Jest, Vitest, or similar

Generally name tests as though they begin with "should...", and follow the pattern of "do X when Y". Example: "return Decline when the weather is bad".

Use `describe` blocks to group tests. For example, for a validation schema, use `describe` to group tests by property, with `accepts A` and `rejects B` test cases inside.

If inputs and expected outputs are simple, just hard-code them in each test. Don't over-DRY test implementation.

If the unit under test has complex inputs with minor variations, resulting in minor variations of the output, use this pattern:

1. Add a type to the top of the file representing the test case (inputs and expected output).

  ```ts
  interface TestCase {
    Kitchen inputKitchen
    Chef inputChef
    Menu inputMenu
    WaiterRecommendation[] expectedRecommendations
  }
  ```

2. Add a helper function at the bottom of thie file called `happyPathTestCase` that returns the complex inputs and expected output for the happy path.

   Example:
   ```ts
   function happyPathTestCase(): TestCase {
     return {
       inputKitchen: {
         // ...complex properties...
       },
       inputChef: {
         // ...complex properties...
       },
       inputMenu: {
         // ...complex properties...
       },
       expectedRecommendations: [
         // ...complex properties...
       ],
     }
   }
   ```

3. Call `happyPathTestCase()` at the top of each test function, and make each test minimally modify the inputs and outputs to represent the behavioral variation.
   
  Example:
  ```ts
  test('recommends simple menu items when the chef has fewer than 2 years\' experience', () => {
    const { inputKitchen, inputChef, inputMenu, expectedRecommendations }
      = happyPathTestCase()
    inputChef.experience.years = 1
    expectedRecommendations = expectedRecommendations.filter((r) => r.complexity > 3)

    const recommendations = makeRecommendations(inputKitchen, inputChef, inputMenu)

    assert(recommendations).toStrictEqual(expectedRecommendations)
  })
  ```

4. Add a `happy path` test as the first test, that simply feeds the unit test inputs and asserts it against the expected output.
