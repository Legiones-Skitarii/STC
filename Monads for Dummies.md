# Monads 101 - As I get them

<i>Note : I will assume
1. You know some programming of Python, Lisp, Java, C/C++ or JS (any one is fine)
2. You are aware of functional programming
3. You know what macros are (nvm if you actually used them)
4. You're interested in using monads, and not caring about the theory
</i>

<i>You're free to point out I'm wrong, but I would like to understand how. So I can learn too</i>

<hr>

I spent some time learning monads, and I must admit that after I got it, I felt an overwhelming disappointment that can only be attributed to annoyance at the simplicity of monads. Most people explain monads like they've never used them, or more appropiately, like they've used them a lot. 

This overwhelming tendency to explain monads in the broadest possible terms lends itself to an aura of mystique that is literally not deserved for something so relatively simple in its usage. To compensate for this blind spot, this is me learning how monads are used. I don't want to care for what they are, but I do want to know how to use them. So what are they ?

<i><b>TLDR : FP version of macros</b></i>

Essentially 
1. They're fp macros with a lot more power[1]
2. They're a design abstraction, not a fundamental part of code.

I'll explain 2 first, then get to 1. What do I mean they aren't a fundamental part of code ?

I mean that you can write programs without them. You will struggle and a lot more, but it can be done. They aren't like loops or control statements, which are essential parts of langauge. You could theoretically never use monads and live a happy life (albeit with ugly code).

They're a design abstraction. Like macros in lisp. You don't need to write macros, but you're code becomes a lot more powerful if you do.

So, rather than wax categorically and philosophically about what they are, let's go through some simple examples, and see exactly what they're doing.

Code for getting a cartesian product from 2/3 lists
```ocaml
let l1 = [1 ; 2 ; 3];;
let l2 = [4 ; 5 ; 6];;
let l3 = ['a' ; 'b' ; 'c'];;

let cartesian2 l1 l2 =
  let output = ref [] in
  List.iter (fun x ->
    List.iter (fun y ->
      output := (x, y) :: !output
    ) l2
  ) l1;
  List.rev !output

let cartesian3 l1 l2 l3 =
  let output = ref [] in
    List.iter (fun x ->
        List.iter (fun y ->
            List.iter (fun z ->
                output := (x, y, z) :: !output
            ) l3
        ) l2
    ) l1;
  List.rev !output
```

Let's ignore for a moment the fact that this is using a List.iter. If I wrote it using pure funcs, it'll be slightly more annoying.

The important thing to note is the headache writing this gives me. (You're free to attempt to test this out yourself)

Now, lemme show you the monadic version

```ocaml
let l1 = [1 ; 2 ; 3];;
let l2 = [4 ; 5 ; 6];;
let l3 = ['a' ; 'b' ; 'c'];;

let (>>=) x f = List.concat_map f x

let cartesian2 l1 l2 = 
    l1 >>= fun x ->
    l2 >>= fun y ->
        [(x,y)]

let cartesian3 l1 l2 l3= 
    l1 >>= fun x ->
    l2 >>= fun y ->
    l3 >>= fun z ->
        [(x,y,z)]
```

Now, if you've ever done webdev, there's an interesting parallel here with.....Javascript, of all the seven hells. More specifcally, callback hell [2]. JS solved this through promise chaining + async/await. Basically, synctactic sugar to hide the ugly details underneath. 

In a (not really) similar vein, macros allow you to abstract away incredibly ugly details beneath a classy veneer of simplicity.

And if I had to define what monads do, I would say that monads abstract over functions so that I can write complicated stuff more easily. In other words

| Monads abstract away computational complexity for easier function chaining

That's it. Congratulations, you now have some idea of what monads do. To repeat,
1. Abstract stuff so functions are easier to write.
2. Prevent pyramid of doom

That's it. That's all they do. There's a couple more examples below, but otherwise, yay ! You know what they are. Also, they're a monoid in the category of endofunctors [3].

<i>P.S : I would recommend that you use claude/gpt/deepseek and go solving simple problems like chaining 2 funcs to more complex conditionals and graph stuff to get an intuition for monads. They're good teachers, though do be cautious of mistakes they'll make. Run the code you get, and practice often.</i>

<hr>

## Examples of different monads (will be updated in the eventual future)
Simple Counter using a state monad
```ocaml
type 'a state = State of (int -> int * 'a);;
let get = State (fun s -> (s,s));;
let put s = State (fun _ -> s,());;
let return x = State (fun s -> s,x);;
let run_state (State f) x = f x;;
let (>>=) (State comp) f = State (
    fun s -> 
        let s',a = comp s in
        let State comp' = f a in
        comp' s'
)

let increment = get >>= fun s -> put (s+1);;
let reset_counter = get >>= fun _ -> put 0;; 
let rec reset_at_30 = get >>= fun s ->
    if s >= 30 then reset_counter
    else increment >>= fun _ -> reset_at_30
;;
run_state reset_at_30 0
```


[1] they're not actually macros, but it's a good starting point for some sort of intuition.

[2] https://en.wikipedia.org/wiki/Pyramid_of_doom_(programming)

[3] https://youtu.be/srQt1NAHYC0?si=EwgocwChX7MlHG7M