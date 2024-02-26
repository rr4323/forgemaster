# Appropriate use of node-role labels

## Table of Contents

<!-- toc -->
- [Release Signoff Checklist](#release-signoff-checklist)
- [Summary](#summary)
- [Motivation](#motivation)
  - [Goals](#goals)
- [Proposal](#proposal)
  - [Use of <code>node-role.kubernetes.io/*</code> labels](#use-of-node-rolekubernetesio-labels)
  - [Current users of <code>node-role.kubernetes.io/*</code> within the project that must change](#current-users-of-node-rolekubernetesio-within-the-project-that-must-change)
    - [Service load-balancer](#service-load-balancer)
    - [Node controller excludes master nodes from consideration for eviction](#node-controller-excludes-master-nodes-from-consideration-for-eviction)
    - [Kubernetes e2e tests](#kubernetes-e2e-tests)
    - [Preventing accidental reintroduction](#preventing-accidental-reintroduction)
- [Design Details](#design-details)
  - [Migrating existing deployments](#migrating-existing-deployments)
    - [Instructions for deployers](#instructions-for-deployers)
  - [Test Plan](#test-plan)
  - [Graduation Criteria](#graduation-criteria)
  - [Upgrade / Downgrade Strategy](#upgrade--downgrade-strategy)
  - [Version Skew Strategy](#version-skew-strategy)
- [Production Readiness Review Questionnaire](#production-readiness-review-questionnaire)
  - [Feature enablement and rollback](#feature-enablement-and-rollback)
  - [Rollout, Upgrade and Rollback Planning](#rollout-upgrade-and-rollback-planning)
  - [Monitoring requirements](#monitoring-requirements)
  - [Dependencies](#dependencies)
  - [Scalability](#scalability)
  - [Troubleshooting](#troubleshooting)
- [Implementation History](#implementation-history)
- [Future work](#future-work)
- [Reference](#reference)
<!-- /toc -->

## Release Signoff Checklist

These checklist items _must_ be updated for the enhancement to be released.

- [ ] Feature issue in release milestone, which links to PEP: https://github.com/kubernetes/enhancements/issues/1143
- [ ] PEP approvers have set the PEP status to `implementable`
- [ ] Design details are appropriately documented
- [ ] Test plan is in place, giving consideration to SIG Architecture and SIG Testing input
- [ ] Graduation criteria is in place
- [ ] "Implementation History" section is up-to-date for milestone
- [ ] User-facing documentation has been created in website
- [ ] Supporting documentation e.g., additional design documents, links to mailing list discussions/SIG meetings, relevant PRs/issues, release notes

**Note:** Any PRs to move a PEP to `implementable` or significant changes once it is marked `implementable` should be approved by each of the PEP approvers. If any of those approvers is no longer appropriate than changes to that list should be approved by the remaining approvers and/or the owning SIG (or SIG-arch for cross cutting PEPs).

**Note:** This checklist is iterative and should be reviewed and updated every time this enhancement is being considered for a milestone.

## Summary

This application help user to record its yearly goal and its progress towards that goal.

## Motivation

as didn't find any common app to provide all the required feature to reach the goal without distracting user from the goal and having best user experience


### Goals

This PEP:

* To provide a simple way of recording personal goals and tracking their progress over time.
* To allow users to easily view their own progress against their goals.
* To provide a simple way of sharing their progress with friends and family.

### Non-Goals

This PEP does not aim to replace existing goal tracking applications or provide a comprehensive system for tracking goals.

## Proposal
* A timetable module which contains the timetable designed to achieve a goal.
* A dashboard module which displays all the timetables and their progress.
* The ability to add/remove timetables from the dashboard.
* Each timetable will contain multiple tasks, each representing one step towards achieving the overall goal.
* Users can mark tasks as complete when they have achieved them. This will update the progress bar for that timetable.
* A master timetable which contains all the timetable to track how much progress has been made on the whole goal.
* User can make notes  about any task they complete or encounter difficulties with.
* User can subscribe to other people 's timetables so they can see how others are getting on with their goals.
* Every user has access to their own data but can also share it with others if desired.
* A authentication and authorization module to ensure only authorised people can see other's data.
* A licensing module so users know who developed the app and under what terms they can use it.



## Design Details
The proposed design consists of three main modules: `timetable`, `dashboard` and `auth`. These are described below in more detail

### Implementation Plan

1. Define the basic structure of the `Timetable` class
2. Create the `Dashboard` class which holds instances of `Timetable`. This is where the user views all their timetable.
3. Create authentication and  authorization system (in progress)
4. Build out the `dashboard`  module using the `timetable` module as a base (not started)
5. Add functionality for adding/removing timetables from the dashboard (not started)
6. Allow users to mark tasks as completed in the `timetable` module (partially done - needs UI work).
7. Make the `notes` attribute on the `Task` object more robust. This could be a dictionary where the keys are dates and the values are notes. This would allow users to add
7. Make the `notes` field on the `Task` model more versatile (e.g., allowing HTML input).
8. Look into ways of displaying the data on the `dashboard`. Possibly using Django's ORM to generate an Excel spreadsheet
8. Look into ways of improving the sharing feature (e.g., email links, QR codes).
9. Investigate how to handle licenses effectively.

### User Stories

- As a user I want to be able to set up a new goal by creating a `Timetable`.
So that I can record my intentions and start working towards them.


- As a user I want to add a `Task` to one of my `Timetables`.
So that I can break down larger goals into smaller, more manageable steps. 

- As a user I want to mark a `Task` as complete when it is finished.
So that I can see which tasks have been accomplished.

### Test Plan

* Unit tests to verify selection using feature gates

### Graduation Criteria

* New labels and feature flags become beta after one release, GA and defaulted on after two, and are removed after two releases after they are defaulted on (so 4 releases from when this is first implemented).
* Documentation for migrating to the new labels is available in 1.18.

### Upgrade / Downgrade Strategy

As described in the migration process, deployers and administrators have 2 releases to migrate their clusters.

### Version Skew Strategy

Controllers are updated after the control plane, so consumers must update the labels on their nodes before they update controller processes in 1.21.

## Production Readiness Review Questionnaire

### Feature enablement and rollback

* **How can this feature be enabled / disabled in a live cluster?**
  - [ ] Feature gate 
    - Feature gate name: `TimeTableModule`
    - Components depending on the feature gate: `time-table`
  - [ ] Other
    - Describe the mechanism:
    - Will enabling / disabling the feature require downtime of the control
      plane?
    - Will enabling / disabling the feature require downtime or reprovisioning
      of a node?

* **Can the feature be disabled once it has been enabled (i.e. can we rollback
  the enablement)?**

Yes

* **What happens if we reenable the feature if it was previously rolled back?**

The old behavior is present.

* **Are there any tests for feature enablement/disablement?**

Yes

### Rollout, Upgrade and Rollback Planning

Covered in migration strategy.

### Monitoring requirements

* **How can an operator determine if the feature is in use by workloads?**

Not applicable to workloads

* **What are the SLIs (Service Level Indicators) an operator can use to
  determine the health of the service?**

Not applicable

* **What are the reasonable SLOs (Service Level Objectives) for the above SLIs?**

Not applicable

* **Are there any missing metrics that would be useful to have to improve
  observability if this feature?**

No

### Dependencies

* **Does this feature depend on any specific services running in the cluster?**

No

### Scalability

* **Will enabling / using this feature result in any new API calls?**

No

* **Will enabling / using this feature result in introducing new API types?**

No

* **Will enabling / using this feature result in any new calls to cloud
  provider?**

No

* **Will enabling / using this feature result in increasing size or count
  of the existing API objects?**

No

* **Will enabling / using this feature result in increasing time taken by any
  operations covered by [existing SLIs/SLOs][]?**

No

* **Will enabling / using this feature result in non-negligible increase of
  resource usage (CPU, RAM, disk, IO, ...) in any components?**

No

### Troubleshooting

* **How does this feature react if the API server and/or etcd is unavailable?**

Not applicable

* **What are other known failure modes?**

Not applicable

* **What steps should be taken if SLOs are not being met to determine the problem?**

Not applicable

## Implementation History

example:-
  - 2019-07-16: Created
  - 2020-04-15: Labels promoted to beta in 1.19 in https://github.com/kubernetes/kubernetes/pull/90126
  - 2020-06-01: Updated for 1.19 with details of production readiness
  - 2021-01-06: GA in 1.21 and marked to be removed in 1.22

## Future work

This proposal touches on the important topic of scheduling policy - the ability of clusters to restrict where arbitrary workloads may run - by noting that some conformant clusters may reject attempts to schedule onto masters. This is out of scope of this PEP except to indicate that node-role use by ecosystem components may conflict with future enhancements in this area.


## Reference
