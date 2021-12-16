---
layout: page
title: Projects
---
## Personal Projects

Welcome to my personal projects gallery! Here, you can see some of the things I work on, whether they are for school, just for kicks, or to learn something new. Each one of these will be accompanied by a write-up about what I did, and will be added here. Enjoy!

### [Object Store](https://github.com/avr1/objs)

During the summer of 2021, I began learning more about cloud computing, and how storage works at a larger scale. I learned about the current desktop storage architecture, with files and directories, and understood that an object store allows for scalability and a more refined permissions model.

So, I built a minimalist object store, written in userspace using the Go Programming Language, to store and modify files in an object store. 

With a permissions model built for a data center, yet using the fundamental UNIX users, the project's minimalist, yet useful goals are a good blend for a learning project.

Currently, I'm working on adding a logical backup feature for certain files, allowing for reverts based on the time of backup.

### Layered Image Manipulation Processor

In my Object-Oriented Design class, I designed an image manipulation processer built entirely in Java, with a Swing frontend.

We built the project over 4 weeks, and each week, we pretended that we had published the code. With ever-changing requirements, we had to design interfaces for extensibility, and use common Design Patterns, like Builder, Strategy, Adapter, and Command.

We iterated through designs, working with interfaces to load, export, and transform images, and as we were told more about the project, we added functionality via different implementations.

Code is available upon request, per the course rules.

### Image Compression

Here's an example of a project my team members and I made for school. It's a cool way to compress pictures to different resolutions while being able to remove only the boring pixels using a seam carving algorithm.

<video width="640" height="300" controls>
  <source src="../assets/img/balloons.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

## Group Projects

## As Part of a Course, I worked on the [Log-Structured Virtual disk for Ceph](https://github.com/avr1/lsd_ceph) Project

We started with a research block device, called [BS3](https://github.com/asch/bs3), which delivered next-generation performance by amortizing write-time organization with read-time awareness.

The device came with a Linux Kernel API, which wasn't useful for platforms that needed virtualization (such as QEMU). So, we rearchitected some of the function calls on the block device to support a C-Compatible API, and wrote our own version of Ceph's RBD API to use as an endpoint with QEMU.

Full demos and project descriptions are available on the GitHub link posted above.


## Open Source Projects

### I've done some work with the following projects:

#### [The Servo Project](https://servo.org/)

Servo is a browser engine built in the Rust Programming Language aiming for better parallelism, security, and modularity.

- Optimized the Servo Browser Engine's HTML parser to get an element’s target and noopener in accordance to the specification.

- Updated a dependency to the latest crate of euclid in several different codebases, including lyon, surfman, surfman_api, servo, canvas, canvas_traits, and compositing.

- Added and updated unit tests to work with the CI build.

- Updated relavant crates to support the latest version of macOS.

#### [lxc](https://linuxcontainers.org/lxc/)

LXC is a userspace interface for the Linux kernel containment features that lets Linux users easily create and manage system or application containers.

- Updated documentation to reflect the incompatibility of LXC with pure unfiltered systems like cfgroupsv2

#### [Kubernetes](https://kubernetes.io/)

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

- Updated documentation to remove a broken link to an analytics page.
- Part of the Storage Special Interest Group.
