---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "VoxelVideo Format"
category: std

docname: draft-habib-voxelvideo-00
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: voxelvideos/voxel-video-file-format
  latest: https://example.com/LATEST

author:
 -
    fullname: Daniel Habib
    organization: QuickVid
    email: daniel@quickvid.ai
    street: 
    - 15 Morris Ave
    - Apt. 307
    city: Long Branch, NJ
    code: '07740'
    country: US
    phone: '+1 908 812 8365'
 -
    fullname: Alyssa Joaquin
    organization: QuickVid
    email: alyssa.joaquin@gmail.com

normative:

informative:
  AES:
    title: >
      A methodology to design a 3D graphic editor for micro-modeling of
      fiber-reinforced composite parts
    author:
      -
        ins: C. I. Shchurova
        name: Catherine I. Shchurova
    date: 2015
    seriesinfo:
      Journal: Advances in Engineering Software, Volume 90, December 2015, Pages 76-82

--- abstract

This document proposes the VoxelVideo format, a file structure designed specifically for the efficient handling, playback, and livestreaming of 3D voxel-based videos. The format is intended for applications in gaming, virtual reality, live sports, and interactive media, providing a robust framework for managing complex 3D data with spatial precision and color fidelity. This document describes the current JSON-based version and outlines future plans to adopt a more efficent, compressed format.

--- middle

# Introduction

The VoxelVideo format addresses the need for an efficient and scalable method to handle, render, and stream 3D voxel-based videos. Existing video formats are not optimized for the distinct characteristics of voxel data, such as spatial precision and color fidelity in three dimensions. The VoxelVideo format is tailored for use in applications like gaming, virtual reality, live sports, and interactive media, where real-time manipulation and playback of complex 3D content are essential. This document describes the JSON-based format and outlines future enhancements, including the adoption of a compressed binary format aimed at improving performance and reducing file sizes.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This document uses the following terms:

- **Voxel**: a three-dimensional pixel representing a point in 3D space with associated attributes such as color [AES].
- **I-frame (Intra-coded frame)**: A self-contained video frame that fully represents the voxel grid.
- **P-frame (Predicted frame)**: A video frame which contains only information on the differences from previous frames.

# VoxelVideo Format Overview

The VoxelVideo format utilizes a JSON-based structure to organize 3D voxel data, facilitating efficient playback and manipulation. It is designed to be straightforward to implement, with a focus on clarity and accessibility in its initial version. Future iterations will shift towards a compressed binary format to enhance scalability and performance.

# File Structure and Key Components

The current structure of the VoxelVideo is as follows:

~~~ typescript
export default interface VoxelVideoData {
  Version: number;        // Specifies the version of the file format
  Framerate: number;      // Frames per second
  Framecount: number;     // Total number of frames
  Duration: number;       // Total duration of the video in seconds
  Dimensions: {           // Dimensions of the voxel matrix
    x: number;            // Width in voxels
    y: number;            // Height in voxels
    z: number;            // Depth in voxels
  };
  Title: string;          // Title of the video
  Blocks: (string | null)[][]; // 3D array representing the voxel grid
}
~~~

## Version

The `Version` field ensures compatibility across different iterations of the format.

## Playback Metadata

Metadata fields like `Framerate`, `Framecount`, and `Duration` provide
essential information for correct playback of the video content.

## Voxel Dimensions

`Dimensions` defines the size of the voxel space, specifying the width, height,
and depth necessary for rendering.

## Title 

The `Title` field serves to identify the content of the video, for
easier cataloging and retrieval.

## Blocks Array

The `Blocks` array represents the 3D voxel grid as a nested array, where each inner array corresponds to a frame of the video. Within each frame, the arrays represent slices of the voxel space at varying depths, providing a structured representation of the 3D voxel data across the entire frame.

# Frame Types and Future Enhancements

Currently, the VoxelVideo format utilizes I-frames, where each frame is a complete voxel grid independent of other frames. Future versions will include P-frames, which will encode changes between frames to reduce file size and improve streaming efficiency in 3D environments.

# Interpretation of the Blocks Array

To interpret the `Blocks` array, read the voxel grid plane by plane, starting at the top-left corner of the first plane (height 0), proceeding row by row from left to right. After completing all rows of a plane, move up to the next plane and repeat the process until all planes are read.

# Potential Use Cases

The VoxelVideo format is suitable for scenarios requiring real-time interaction and manipulation of 3D video content. Use cases include immersive virtual reality environments where live 3D voxel video streaming can deliver spatially precise and dynamic visual data. The format is also applicable in education simulations that require high-fidelity 3D visualizations for teaching concepts in fields such as medicine, engineering, and environmental science. Interactive gaming environments can utilize the VoxelVideo format to represent 3D voxel data, allowing for the creation of fully manipulable virtual worlds that support complex interactions within the game environment.

In additon, the VoxelVideo format may be used in live sports broadcasting to generate 3D replays and visualizations, enabling viewers to observe events from various perspectives and analyze the content interactively. This capability can provide additional insights beyond traditional 2D video formats.

The VoxelVideo format supports livestreaming using Dynamic Adaptive Streaming over HTTP (DASH), similar to its application for 2D videos. This approach allows for efficient and scalable delivery of 3D voxel video content across various devices and network conditions by segmenting videos into smaller parts and encoding each segment at different quality levels.

# Future Directions

The current version of the VoxelVideo format (version 0.0), utilizes a JSON-based structure designed for simplicity and ease of use, facilitating initial development and experimentation. Future iterations will focus on enhancing performance and scalability by transitioning to a compressed binary format, aiming to reduce file sizes and improve data storage and retrieval efficiency. Additionally, future versions will explore the implementation of P-frames to encode changes between frames, further optimizing file size and playback performance. These enhancements are intended to expand the format's capabilities, providing a more robust solution for managing and delivering 3D voxel-based video content at scale.

# Security Considerations

This document does not specifically address security considerations. It is important for implementers of the VoxelVideo format to consider the potential security implications associated with processing untrusted voxel data. Implementers should account for the worst-case scenarios in terms of computational complexity, memory usage, and required processing resources when handling voxel data, especially in contexts like livestreaming where inputs may be dynamic and unverified.

# IANA Considerations

This document has no IANA actions.

--- back
