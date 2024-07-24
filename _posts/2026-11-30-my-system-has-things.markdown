---
layout: post
title: "I want a system with things, not data"
date: 2026-11-30 11:00:44 +0200
categories: takes
---

A large portion of all software being written today is done so to power information systems on the internet. By information systems I refer to platforms that one way or another handle administrative work for businesses and organizations, serve popular apps with data and functionality or otherwise.

## What we do every day

Whether it is interfacing with people building web SPAs, native apps for iOS and Android or third parties, most of us are likely spending time building APIs that somehow lets these people read and write information in our system. Some of us might be working with GraphQL, others with gRPC - but quite likely most of us are likely working with regular "JSON over HTTP" (which through a weird turn of events people now call ["REST"](https://htmx.org/essays/how-did-rest-come-to-mean-the-opposite-of-rest/)).

But regardless of communication technology - there are competing patterns for how people like to design their APIs. One prism through which you can choose to view it makes you see two camps: APIs built on things, and APIs built on functions.

## The platonic ideal

I have had this growing itch in the back of my mind that there is something to be said about how we can make large systems easy to understand ever since I worked with Stripes' payment APIs. Back in the late 2010s, Stripe was moving people to a new version of their APIs where each payment (not subscriptions) where handled through something called a [Payment Intent](https://docs.stripe.com/api/payment_intents). Here is the top summary of this model:

> A PaymentIntent guides you through the process of collecting a payment from your customer. We recommend that you create exactly one PaymentIntent for each order or customer session in your system. You can reference the PaymentIntent later to see the history of payment attempts for a particular session.
>
> A PaymentIntent transitions through multiple statuses throughout its lifetime as it interfaces with Stripe.js to perform authentication flows and ultimately creates at most one successful charge.

Making a payment - with all its vast intricacies and hundreds of underlying integration points - are all placed under a single top-level umbrella type. This type has a clear name (I do in fact "intend to pay") and serves as a state machine guiding you to at most one charge.

## Things vs functions

A world built on **functions** often looks like an entire system being built on individual procedures that can be called to interact with it. Some of them read data, some of them manipulate data. The reading operations are often expressed a bit more low level, such as `getShoppingCart`, `getInsurancePolicies` or `getUserDetails`. The writing operations typically tend to be more high level - focusing on the actual user event that triggered it. It could for instance be something like `completePurchase`, `terminateInsurance` or `todoBlorgyFlorg`.

## Why things

## Board game analogy

## Wrap up
