Observations are a way of describing a program through semi-formal almost pseudocode like language which seeks to describe different properties of types while relying on the semantic understanding of an implementer to fully formalize within a programming language. The intent is for most implementers with some knowledge of programming to be able to look through a set of observations and write an implementation that fulfills the requirements. 

Here is a notation for observations that may cover most of your cases,

```{oblang}
X # There exists a type X
P(X) # For all X there exists some P
P(X) := Y # For all X there exists some P of type Y
P(X) := Y | cond is True # For all X there exists some P of type Y given a condition cond is True
P(X) := Y iff cond is True # For all instances of X there exists some P of type Y if and only if a condition cond is True
P(x : X) := Y # For all X, take an arbitrary instance x, there exists a P of type Y
P(x : X) := Q(x') = Y # For all X, take an arbitrary instance x, there exists a P such that all unrelated observations of x held constant, a Q of x is Y
X[Y] # There exists a type X parameterized by Y
P(X[Y]) := Y # For all X parameterized by Y, there exists a P that is of type Y
X :< Y # All observations of Y apply to X unless contradictory or specified otherwise
X |> Y # An occurence of X causes Y
X <| Y # An occurrence of Y causes X
```

We may define various programs through observations without specifying their internals, only properties that we would like to observe of the final program. For example,

```{oblang}
List[A]
length(List) := z > 0
index(l : List[A], z : Z >= 0) := Union[A, None] | z < length(l)
put(l : List[A], z : Z >= 0, a : A) := index(l', z) = a | z < length(l) 
```

This is not a formal specification or a program, it is simply describing that a List has a length, its index returns a value that is A or None if the indexer is an integer greater than 0 and is less than the length of the List, and you can put a value of type A into the List if the indexer is less than the length.

It doesn't specify what happens if a user does put a value into the list at an index that is greater than its length, just that they may given the condition it is less than the length. In that sense it is very permissive and the implementer must understand what a List is inorder to actually implement the program. 

Keep in mind that the notation does not assume a particular programming paradigm, the List must have a length but this could be a function which iterates over a collection, could be a field in an object, or could be packaged in a Tuple alongside the List representation as an array. More to the point that we may never actually implement a generic List that must be parameterized by any type. In subsequent observations we may only need one that is defined for Integers and Floats and thus only need to observe a FloatList or an IntegerList if the implementer chooses to program in that manner. In the future we may consider this to be a precursor or "napkin math" to developing programs in a full formalized language like Lean.

