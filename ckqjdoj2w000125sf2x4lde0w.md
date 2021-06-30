## Using the Destructuring Assignment Syntax in JavaScript

The destructuring assignment syntax is a JavaScript syntax feature that was introduced in the 2015 version of JavaScript and lets us unpack a list of values of an array or key-value pairs of an object into individual variables.

It’s very handy for retrieving entries from arrays or objects and setting them as values of individual variables. This is very handy because the alternative was to get an entry from an array from an index and then setting them as values of variables for arrays.

For objects, we have the value from the key and set them as values of variables.

### Array Destructuring

We can use the destructuring assignment syntax easily in our code. For arrays, we can write:

    const [a,b] = [1,2];
    

Then, we get 1 as the value of `a` and 2 as the value of `b` because the destructing syntax unpacked the entries of an array into individual variables.

Note that the number of items in the array does not have to equal the number of variables. For example, we can write:

    const [a,b] = [1,2,3]
    

Then `a` is still 1 and `b` is still 2 because the syntax only sets the variables that are listed in the same order as the numbers appeared in the array. So, 1 is set to `a`, 2 is set to `b`, and 3 is ignored.

We can also use the rest operator to get the remaining variables that weren’t set to variables. For example, we can have:

    const [a,b,...rest] = [1,2,3,4,5,6]
    

Then, `rest` would be `[3,4,5,6]` while we have `a` set to 1 and `b` set to 2. This lets us get the remaining array entries into a variable without setting them all to their own variables.

We can use the destructuring assignment syntax for objects as well. For example, we can write:

    const {a,b} = {a:1, b:2};
    

In the code above, `a` is set to 1 and `b` is set to 2 as the key is matched to the name of the variable when assigning the values to variables.

Because we have `a` as the key and 1 as the corresponding value, the variable `a` is set to 1 as the key name matches the variable name. It is the same with `b`. We have a key named `b` with a value of 2, because we have the variable named `b`, we can set `b` to 2.

We can declare variables before assigning them with values with the destructuring assignment syntax. For example, we can write:

    let a, b;
    ([a, b] = [1, 2]);
    

Then, we have `a` set to 1 and `b` set to 2 because `a` and `b` that were declared are the same ones that are assigned.

As long as the variable names are the same, the JavaScript interpreter is smart enough to do the assignment regardless of whether they’re declared beforehand or not.

We need the parentheses on the line so that the assignment will be interpreted as one line and not individual blocks with an equal sign in between, because two blocks on the same line aren’t valid syntax.

This is only required when the variable declarations happen before the destructuring assignment is made.

We can also set default values for destructuring assignments. For instance:

    let a,b;
    ([a=1,b=2] = [0])
    

This is valid syntax. In the code above, we get that `a` is 0 because we assigned 0 to it. `b` is 2 because we didn’t assign anything to it.

The destructuring assignment syntax can also be used for swapping variables, so we can write:

    let a = 1;
    let b = 2;
    ([a,b] = [b,a])
    

`b` would become 1 and `a` would become 2 after the last line of the code above. We no longer have to assign things to temporary variables to swap them, and we also don’t have to add or subtract things to assign variables.

The destructuring assignment syntax also works for assigning returned values of a function to variables.

So, if a function returns an array or object, we can assign them to variables with the destructuring assignment syntax. For example, if we have:

    const fn = () =>[1,2]
    

We can write:

    const [a,b] = fn();
    

To get 1 as `a` and 2 as `b` with the destructuring syntax because the returned array is assigned to variables with the syntax.

Similarly, for objects, we can write:

    const fn = () => {a:1, b:2}
    const {a,b} = fn();
    

We can ignore variables in the middle by skipping the variable name in the middle of the destructuring assignment. For example, we can write:

    const fn = () => [1,2,3];
    let [a,,b] = fn();
    

We get `a` with the value of 1 and `b` with the value of 3, skipping the middle value.

It’s important to know that if we use the rest operator with a destructuring assignment syntax, we cannot have a trailing comma on the left side, so this:

    let [a, ...b,] = [1, 2, 3];
    

Will result in a `SyntaxError`.

### Object Destructuring

We can use the destructuring assignment syntax for objects as well. For example, we can write:

    const {a,b} = {a:1, b:2};
    

In the code above, `a` is set to 1 and `b` is set to 2 because the key is matched to the name of the variable when assigning the values to variables.

As we have `a` as the key and 1 as the corresponding value, the variable `a` is set to 1 because the key name matches the variable name. It is the same with `b`. We have a key named `b` with a value of 2, because we have the variable named `b`, we can set `b` to 2.

We can also assign it to different variable names, so we don’t have to set the key-value entries to different variable names. We just have to add the name of the variable we want on the value part of the object on the left side, which is the one we want to assign it to, like the following:

    const {a: foo, b: bar} = {a:1, b:2};
    

In the code above, we assigned the value of the key `a` to `foo` and the value of the key `b` to the variable `bar`. We still need `a` and `b` as the keys on the left side so they can be matched to the same key names on the right side for the destructuring assignment.

However, `a` and `b` aren’t actually defined as variables. It’s just used to match the key-value pairs on the right side so that they can be set to variables `foo` and `bar`.

Destructuring assignments with objects can also have default values. For example, we can write:

    let {a = 1, b = 2} = {a: 3};
    

Then, we have `a` set to 3 and `b` set to 2 which is the default value as we didn’t have a key-value pair with a key named `b` on the right side.

Default values can also be provided if we use the destructuring syntax to assign values to variables that are named differently from the keys of the originating object. So, we can write:

    const {a: foo=3, b: bar=4} = {a:1};
    

In this case, `foo` would be 1 and `bar` would be 4 because we assigned the left bar with the default value, but assigned `foo` to 1 with the destructuring assignment.

The destructuring assignment also works with nested objects. For example, if we have the following object:

    let user = {
      id: 42,
      userName: 'dsmith',
      name: {
        firstName: 'Dave',
        lastName: 'Smith'
      }
    };
    

We can write:

    let {userName, name: { firstName }} = user;
    

To set `displayName` to `'dsmith'` , and `firstName` to `'Dave'`. The lookup is done for the whole object and, so, if the structure of the left object is the same as the right object and the keys exist, the destructuring assignment syntax will work.

We can also use the syntax for unpacking values into individual variables while passing objects in as arguments.

To do this, we put what we want to assign the values, which is the stuff that’s on the left side of the destructuring assignment expression, as the parameter of the function.

So, if we want to destructure `user` into its parts as variables, we can write a function like the following:

    const who = ({userName, name: { firstName }}) => `${userName}'s first name is ${firstName}`;
    
    who(user)
    

So, we get `userName` and `firstName`, which will be set as `'dsmith'` and `'Dave'` respectively as we applied the destructuring assignment syntax to the argument of the `who` function, which is the `user` object we defined before.

Likewise, we can set default parameters as with destructuring in parameters like we did with regular assignment expressions. So, we can write:

    const who = ({userName = 'djones', name: { firstName }}) => `${userName}'s first name is ${firstName}`
    

If we have `user` set to:

    let user = {
      id: 42,
      name: {
        firstName: 'Dave',
        lastName: 'Smith'
      }
    };
    

Then we when call `who(user)`, we get `'djones's first name is Dave'` as we set `'djones'` as the default value for `userName`.

We can use the destructuring assignment syntax when we are iterating through iterable objects. For example, we can write:

    const people = [{
        firstName: 'Dave',
        lastName: 'Smith'
      },
      {
        firstName: 'Jane',
        lastName: 'Smith'
      },
      {
        firstName: 'Don',
        lastName: 'Smith'
      },
    ]
    
    for (let {
      firstName,
      lastName
    } of people) {
      console.log(firstName, lastName);
    }
    

We get:

    Dave Smith
    Jane Smith
    Don Smith
    

Logged, as the destructuring syntax works in `for...of` loops because the variable after the `let` is the entry of the array.

Computed object properties can also be on the left side of the destructuring assignment expressions. So, we can have something like:

    let key = 'a';
    let {[key]: bar} = {a: 1};
    

This will set `bar` to 1 because `[key]` is set to `a` and then the JavaScript interpreter can match the keys on both sides and do the destructuring assignment to the variable `bar`.

This also means that the key on the left side does not have to be a valid property or variable name. However, the variable name after the colon on the left side has to be a valid property or variable name.

For instance, we can write:

    const obj = { 'abc 123': 1};
    const { 'abc 123': abc123 } = obj;
    
    console.log(abc123); // 1
    

As long as the key name is the same on both sides, we can have any key in a string to do a destructuring assignment to variables.

Another thing to note is that the destructuring assignment is smart enough to look for the keys on the same level of the prototype chain, so if we have:

    var obj = {a: 1};
    obj.__proto__.b = 2;
    const {a, b} = obj;
    

We still get `a` set to 1 and `b` set to 2 as the JavaScript interpreter looks for `b` in the prototype inheritance chain and sets the values given by the key `b`.

As we can see, the destructuring assignment is a very powerful syntax. It saves lots of time writing code to assign array entries to variables or object values into their own variables.

It also lets us swap variables without temporary variables and makes the code much simpler and less confusing. It also works through inheritance so the property does not have to be in the object itself, but works even if the property is in its prototypes.

    const fn = () => {a:1, b:2}
    const {a,b} = fn();
    

The post [Using the Destructuring Assignment Syntax in JavaScript](https://thewebdev.info/2021/06/28/using-the-destructuring-assignment-syntax-in-javascript/) appeared first on [The Web Dev](https://thewebdev.info).