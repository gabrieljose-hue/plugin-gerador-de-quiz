# Quiz Generator Plugin — RTG

A Claude Code plugin that automatically generates sales quizzes with 15 structured screens, following a proven psychological conversion framework (similar to Inlead quizzes).

## Overview

The plugin orchestrates 3 AI skills:
1. **Data Collector** — collects product info, audience pain points, desires, and testimonials
2. **Quiz Creator** — generates all 15 quiz screens following the 13-stage framework
3. **Quiz Reviewer** — validates conversion quality and psychological flow

## Installation

1. Open Claude Code (claude.ai/code)
2. Import this folder as a project
3. Run `/criar-quiz` in the chat

## Quiz Framework (13 Stages)

| Stage | Screen | Type | Goal |
|-------|--------|------|------|
| 1 | Opening | Presentation | Curiosity + Gender segmentation |
| 2 | Demographics | Single choice | Age profiling with photos |
| 3 | Social Proof | Presentation | Credibility before questions |
| 4 | Diagnosis 1 | Single choice | Reveal problem severity |
| 5 | Lead Capture | Form | Collect name + email |
| 6 | Diagnosis 2 | Single choice | Personalized diagnosis |
| 7 | Diagnosis 3 | Single choice | Additional dimension |
| 8 | Pain Points | Multi-choice | Confirm problem consequences |
| 9 | Main Desire | Multi-choice | Lead declares their goal |
| 10 | Testimonials | Presentation | Proof the problem is solvable |
| 11 | Promise | Presentation | Personalized solution promise |
| 12 | Micro-commitment | Single choice | Motivation level (pre-offer) |
| 13 | Loading | Loading | Anticipation + personalization feel |
| 14 | Result | Presentation | Lead is ready for transformation |
| 15 | Sales Page | Presentation | Full internal sales page |

## Output Files

- `output/dados-quiz.json` — collected data
- `output/quiz.json` — complete quiz structure
- `output/quiz.md` — human-readable implementation guide for Inlead

## Screen Types

- `apresentacao` — presentation screen (no interaction)
- `escolha-unica` — single choice
- `multipla-escolha` — multiple choice
- `captura` — form fields (name, email)
- `carregamento` — loading screen

## Author

RTG Marketing Digital
