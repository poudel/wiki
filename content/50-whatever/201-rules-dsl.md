+++
title = "[WIP] SaaS DSL"
weight = 201
date = 2023-10-01
+++

This is me Mulling over rule engines and DSLs.

Usually in a complex SAAS (Software as a Service) project, you need to
let some power users be able to turn some knobs and essentially
program the product to their needs.

Examples include things like JQL for Atlassian's JIRA, various
drag-and-drop flow programming systems that let you hook actions on
top of events happening etc. This empowers the users to cater the
product to their needs without the devs being involved. When done
right, this can save a lot of product development time.

Some SAAS products actually embed a programming language and expose it
to the users. For example, the now deprecated Auth0 rules lets you
write javascript functions.

While you want to empower the users, and give them powerful tools to
do their jobs, you want to make sure that the power is still limited.
Of course, plugin systems are not a new thing. But in the context of a
SAAS, we're talking about executing untrusted code.


## Further Links to read

* [Auth0 Rules](https://auth0.com/docs/customize/rules)
* [jmespath.py](https://github.com/jmespath/jmespath.py)
* [Prolog in python](https://www.openbookproject.net/py4fun/prolog/prolog1.html)
* [Greenspun's tenth rule](https://en.wikipedia.org/wiki/Greenspun%27s_tenth_rule)
* [HCL](https://github.com/hashicorp/hcl)
* [jq](https://github.com/jqlang/jq)


## [WIP] Roles and permissions

Some interesting DSLs to manage permissions

### Prolog

* [Gerrit example/rules.pl](https://github.com/GerritCodeReview/gerrit/blob/master/prologtests/examples/rules.pl)
* [Prolog Submit Rules Cookbook (Deprecated)](https://gerrit-review.googlesource.com/Documentation/prolog-cookbook.html)

### Zanzibar

* [Why Google Zanzibar shines at building authorization](https://news.ycombinator.com/item?id=36470943)
* [Zanzibar paper by Google](https://research.google/pubs/pub48190/)
* [Auth0 FGA](https://docs.fga.dev/)
* [Zanzibar Academy](https://zanzibar.academy/)

### AWS

* [Cedar](https://www.cedarpolicy.com/en)
* [Cedar: A new policy language](https://onecloudplease.com/blog/cedar-a-new-policy-language)
* [AWS IAM Roles, a tale of unnecessary complexity](https://news.ycombinator.com/item?id=33566419)
