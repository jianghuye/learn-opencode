---
layout: home
title: OpenCode Tutorial
titleTemplate: AI Coding Assistant Practical Guide
description: OpenCode Official Tutorial. From zero to hero, learn how to use AI coding assistants to write code, fix bugs, and automate work. Supports Claude, OpenAI, DeepSeek and more.

hero:
  name: OpenCode
  text: Practical Guide
  tagline: Completely Free · Learn to Use AI for Real Work from Scratch
  actions:
    - theme: brand
      text: Get Started
      link: /en/1-start/
    - theme: alt
      text: Scenarios
      link: /en/4-scenarios/
    - theme: alt
      text: Community
      link: /en/community

features:
  - icon:
      src: /icons/rocket.svg
    title: Quick Start in 30 Mins
    details: One-click installation, network config, model connection. Complete your first conversation in 30 minutes.
    link: /en/1-start/
    linkText: Install Now
  - icon:
      src: /icons/globe.svg
    title: Global Model Support
    details: Native support for Claude, OpenAI, DeepSeek, Ollama and more.
    link: /en/1-start/04-connect
    linkText: Connect Models
  - icon:
      src: /icons/heart.svg
    title: Free & Open Source
    details: Tutorials and tools are open source. No barriers to learning. Contributions welcome.
    link: https://github.com/vbgate/learn-opencode
    linkText: GitHub Repo
  - icon:
      src: /icons/code.svg
    title: Source Code Analysis
    details: Not made-up tutorials. Every feature is analyzed based on official OpenCode source code.
  - icon:
      src: /icons/files.svg
    title: Multi-file Context
    details: Read the entire project at once. Understand code relationships. No more feeding files one by one.
    link: /en/3-workflow/03-init
    linkText: Project Init
  - icon:
      src: /icons/wrench.svg
    title: Deep Customization
    details: Custom Agents, Skills, Commands, Themes. Build your exclusive AI tool.
    link: /en/5-advanced/
    linkText: Advanced Manual
---

## Choose Your Path

<div class="learning-paths">
  <a href="/en/1-start/" class="path-card">
    <span class="path-icon">🚀</span>
    <h3>Complete Beginner</h3>
    <p class="path-duration">~3 Hours</p>
    <p class="path-desc">Complete required courses (Stages 1-3). Master OpenCode core usage.</p>
    <span class="path-cta">Start Learning →</span>
  </a>
  <a href="/en/4-scenarios/writer-workflow" class="path-card">
    <span class="path-icon">✍️</span>
    <h3>Content Creator</h3>
    <p class="path-duration">~6-7 Hours</p>
    <p class="path-desc">Required + Content Creation Line. Build efficient content production lines.</p>
    <span class="path-cta">Start Learning →</span>
  </a>
  <a href="/en/4-scenarios/coder-daily" class="path-card">
    <span class="path-icon">💻</span>
    <h3>Developer</h3>
    <p class="path-duration">~5-6 Hours</p>
    <p class="path-desc">Required + Dev Line. Boost development efficiency with AI.</p>
    <span class="path-cta">Start Learning →</span>
  </a>
  <a href="/en/4-scenarios/office-files" class="path-card">
    <span class="path-icon">📊</span>
    <h3>Productivity</h3>
    <p class="path-duration">~4-5 Hours</p>
    <p class="path-desc">Required + Efficiency Boost. Let AI handle the chores.</p>
    <span class="path-cta">Start Learning →</span>
  </a>
</div>

## What You Get

<div class="stats-grid">
  <div class="stat-item">
    <span class="stat-number">{{ (stats.wordCount / 10000).toFixed(1) }}w</span>
    <span class="stat-label">Words</span>
    <span class="stat-desc">In-depth details</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">{{ stats.notesCount }}</span>
    <span class="stat-label">Visual Notes</span>
    <span class="stat-desc">Key points in one image</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">18</span>
    <span class="stat-label">Projects</span>
    <span class="stat-desc">Hands-on practice</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">20+</span>
    <span class="stat-label">Prompts</span>
    <span class="stat-desc">Copy & Paste templates</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">800+</span>
    <span class="stat-label">Examples</span>
    <span class="stat-desc">Step-by-step guides</span>
  </div>
</div>

<script setup>
import stats from '../data/stats.json'
</script>

## Ready?

<div class="final-cta">
  <p class="cta-highlight">All content is FREE</p>
  <p class="cta-subtitle">Just need a computer and 30 minutes</p>
  <a href="/en/1-start/" class="cta-button">Start Now</a>
</div>
