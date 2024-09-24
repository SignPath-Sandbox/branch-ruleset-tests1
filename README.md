# Repository for testing branch rulesets with the audit log

There will be a set of branch restriction policies enabled for the default branch and for all branches that match `releases/*`. The rules listed below will be enforced without bypassers during different time intervals.

There are two rulesets:

* On the repository level: `2024-09-24 - initial test (do not delete)`
* On the organization level: `2025-09-25 - initial test for branch-ruleset-repos (do not delete)`

There will be the following timeline:

* **T0:** 2024-09-24 17:51 UTC: The repository level ruleset is created
* **T1:** TODO: The repository level ruleset is changed
* **T2:** TODO: An organization level ruleset is created and the repository ruleset disabled

## Rule enforcement

| Rule                                                    | TO -> T1 | T1 -> T2   | T2 -> _now_  |
|----------------------------------------                 | ---------|----------  |--------------|
| non_fast_forward                                        | No       | Yes        | Yes          |
| deletion                                                | Yes      | No         | Yes          |
| pull_request _(with 1 approver and everything on true)_ | Yes      | Incomplete | Yes          | 

