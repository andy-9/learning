# What is JSON?

```
{
    "car": {
        "color": "blue",
        "price": "$20,000",
        "wheels": [
            {
                "model": "X3",
                "location": "front-right"
            },
            {
                "model: "Y3",
                "location": "front-left"
            }
        ]
    },
    "bus": {
        "color": "white",
        "price": "120.000"
    }
}
```

## JSON Path
* is a query language to help parse data represented in a JSON- or YML-format
* the top-level dictionary is named `root`, denoted by a `$`
* use dot notation for querying: `$.car.price`
* use indexing for lists: `$.car.wheels[1].location`. 2 or more results have no space between commas! `$[1,3]`
* output is always a list

### Criteria
* check if: `?`
* each item in a list: `@`
* operators: `>`, `<`, `==`, `!=`, `@ in [40, 43, 45]`, `@ nin [40, 43, 45]`
* Get all numbers > 40: `$[?(@ > 40)]`
* Get the model of the front-left wheel: `$.car.wheels[?(@.location == 'front-left').model]`

### Wildcards
* get all prices: `$.*.price`
* get all models: `$.car.wheels[*].model`

### Lists
* Get elements from-to: `$[0:4]` (from 0 to 3) (not including 4!)
* Get every second - define step: `$[0:4:2]` (0 and 2)
* Get last element: `$[-1:0]` or simply `$[-1:]`
* Get the last 3 elements: `$[-3:]`

<br>

## Kubecontrol
### KubeCtl
* get nodes: `kubectl get nodes`  
  get pods: `kubectl get pods`
* print additional details: `kubectl get nodes - wide`
* print output in JSON-format: `kubectl get pods -o json`
* use the JSON path query with kubectl command: `kubectl get pods -o=jsonpath='{.items[0].spec.containers[0].image}'`
* merge queries: `kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{.items[*].status.capacity.cpu}'`
* add new line: `kubectl get nodes -o=jsonpath='{.items[*].metadata.name} {'\n'} {.items[*].status.capacity.cpu}'`
* loop: `kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name} {"\t"} {.status.capacity.cpu} {"\n"} {end}'`  
  or: `kubectl get nodes -o=custom-columns=NODE:.metadata.name, CPU:.status.capacity.cpu`
* sort output: `kubectl get nodes --sort-by= .metadata.name`
