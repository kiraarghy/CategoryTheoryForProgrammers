# Catergory theory for programmers 

## Chapter 1 challenges 

1. <i>'Implement, as best as you can, the identity function in your favorite
language (or the second favorite, if your favorite language
happens to be Haskell).' </i>

```reason
let id(x) = x;
```

2. <i>'Implement the composition function in your favorite language. It
takes two functions as arguments and returns a function that is
their composition.'</i>

```reason
let plusOne = x => x + 1;

let plusTwo = x => x + 2;

let plusThree = x => x |> plusOne |> plusTwo;

Js.log(plusThree(3)); //logs 6
```

3. <i>'Write a program that tries to test that your composition function
respects identity.'</i>

I'm not sure for this one, I would assume that if I check the equivalency of the output of the plusThree versus the int I expect it to output would check that the function respects identity?

```reason
let plusOne = x => x + 1;

let plusTwo = x => x + 2;

let plusThree = x => x |> plusOne |> plusTwo;

Js.log(plusThree(3) //output of 6 === 6); //logs true
```

4. <i>'Is the world-wide web a category in any sense? Are links morphisms?'</i>

I'm not really sure? Like if we're going on a basic idea of the WWW being a bunch of static pages or a data source and then links act as functions telling the data source I want X, then at a stretch, yes? But that's getting pretty abstract and really links if we include get/ post requests aren't really pure functions as data goes from the client to the server and visa versa in one request.

5. <i>'Is Facebook a category, with people as objects and friendships as
morphisms?'</i>

See above...

6. <i>'When is a directed graph a category?'</i>

I don't know a huge amount of directed graphs, but from quick research, directed graphs have fewer rules when connecting up dots, catergory theory considers that the functors connecting up the dots have to follow very strict rules e.g. ```string -> string``` is fine but ```string -> int``` without a corresponding morphism would not be okay, in a directed graph the above examples could be fine. If a directed graph however followed the above rules as per catergory theory, it would be a catergory.

## Chapter 2 challenges

1. Define a higher-order function (or a function object) memoize in your favorite language. This function takes a pure function f as an argument and returns a function that behaves almost exactly like f, except that it only calls the original function once for every argument, stores the result internally, and subsequently returns this stored result every time it’s called with the same argument. You can tell the memoized function from the original by watching its performance. For instance, try to memoize a function that takes a long time to evaluate. You’ll have to wait for the result the first time you call it, but on subsequent calls, with the same argument, you should get the result immediately.

Note this works for a pure function g. I'm gonna see if I can do a function that is less prescriptive in the type of arguments it may receive.

```reason let blah = Hashtbl.create(1);

let memoizer = (~f: int => int, n: int) =>
  Hashtbl.mem(blah, n) ?
    Hashtbl.find(blah, n) :
    {
      let x = f(n);
      Hashtbl.add(blah, n, x);
      x;
    };

let rec g = (n: int) =>
  switch (n) {
  | 100000000000000 => 100000000000000
  | _ =>
    let x = g(n + 1);
    x;
  };

```

console output:

```
g unmemoized: 143.489ms
g memoized establish: 149.157ms
g unmemoized: 0.402ms
```

2. Try to memoize a function from your standard library that you
normally use to produce random numbers. Does it work?

```reason
let blah = Hashtbl.create(1);

let memoizer = (~f: int => int, n: int) =>
  Hashtbl.mem(blah, n) ?
    Hashtbl.find(blah, n) :
    {
      let x = f(n);
      Hashtbl.add(blah, n, x);
      x;
    };

memoizer(~f = Random.int, 3);
```

It seems to work, console output: 

```
value: 1
notmemoizedyet: 3.037ms
value: 1
memoized: 0.412ms
```

<b> But we end up in a situation where no matter what the randomness is eliminated. The memoized version will always return the first evaluated value. This is due to Random.int in Reason being an impure function.

3. Most random number generators can be initialized with a seed. Implement a function that takes a seed, calls the random number generator with that seed, and returns the result. Memoize that function. Does it work?

```reason
let blah = Hashtbl.create(1);

let memoizer = (~f: int => int, n: int) =>
  Hashtbl.mem(blah, n) ?
    Hashtbl.find(blah, n) :
    {
      let x = f(n);
      Hashtbl.add(blah, n, x);
      x;
    };

    let g = (seed: int) => {
        Random.init(seed);
        Random.int(10);
    }


memoizer(~f = g, 10);
memoizer(~f = g, 10);
memoizer(~f = g, 12);
memoizer(~f = g, 12);
```

Console output: 

```
6
unmemoized: 7.225ms
6
memoized: 8.447ms
6
memoized: 0.500ms
4
memoized: 4.173ms
4
memoized: 0.064ms
```

<b> This seems to work as expected, in this case supplying func g with a seed means it is pure, we can guarantee that provided we pass in the same int we will always get the same int out.


Which of these C++ functions are pure? Try to memoize them and observe what happens when you call them multiple times: memoized and not.
(a) The factorial function from the example in the text.
(b) 
```C 
std::getchar()
```
(c) 
```C
bool f() {
std::cout << "Hello!" << std::endl;
return true;
}
```
(d) 
```C 
int f(int x) {
static int y = 0;
y += x;
return y;
}
```

This is kinda annoying as I'm not doing this is C++ 🤔.

<b> A The factorial example referred to is this:

```C
int fact(int n) {
  int i;
  int result = 1;
    for (i = 2; i <= n; ++i)
      result *= i;
    return result;
}
```

The above function seems to be pure, we can guarantee that as long as the same n is put in we'll always get the same result.

```Reason
let blah = Hashtbl.create(1);
let rec fact = (n: int) =>
  if (n >= 1) {
     (fact(n - 1) *n);
  } else {
    1;
  };

let memoizedfact = (n: int) =>
  Hashtbl.mem(blah, n) ?
    Hashtbl.find(blah, n) :
    {
      let x = fact(n);
      Hashtbl.add(blah, n, x);
      x;
    };
```

console output: 

```
120
unmemoized: 2.833ms
120
unmemoized: 0.158ms
120
memoized: 0.578ms
120
memoized: 0.369ms
```

(fact(5) 2 times for memoized and nonMemoized)

The above implemented in Reason results in some performance weirdness in the memoized version. I assume this is due to V8 being super useful™ and finding performance enhancements for the non-memoized version.

<b> B 

Not sure about 

```C
std::getchar()
```

I *think* it is pure having read a number of C articles but have no clue.

<b> C

```C
bool f() {
std::cout << "Hello!" << std::endl;
return true;
}
```

```reason
let bool = () => {
  Js.log("Hello!");
  true;
};


```

The above in an impure function (side-effects of printing "Hello!").

However as the logging doesn't effect the execution of the function, we can guarantee it will return true. Memoizing this would be simple enough but results in a less efficient execution of the code.

<b> D
  
```C 
int f(int x) {
static int y = 0;
y += x;
return y;
}
```

```reason
let blah = Hashtbl.create(1);

let memoizer = (~f: int => int, n: int) =>
Hashtbl.mem(blah, n) ?
Hashtbl.find(blah, n) :
{
    let x = f(n);
    Hashtbl.add(blah, n, x);
    x;
};

let f = (x: int) => {
  let y = 0;
  let y = y + x;
  y;
};
```

Console output:

```
3
f: 2.456ms
3
f: 0.130ms
3
memoized f: 0.587ms
3
memoized f: 0.422ms
```

Again weirdness from V8, I feel at this point I might have to look at my base memoization function 😭.
