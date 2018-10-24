# Securing the Docker Host

## Introduction

A short while ago we published a blog on best practices for Docker security called [Docker Security Best Practices: Part 1](https://anchore.com/blog/docker-security-best-practices-part-1/). We broke this down by briefly discussing a comprehensove approach to security the entire container stack from top to bottom. This involves securing the underlying host operating system, the container images themselves, and the container runtime. In this post, we will discuss securing the host operating system in a bit more detail. In short, containerized applications are only as secure as the underlying host. There are some important operating system security best approaches that will strength this layer of the container stack.