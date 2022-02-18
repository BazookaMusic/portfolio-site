---
title: Inferring a type in exponential time in C#
date: '2022-01-09'
tags: ['type inference', 'CSharp']
draft: false
summary: A showcase of the very intuitive proof that type inference is exponential in regards to code size.
---

I've always found proofs by counterexample to be one of the most awe-inducing tools we have in mathematics. Hard questions
are solved not by a complex,hard to understand, mathematical paper, but by a single counter-example which can even occupy a single page or line.

In this short article, I'll write a short demonstration of how the compiler can take exponential time to infer the types of a program. This proves that type inference in C# is at least of exponential complexity.

## Doubling the size of a type

Let's define a function called `Grow`. This function will take a value of type `T` and return a 2-tuple with the value duplicated.

```c#
public static (T,T) Grow<T>(T item)
{
    return (item, item);
}
```

An example:

```C#
static void Main()
{
    int someInt = 1;
    var someIntTuple = Grow(someInt); // has type (int, int)
}
```

## Composability

Our new function is generic and can be composed with itself! 

```C#
static void Main()
{
    int someInt = 1;
    var someIntTuple = Grow(someInt); // has type (int, int)

    var someIntTupleTuple = Grow(someIntTuple); // has type ((int,int),(int, int))
}
```

Let's keep doing this. We'll double the type 64 times:

```C#
static void Main()
{
    // int
    var v0 = 0;

    // (int, int)
    var v1 = Grow(v0);

    // ((int, int), (int, int))
    var v2 = Grow(v1);

    // (((int, int), (int,int)) , ((int, int), (int,int)))
    var v3 = Grow(v2);

    // ...
    var v4 = Grow(v3);
    var v5 = Grow(v4);
    var v6 = Grow(v5);
    var v7 = Grow(v6);
    var v8 = Grow(v7);
    var v9 = Grow(v8);
    var v10 = Grow(v9);
    var v11 = Grow(v10);
    var v12 = Grow(v11);
    var v13 = Grow(v12);
    var v14 = Grow(v13);
    var v15 = Grow(v14);
    var v16 = Grow(v15);
    var v17 = Grow(v16);
    var v18 = Grow(v17);
    var v19 = Grow(v18);
    var v20 = Grow(v19);
    var v21 = Grow(v20);
    var v22 = Grow(v21);
    var v23 = Grow(v22);
    var v24 = Grow(v23);
    var v25 = Grow(v24);
    var v26 = Grow(v25);
    var v27 = Grow(v26);
    var v28 = Grow(v27);
    var v29 = Grow(v28);
    var v30 = Grow(v29);
    var v31 = Grow(v30);
    var v32 = Grow(v31);
    var v33 = Grow(v32);
    var v34 = Grow(v33);
    var v35 = Grow(v34);
    var v36 = Grow(v35);
    var v37 = Grow(v36);
    var v38 = Grow(v37);
    var v39 = Grow(v38);
    var v40 = Grow(v39);
    var v41 = Grow(v40);
    var v42 = Grow(v41);
    var v43 = Grow(v42);
    var v44 = Grow(v43);
    var v45 = Grow(v44);
    var v46 = Grow(v45);
    var v47 = Grow(v46);
    var v48 = Grow(v47);
    var v49 = Grow(v48);
    var v50 = Grow(v49);
    var v51 = Grow(v50);
    var v52 = Grow(v51);
    var v53 = Grow(v52);
    var v54 = Grow(v53);
    var v55 = Grow(v54);
    var v56 = Grow(v55);
    var v57 = Grow(v56);
    var v58 = Grow(v57);
    var v59 = Grow(v58);
    var v60 = Grow(v59);
    var v61 = Grow(v60);
    var v62 = Grow(v61);
    var v63 = Grow(v62);
}
```

Now the type is a complex type with `2^64` references to the int type. To compile this program, the compiler will need to create the string representation of these types and replace the `var` keyword with the correct types. See this [sharplab snippet](https://sharplab.io/#v2:CYLg1APgAgTAjAWAFBQMwAJboMLoN7LpGYZQAs6AsgBQCU+hxTA9M+gJYB2ALo00QDcAhgCd0AgAzoAvOgkBuZH36t01LtwA0HHrWVNhYgXBnoA4iID2Ad2qTaipPuKrq6nto21t7rTu60ekj8xIbiMKYWNnZwDkrBIeiubhqeuj6pXvQ+vmkBGR5ZQYniouIYslG2AjBxTgkhqgB0Lc6CZQIUlVbVqHUlYQIArJE9dmT9iYMAbKPRw5MhgwDsc9XTi/yDABxrdsubBh0AnHsC24ehHXBS3fPHl+1GcCZ31TePpc8RbzGxjgNrhVzGNjLUAcQAL7KZRoTBwWbUAAqmiR9CqAB4kQA+ZEcbgAUwAtsViAQGvwoKt1ISiZ5aZtoUhIUA==) to see what this looks like in practice. 

## Time Complexity Lower Bound
Seeing as the size of the types grow exponentially with the amount of lines in our specially constructed program, we have found an instance of the problem that cannot be solved in polynomial time in regards to the program size.

## Closing Remarks

People don't usually write programs this way. They try to create data structures and types which will fit in memory and make their code perform well. The algorithm used for type inference in C# is fast enough for most use cases, where people don't try to bring the compiler to its knees by writing adversarial programs. 



