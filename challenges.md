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