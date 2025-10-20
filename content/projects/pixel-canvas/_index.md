---
title: Pixel Canvas
date: 2025-09-05
repository: https://github.com/remote-remote/pixel_canvas
---

Pixel Canvas is a real-time collaborative canvas that I've been working on as a learning experiment to better understand Elixir and the OTP platform.
I simply wanted a use case that would make load testing a high volume of throughput with real-time latency requirements. 
Some of the project goals include:
- manual TCP/IP handling
- hand-rolled websocket implementation
- custom binary WS protocol
- targeting 1M messages per second
- custom telemetry infrastructure
