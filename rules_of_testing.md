# Testing and the existential crisis
## TLDR : The basic rules of testing software

Recently, as I started writing unit tests for software (and longingly looked at some decorative nooses while doing so), I got so confused that I eventually had to come up with bunch of rules on how to write tests.

Everybody has some fancy shmancy theory on this stuff. Here’s a simple set of rules. Like actionables. Not advice.

---

Let there exist a function

`f :: x -> y, S?`

where f is the function, x in the input, y is an output, and S is an optional side effect

1. For any input `x` and an expected output `y`, let `f(x) = y'`
    * assert `y' == y`
2. If `S` exists and if `O(S) == O(1) && O(S') == O(1)`
    * `assert S`
    * Explanation : If a side effect can be done in constant time and can be reversed without significant issues, perform it.
3. If `S` exists and if `O(S) != O(1) || O(S') != O(1)`
    * `mock(S)`
    * Explanation : The inverse of the above. If a side effect is expensive to perform or cannot be reversed easily, mock it.
4. If `f` consists of `(f1 . f2 . f3 ... fx)` , recursively apply rules 1-3 for the inner functions
5. If `f` consists of only `(f1)` with no additional logic and the functions are of the form `f(x)` and `f1(x , c)` then
    * `mock(f1)`
    * Explanation : We assume that if rule 4 is applied, then `f1` has already been tested robustly. Given no additional logic, we can assume that the only thing to test is whether the function is called with the proper parameters

6. Extra Rules
    * If a function doesn't return a value (as is the case in most python libraries which simply perform a side effect without the ability to check their status in wonderful oops fashion) i.e `y` doesn't exist
        * Then ignore rule 1 and simply apply rule 2 & rule 3
        * Note : Please don't do this. Functions that return nothing are literally pointless. Put an underscore if you don't need the values, but performing a side effect without the ability to check it's status is incredibly shitty code.

*Note : I've been told rule 5 depends on the code, but I shall leave it in for now since I have yet to see good code to the contrary. If you think I'm wrong, drop me a mail.*
