---
title: Pixel Canvas
date: 2025-08-05
repository: https://github.com/remote-remote/pixel_canvas
image: pixel-canvas.png
summary: Pixel Canvas is a real-time collaborative canvas that I'm building as a learning project to better understand Elixir and the OTP platform. I chose this as a use-case that is easy to understand and test, and has high-throughput low-latency requirements.
---

Pixel Canvas is a real-time collaborative canvas that I'm building as a learning project to better understand Elixir and the OTP platform.
I chose this as a use-case that is easy to understand and test, and has high-throughput low-latency requirements.

## Constraints

Because this is a project meant to help me deeply understand the building blocks of OTP, I decided to approach it with a **no dependencies** approach. This means:

- manual TCP/IP handling
- hand-rolled websocket implementation
- custom binary WS protocol
- custom telemetry infrastructure

## AI Usage

During this process, I've been refining an AI-based learning framework. I wanted to leverage Claude as a mentor to help me along the way, but I set a hard rule that I wouldn't let the LLM write **any** of my application code. The first commit in this project includes a CLAUDE.md that directs the agent to **NEVER** make direct code changes. At a high level, I directed Claude to:

- provide honest, even harsh code reviews and architectural feedback
- discuss architectural options with me without jumping straight to the "correct" answer
- write unit tests based on specs I've settled on, but before I implement (classic TDD flow)

This has been like a more interactive codecrafters.io - having an expert available to explain tradeoffs and catch mistakes in real-time has dramatically accelerated my learning.

## Scaling Goals

I have set some very high goals for this project in an attempt to really stretch out into handling large-scale traffic. The numbers are intentionally unrealistic - I'm simulating worst-case sustained load like what an IoT aggregator might see:

- accept and broadcast 1M pixel changes per second
- each client can send up to 60 messages/second
- this requires the app to handle ~17k concurrent connections all maxing out their rate limits

Since I don't have a bunch of IoT devices, the canvas gives me something people can easily interact with while I stress-test the underlying infrastructure.

## Current Progress

This is a pretty ambitious project that I will probably be woodshedding for a while. As of 2025-10-22, I have accomplished the following:

- a working prototype that handles ~70k messages per second across 500 connections
- simple hand-rolled telemetry infrastructure for measuring performance
- in-project load tests for local testing
- started on a [distributed load tester](./projects/load-tester)

The main bottleneck I've identified is process message buildup - I need to get from 70k to 1M msg/s (14x improvement) by addressing GenServer mailbox congestion.

## What's Next

- connection pooling to reduce number of spawned processes
- broadcast pooling to distribute the message queue load across multiple processes
