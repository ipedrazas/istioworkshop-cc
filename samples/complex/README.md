# Complex Microservice model


TYhis model has 5 microservices. Entry calls Dep1 and Dep2, and...

Entry -> Dep1, Dep2

Dep1 -> Dep1Dep1, Dep2Dep1
Dep2 -> Dep2Dep2, Dep2Dep1

Dep1Dep1 -> 0
Dep2Dep1 -> 0
Dep2Dep2 -> Dep3

