# Functional Design

Notes

1. Executable Encoding: pass a function to the case class. Has the adventage that you can add new operations nut the disadventage you can not add easily a new way of executing the model, a new function as parameter. Used normally to leverage legacy code

2. Declarative Encoding: operation as traits and objects extending those traits. Has the adventage you can easily add a new way of executing the model but disadventage that adding operations will lead you to imcompleteness in all the places where you pattern match


The best  primitives are:

1. Composable: proof with signature
2. Expressive: Assemble a list of problem you want to solve, and see if your operator is enough expressive to solve those problems
3. Orthogonal: you can not prove something is orthogonal but you can prove that something is not orthogonal if is possible to make up that operation with other operations


Patterns:

1. Binary
2. Sequentiality +
3. Fallbacking
4. andThen


Some resources recommended

1. The expression problem
2. scott wlaschin in f# does interesting talks
3. I think Alvin's FP book is good, but it's definitely at a more introductory level and really talks a pretty hybrid style
4. https://pragprog.com/titles/swdddf/domain-modeling-made-functional/
5. https://www.manning.com/books/functional-and-reactive-domain-modeling
6. https://www.youtube.com/watch?v=GqmsQeSzMdw Runar
7. https://leanpub.com/algebra-driven-design
8. https://github.com/system-f/fp-course
9. Implicit evidence, type evidence, variance
10. Scalazzi



