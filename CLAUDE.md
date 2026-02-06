# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 사용자 맥락

**역할**: User Growth PM - AI 크리에이터 채널 운영 및 성장 전략 담당

**일상 업무**:
- AI 크리에이터 3-4명 × 3개 플랫폼(YouTube, TikTok, Instagram) = 9-12개 채널 운영
- 매일 영상 업로드 (크리에이터당 15-30분, 플랫폼별 반복 작업)
- 각 스튜디오에서 성과 데이터 수집 → 분석 → Lark 운영일지 작성

**주요 도구**: YouTube Studio, TikTok Studio, Instagram 대시보드, 내부 콘텐츠 관리 툴, Lark

**Pain Points**:
- 같은 영상을 3개 플랫폼에 반복 업로드
- 9-12개 채널 성과를 각각 수동으로 확인
- 숫자 확인 후 콘텐츠별 장단점 분석까지 직접 작성

**스킬 설계 방향**: 반복적인 데이터 정리와 포맷 작성을 자동화하여, 분석과 전략에 집중할 수 있도록 지원

## Repository Purpose

This is a Claude Code skills repository containing custom skill definitions and design documents. Skills are reusable command extensions that automate specific workflows.

**Workshop Context**: This repository was created for a Claude Code workshop on building automation skills for repetitive PM tasks. The workflow follows: interview → design document → SKILL.md → packaged .skill file.

**Existing Skills**:
- `meeting-summary.skill` - Transforms meeting transcripts/notes into structured summaries with participants, key points, decisions, and action items
- `daily-ops-log/` - (In design phase) Generates daily operations logs from multi-channel social media performance data

## Repository Structure

```
my-first-skill/
├── *.skill                    # Packaged skills (zip archives)
├── meeting-summary/           # Extracted skill (working directory)
│   └── SKILL.md              # Skill implementation
├── daily-ops-log/            # Skill design documents
│   ├── daily-ops-log-설계서.md  # Full specification
│   └── 사전설문응답.md           # User interview
└── CLAUDE.md                 # This file
```

## Skill Development Workflow

### 1. Design Phase
Design documents (`*-설계서.md`) are created first, following the workshop format (see "Skill Design Format" below). These are planning documents, not executable skills.

### 2. Implementation Phase
Create `SKILL.md` in a new directory with this structure:
```markdown
---
name: skill-name
description: |
  Brief description that appears in skill listings.
  Can be multiple lines.
---

# Skill Title

[Implementation instructions for Claude Code...]
```

### 3. Packaging
```bash
# Package a skill
cd skill-directory && zip -r ../skill-name.skill SKILL.md

# Extract for editing
unzip skill-name.skill -d skill-name/

# Test locally (skill must be in a directory with SKILL.md)
# Use the skill via /skill-name trigger
```

### 4. Key Points
- **SKILL.md** contains executable instructions for Claude Code
- **설계서.md** contains planning documentation for humans
- Skills use YAML frontmatter for metadata (name, description)
- The skill name in frontmatter determines the trigger (e.g., `/meeting-summary`)

## Skill Design Format

Design documents (`*-설계서.md`) follow this Korean workshop structure:

| Section | Purpose |
|---------|---------|
| **0. 선언** | Name, one-liner, skill type (텍스트 변환/파일 기반/외부 API/다단계), MVP goal |
| **1. 언제 쓰나요?** | Representative use cases, pain points, why it's needed |
| **2. 사용법** | Triggers, output format, examples |
| **3. 입력/출력 명세** | Required vs optional inputs, output rules |
| **4. 범위** | What it does (max 3 things) and what it doesn't do (max 2 things) |
| **5. 데이터/도구/권한** | Data sources, output destinations, external services |
| **6. 실패/예외 처리** | Expected failure scenarios and how to handle them |
| **7. 대화 시나리오** | Normal case + failure case with exact dialogue examples |
| **8. 테스트 & 완료 기준** | Checklist and definition of done |

Design documents typically include:
- Mermaid flowcharts for workflow visualization
- "나중에 더 발전시킬 아이디어" section for future enhancements
- "배포 준비" section with deployment checklist

## Development Commands

### Working with Skills
```bash
# Extract a skill for editing
unzip skill-name.skill -d skill-name/

# Re-package after editing
cd skill-name && zip -r ../skill-name.skill SKILL.md

# View skill contents without extracting
unzip -p skill-name.skill SKILL.md
```

### Testing Skills
- Skills are tested through Claude Code conversations
- Use the trigger defined in the skill's YAML frontmatter (e.g., `/daily-ops-log`)
- Test both normal and failure scenarios from the design document
- Verify output is Lark-compatible markdown

## Creating New Skills

### From Design Document
When converting a `*-설계서.md` into a working skill:

1. **Read the design document** - Understand the scope, inputs/outputs, and scenarios
2. **Create SKILL.md** - Translate the design into Claude Code instructions:
   - Use section 2 (사용법) for triggers in YAML frontmatter
   - Use section 3 (입력/출력 명세) to define input/output handling
   - Use section 7 (대화 시나리오) for conversation flow examples
   - Use section 4 (범위) to set clear boundaries
3. **Test thoroughly** - Verify all scenarios from section 8 (테스트 & 완료 기준)
4. **Package** - Create `.skill` file for distribution

### SKILL.md Template
```markdown
---
name: skill-name
description: |
  [One-liner from design document section 0]
  [Additional context if needed]
---

# [Skill Title]

[Clear instructions for Claude Code on how to handle this skill, including:
- What inputs to request
- How to process them
- What format to output
- How to handle edge cases]
```

## Key Conventions

- **Output Format**: All skills generate Lark-compatible markdown for easy copy-paste
- **MVP Scope**: Skills focus on text transformation; avoid external API dependencies in MVP
- **Language**: Design documents in Korean; SKILL.md can use Korean or English based on user preference
- **Triggers**: Multiple triggers can be defined (e.g., `/daily-ops-log`, "운영일지 작성해줘", "오늘 성과 정리해줘")
- **Error Handling**: Always provide helpful guidance when inputs are missing or incomplete
- **Validation**: Test checklist from design document section 8 must pass before packaging
