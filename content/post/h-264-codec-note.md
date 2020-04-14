---
title: 'H.264 codec Note'
date: Mon, 19 Nov 2018 05:47:12 +0000
draft: false
tags: ['H264 structure', 'Software Design']
---

• Elements of a video Sequence

o Frames

o Slices

o MBs (macroblocks)

• Frame Types

o I-, P-, B-frames

o [GOP](https://en.wikipedia.org/wiki/Group_of_pictures) (group of picture), specifies the order in which intra\- and inter-frames are arranged.

o NAL (Network Abstraction Layer)

v SPS (Sequence parameter set)

v PPS (Picture parameter  set)

v IDR (Instantaneous Decoder Refresh), every IDR frame is an I-frame, but not vice versa,

• Coding Tools

o Entropy Coding

v CAVLC

v CABAC

o Integer Transform

v DCT

o Quantization

o Inter-Prediction

o Intra-Prediction

v Intra\_4x4, Intra\_8x8, Intra\_16x16

o Deblocking filter

• Profiles and Levels

o Baseline Profile (66)

o Main Profile (77)

o Extended Profile (88)

o High Profile (100)

* * *

详细原文参考：[Springer](https://www.springer.com/cda/content/document/cda_downloaddocument/9781461422297-c1.pdf?SGWID=0-0-45-1338306-p174267072)