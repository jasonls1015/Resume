# API Documentation: Disambiguating Status Codes Case Study

## Overview

The Intellicheck API returns HTTP status codes for every transaction, successful or not, and separately, one endpoint returns Capture journey codes describing what happened **during** a transaction (for example, an attempt falling back from automatic to manual capture mode). Two related but distinct concepts, both called "status codes" in casual conversation, and a recurring source of confusion for external developers reading the docs and for an internal RAG support tool trying to answer questions about them.

This case study covers how the documentation, and the retrieval logic built on top of it, evolved to make that distinction unambiguous.

## Challenge

Developers integrating against the API would ask variations of the same question: "What are the possible status codes for this endpoint?" That question is ambiguous in at least two ways:

- Do they mean the HTTP status codes returned by the endpoint itself, covering both successful and failed requests?
- Do they mean the Capture journey codes, returned by a separate endpoint, describing what happened mid-transaction?

Neither side of that ambiguity was well documented to begin with. HTTP status codes for successful transactions weren't documented at all, despite the fact that a "successful" response varies meaningfully depending on which transaction type was requested. Error-side HTTP status codes existed only as scattered examples, not as a structured reference. And the journey codes lived in a completely separate part of the docs, with no explicit signal to the reader (or to an automated tool) that "status" meant something different there than it did anywhere else.

## Solution

The fix had two parts:

- **Documentation content.** I documented the successful HTTP response codes first, with worked examples for each transaction type, since the shape of a "successful" response isn't uniform across the API and that variation was undocumented. I documented the HTTP error codes second, giving every error condition a clear description of when it occurs, rather than leaving readers to infer it from a bare code and message. The Capture journey codes were kept in their own clearly scoped reference table, with an explicit note distinguishing them from HTTP status entirely, so "what did the transaction do" stops competing with "what was the HTTP result" for the same mental category.

- **Retrieval logic.** For the internal support tool built on top of this content, the fix went further: the word "status" alone wasn't a reliable enough signal to route a question correctly. Detecting whether "endpoint" appeared near "status" in the question, and adjusting how the underlying content was chunked so table headers stayed attached to their rows, made the difference between a confident, correct answer and a confused non-answer.

## Outcome

Developer questions that previously required a support escalation ("what does this response mean?") became self-service, both through the docs and through automated Q&A. Successful responses, previously undocumented, now have a clear reference by transaction type. The distinction between HTTP status and journey codes became a documented pattern rather than something a developer had to reconstruct by reading enough examples to notice it. Building out that many worked examples also surfaced a genuine rendering bug in the documentation platform itself, reported and fixed by the vendor within days.

> **See it live.**
> 
> The Intellicheck API reference is active and publicly accessible. You can view a live page here:
> 
> - [Start endpoint](https://idn-direct-api.readme.io/v0.0.1/reference/mvp_start_api_v1_start_post).

## Conclusion

The lesson generalizes to any API: when the same word means two different things depending on which part of a system you're looking at, that ambiguity doesn't resolve itself just because a human reader can usually figure it out from context. It has to be named and structured explicitly, and that requirement gets stricter, not looser, once something other than a human is doing the reading.