---
title: '[Reading Notes] Business process management (BPM) standards: a survey'
date: 2019-04-22 20:42:13
tags: [Reading Notes, BPM, Survey, Business Process Management]
---

# Abstract

## purpose

This paper seeks to make sense of the myriad BPM standards, organizing them in a classification framework, and to identify key industry trends.

## Design/methodology/approach

Proposed BPM Standards Classification Framework to list each standard’s distinct features, strengths and weaknesses.

## Findings

An attempt is made to classify BPM languages, standards and notations into four main groups: execution, interchange, graphical, and diagnosis(lack) standards.

## Practical implications

Researchers and practitioners may wish to position their work around this review.

## Originality/value

No body did before.

## Keywords

Process management, Standards, Work flow

## Paper type

Literature review

# Introduction

## The growth of business process management

Some factors:

- the rise in frequency of goods ordered;
- the need for fast information transfer;
- quick decision making;
- the need to adapt to change in demand;
- more international competitors; and
- demands for shorter cycle times

Software tools supporting the management of such operational processes became known as business process management systems (BPMS).

## The proliferation of BPM languages, standards and software systems

Naturally, interest in BPM from practitioners and researchers grew rapidly.

Many new BPM terminologies and technologies are often not well defined and understood by many practitioners and researchers using them.New languages and notations proposed often contain duplicating features for similar concepts, and loosely claim to be based on theoretical formalisms such as Pi-calculus and Petri nets. Most of them have also not been validated, especially in a real business and office environment.

## Motivation of this paper

This paper’s goal is to leave the reader with some semblance of order out of a disparate collection of specifications, white papers, journal publications, conference publications and workshop notes to be consolidated as a single paper.

- discuss and rationalize the terminologies associated with BPM and its standards;
- systematically categorize/classify BPM standards;
- discuss the current strengths and limitations of each standard;
- clarify, the differences of theoretical underpinnings of prominent BPM standards; and
- explore the gaps of knowledge of current BPM standards and how these may be bridged.

# BPM basics

## The BPM life cycle

|Term|Explanation|
|:--|:--|
|Process design|In this stage, fax- or paper-based as-is business processes are electronically modeled into BPMS. Graphical standards are dominant in this stage.|
|System configuration|This stage configures the BPMS and the underlying system infrastructure. This stage is hard to standardize due to the differing IT architectures of different enterprises.|
|Process enactment|Electronically modeled business processes are deployed in BPMS engines. Execution standards dominate this stage.|
|Diagnosis|Given appropriate analysis and monitoring tools, the BPM analyst can identify and improve on bottlenecks and potential fraudulent loopholes in the business processes. The tools to do this are embodied in diagnosis standards.|

## BPM vs BPR vs WfM

- BPM: Business Process Management
- BPR: Business Process Reengineering
- WfM: Workflow Management

### BPM vs BPR

BPR calls for a radical obliteration of existing business processes, its descendant BPM is more practical, iterative and incremental in fine-tuning business processes.

### BPM vs WfM

- One viewpoint by Gartner research views BPM as a management discipline with WfM supporting it as a technology. 
- Another viewpoint from academics is that the features stated in WfM according to Georgakopoulos et al. is a subset of BPM defined by van der Aalst et al., with the diagnosis stage of the BPM life cycle as the main difference.

## BPM theory vs BPM standards and languages vs BPMS

BPMS/BPMSs: Business Process Management Suites

![](https://raw.githubusercontent.com/imonce/imgs/master/20190508215035.png)

## BPM vs service oriented architecture

SOA: Service Oriented Architecture

BPM is a process-oriented management discipline aided by IT while SOA is an IT architectural paradigm. 

According to Gartner (Hill et al., 2006), BPM “organizes people for greater agility” while SOA “organizes technology for greater agility”.

![](https://raw.githubusercontent.com/imonce/imgs/master/20190508215950.png)

# Categorising the BPM standards

B2B: business-to-business

- Graphical standards. This allows users to express business processes and their possible flows and transitions in a diagrammatic way.
- Execution standards. It computerizes the deployment and automation of business processes.
- Interchange standards. It facilitates portability of data, e.g. the portability of business process designs in different graphical standards across BPMS; different execution standards across disparate BPMS, and the context-less translation of graphical standards to execution standards and vice versa.
- Diagnosis standards. It provides administrative and monitoring (such as runtime and post-modeling) capabilities. These standards can identify bottlenecks, audit and query real-time the business processes in a company.

![](https://raw.githubusercontent.com/imonce/imgs/master/20190508222437.png)

![](https://raw.githubusercontent.com/imonce/imgs/master/20190508222557.png)

