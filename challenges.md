# Catergory theory for programmers 

## Chapter 1 challenges 

1. '. Implement, as best as you can, the identity function in your favorite
language (or the second favorite, if your favorite language
happens to be Haskell).' 

```reason
let id(x) = x;
```

2. 'Implement the composition function in your favorite language. It
takes two functions as arguments and returns a function that is
their composition.'

```reason
let plusOne = x => x + 1;

let plusTwo = x => x + 2;

let plusThree = x => x |> plusOne |> plusTwo;

Js.log(plusThree(3)); //logs 6
```

3. 'Write a program that tries to test that your composition function
respects identity.'

