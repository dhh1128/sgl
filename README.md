[![CircleCI](https://circleci.com/gh/evernym/sgl.svg?style=svg)](
https://circleci.com/gh/evernym/sgl)

# SGL (Simple Grant Language)

SGL is a simple but flexible [DSL](
https://en.wikipedia.org/wiki/Domain-specific_language) for matching people against criteria (typically, authorization). It is like [XACML](
https://en.wikipedia.org/wiki/XACML) but simpler and JSON-oriented. You
can use it to write rules about who should be able to do what, and then
to compare circumstances to the rules to enforce custom logic. This lets
you create your own [Role-Based Access Control](
https://en.wikipedia.org/wiki/Role-based_access_control)
mechanisms, as well as authorizations based on other criteria.

For example, here's an SGL rule that says only members of the press
should be allowed backstage at a concert:

```JSON
{"grant": ["backstage"], "when": { "roles": "press" }}
```

And here's how you might use that rule in python code. (Compare the [JavaScript/Node.js equivalent](https://github.com/evernym/simple-grant-lang/blob/master/README.md)):

```python
from sgl.api import satisfies

my_rule = {"grant": ["backstage"], "when": { "roles": "press" }}

people = [
    {"id": "Alex", "roles": ["ticket-holder"]},
    {"id": "Sofia", "roles": ["ticket-holder", "press"]}
]

for person in people:
    name = person['id']
    if satisfies(person, my_rule):
        print(f"Welcome backstage, {name}.")
    else:
        print(f"Sorry, this area is restricted, {name}.")
```

If you ran this code, you'd see:

```bash
$ python sample_code.py
Sorry, this area is restricted, Alex.
Welcome backstage, Sofia.
```

SGL supports arbitrarily complex rules with boolean operators, as well
as rules that require multiple people to jointly exercise a privilege.
However, it's simple enough that you should be able to learn it in just
a few minutes. See the [tutorial](
https://evernym.github.io/sgl/docs/tutorial.html).

>Note: SGL can be [rendered in various styles](
https://evernym.github.io/sgl/docs/renderings.html). The example above
uses the recommended JSON rendering, since JSON is familiar, broadly
supported, and easy to read. Other possibilities include ProtoBuf,
MsgPack, CBOR, XML, and human-friendly text.

SGL is not integrated with any particular enforcement mechanism, because
it's designed for problems where you have to do your own enforcement.
Hooking it up to enforcement mechanisms is trivial, though.

## See also
* [Tutorial](https://evernym.github.io/sgl/docs/tutorial.html)
* [Reference](https://evernym.github.io/sgl/docs/reference.html)
