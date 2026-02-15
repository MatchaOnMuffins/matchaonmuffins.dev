---
title: A GPT Inside a Prime Number
description: A fully functional GPT model whose source code is a 3600-digit prime.
pubDate: 2026-02-14
image: /blog/femtogpt/femtoGPT.jpeg
featured: true
---

![femtoGPT](/blog/femtogpt/femtoGPT.jpeg)

This is a 3600 digit prime number. It's also a full GPT model.

You can convert this prime number to bytes, unzip it, and run a fully functional GPT model from the Python code in it. No dependencies, no imports beyond the standard library. Just a number.

I call it femtoGPT. A fully contained GPT model that's smaller than anything out there, has no dependencies, and whose source code is a prime number.

## Background

A few days ago (February 11 - 13, 2026), Andrej Karpathy released [nanoGPT](https://github.com/karpathy/nanoGPT), a fully contained GPT model at 243 lines of python and Kuber Mehta followed with [picoGPT](https://github.com/kuberwastaken/picogpt/) at 84. I golfed it down to 53, then encoded the whole thing as a prime number inspired by the [DeCSS](https://en.wikipedia.org/wiki/DeCSS) saga.

## The DeCSS prime

In 1999, a teenager named [Jon Lech Johansen](https://en.wikipedia.org/wiki/Jon_Lech_Johansen) wrote DeCSS, a program that could decrypt the Content Scramble System (CSS) used to protect DVDs. The movie industry sued, and courts ordered the code suppressed. In response, people started distributing it in every format imaginable: on T-shirts, in haiku, as a gallery of steganographic images.

In 2001, Phil Carmody found a [prime number](https://en.wikipedia.org/wiki/Illegal_prime) that, when converted to bytes, produced a gzip file containing the DeCSS source code. The point was part mathematical curiosity, part protest — you can't ban a number, and a prime number is about as pure a mathematical object as you can get. It was a statement about the absurdity of treating code as illegal speech.

I thought it would be fun to do the same thing with a GPT.

## How it works

The process is straightforward:

1. Take the 53-line Python script and gzip it.

2. Interpret the compressed bytes as a big integer.

3. Search upward from that integer until you hit a probable prime. By the [Prime Number Theorem](https://en.wikipedia.org/wiki/Prime_number_theorem), the average gap between consecutive primes near $N$ is approximately $\ln(N)$. For a 3600-digit number, $\ln(N) \approx 3600 \cdot \ln(10) \approx 8{,}300$, so you only need to check a few thousand candidates. Takes seconds.

The primality testing uses [gmpy2](https://gmpy2.readthedocs.io/)'s `is_prime`, which runs [Miller-Rabin](https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test) with 200 rounds of witnesses. That makes the probability of a false positive vanishingly small (on the order of $4^{-200}$), but it's still a probabilistic test. Proving primality deterministically for a number this large is a much harder problem, so technically this is a [probable prime](https://en.wikipedia.org/wiki/Probable_prime). For all practical purposes, it's prime.

The key insight is that searching upward from the gzip output changes some trailing bytes — but gzip is tolerant of trailing garbage. The gzip format stores the original data length and a CRC32 checksum in a footer, and decompressors stop reading after the valid stream ends. The extra bytes introduced by bumping the integer up to a prime just sit past the end of the stream and get ignored.

To run it, you can simply reverse the process. Convert the prime to bytes, decompress, and execute the code.

## What's inside

The 3,600 digit prime includes training, inference, an autograd engine, multi-head attention, a feed-forward network with GeLU, and AdamW. All without external dependencies.

The repo is at [github.com/MatchaOnMuffins/femtoGPT](https://github.com/MatchaOnMuffins/femtoGPT).
