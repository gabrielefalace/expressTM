# Threat Flow
a condensed STRIDE based TM method

# Summary

Yet another threat modeling technique tailored to do a simple yet effective (enough) threat modeling. It's based on classic STRIDE methodology and 
* Uses "criticality" instead of (or better yet, in addition to) usual Trust Zones, in order to better capture distributed architectures (microservices, anyone?)
* Condenses the STRIDE rules in a more compact form to maximize speed
* mixes in some of privacy threat form the LINDDUN framework 

# Input

An architecture diagram is needed as input, such as a DFD

# Procedure

## Finding Criticalities
We'll label every system or actor in the diagram as having a “criticality” value. This will range from 0 to a maximum number. This doesn't need to be determined up front, rather will be the result of updating it several times during the labeling process itself.

Remark: in old-fashioned “layered” systems, trust zones used to implicitly capture the concept of criticality too. 
In a microservice setting, services in different trust zones might have the same level of criticality. 
In this case we need to denote criticality explicitly, otherwise we end up with a model where we can't infer nothing from statements like _“Here we have a system labeled 2 accessing directly a system labeled 13“_. In fact — when labeling per trust zone — we'd reach high number pretty quickly, but those numbers wouldn't have have a criticality meaning therefore rendering impossible to draw conclusions about “bad relationships” among systems.

1. Start by giving untrusted external entities 0 e.g. users
2. Every element directly contacted by a 0 will have 1
3. navigate the diagram and give a value o criticality, answering the question: “how harmful is if this component gets compromised?”


## Applying STRIDE rules, flow based

Here's a pseudocode of applying the STRIDE with some additional privacy-specific threats from the LINDDUN framework.

```
for each flow (orig, dest):
    delta = criticality(dest) - criticality(orig)
    if delta < 0:
        mark dest with 🅘
    if delta > 0:
        mark dest with 🅔
        mark flow with 🅣
        if criticality(orig) = 0:
            mark dest with 🅢 🅓 🅡
            mark dest with 🆄 
    if dest is dataStore:
        PII -> 🅸
        other -> 🅳
        PII & other -> 🅻🅸🅳 
```
