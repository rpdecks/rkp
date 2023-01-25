---
title: "Honeybadger setup for React App"
date: 2020-09-24T08:58:52-04:00
author: Robert Phillips
image: "images/blog/ask.jpg"
bg_image: "images/featue-bg.jpg"
categories: ["Devops, React"]
tags: ["Technology", "Tutorial", "React"]
description: "Honeybadger configuration's revision"
draft: true
type: "post"
---

# Brainstorm:

1. Honeybadger basic intro
1. Our need
1. Our config (React App deployed on Heroku)
1. Our problem (how to get REACT_APP config var set)
1. Our way (Github action)
1. Thoughts on friction (must be a better way??)
1. Call for help?

Environments and versions:
https://docs.honeybadger.io/lib/javascript/guides/environments-and-versions/

Recently my team was encouraged to utilze Honeybadger.io as a tool for
monitoring our production app for real-time exceptions and errors (among other things). We want to see the errors our users have and fix them. Honeybadger has a good reputation, so we reach for it and got started.

The focus of this article is to present, at a very basic level, the recommended configuration and a particular source of friction we got through in hopes to either help others or get feedback on a better way to do this.

Our stack:
Ruby on Rails / React App

Primary issue:
Honeybadger.configure needs a revision number to link the error to the uploaded app source map. Quite reasonably, the documentation suggests using the build's commit hash as the revision number. This is unique enough that Honeybadger can index using it.
