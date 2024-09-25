# Repository for testing branch rulesets with the audit log

There will be a set of branch restriction policies enabled for the default branch and for all branches that match `releases/*`. The rules listed below will be enforced without bypassers during different time intervals.

There are two rulesets:

* On the repository level: `2024-09-24 - initial test (do not delete)`
* On the organization level: `2025-09-25 - initial test for branch-ruleset-repos (do not delete)`

There will be the following timeline:

* **T0:** 2024-09-24 19:58 UTC: The repository level ruleset is created
* **T1:** 2024-09-25 05:40 UTC: The repository level ruleset is changed
* **T2:** 2024-09-25 08:37 UTC: An organization level ruleset is created and the repository ruleset disabled

## Rule enforcement

| Rule                                                    | TO -> T1 | T1 -> T2   | T2 -> _now_  |
|----------------------------------------                 | ---------|----------  |--------------|
| non_fast_forward                                        | No       | Yes        | Yes          |
| deletion                                                | Yes      | No         | Yes          |
| pull_request _(with 1 approver and everything on true)_ | Yes      | Incomplete | Yes          | 

## Tests

There are the following workflow files which call the respective signing policies for which policies have been configured:

* [01_continuity_error.yml](.github/workflows/01_continuity_error.yml): This signing policy evaluates that pull requests (including all flags) and a deletion restriction have been enabled since T0. As both were not set between T1 and T2, this workflow fails.
* [02_build_time_error.yml](.github/workflows/02_build_time_error.yml): This signing policy validates that the creation was restricted at build time. Creation was nevery restricted, so it fails.
* [03_before_repo_creation_error.yml](.github/workflows/03_before_repo_creation_error.yml): This signing policy validates that pull_request (without flags) has been enabled since T-1 (before the repo was created). That is of course not possible and the workflow thus fails.
* [04_success.yml](.github/workflows/04_success.yml): This signing policy validates that non_fast_forward has been restricted since T1. This is the case, so it should pass.
