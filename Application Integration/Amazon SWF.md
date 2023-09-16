# Amazon SWF - Simple Workflow Service<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Events Record](#2-events-record)

# 1. Introduction

- The Amazon Simple Workflow Service (Amazon SWF) makes it easy to build applications that coordinate work across distributed components.

# 2. Events Record

- `markers` - To record events in the workflow execution history for application specific purposes.
  - Markers are useful when you want to record custom information to help implement decider logic.
  - For example, you could use a marker to count the number of loops in a recursive workflow.
- `signals` - Just enables you to inject information into a running workflow execution.
- Take note that in this scenario, you are required to record information in the workflow history of a workflow execution.
- `timers` - Just enables you to notify your decider when a certain amount of time has elapsed and does not meet the requirement in this scenario.
- `tags` - Just enables you to filter the listing of the executions when you use the visibility operations, which once again does not meet the requirement in this scenario.