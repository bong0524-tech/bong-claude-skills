# bong-marketplace

팀 공용 Claude Code 스킬 마켓플레이스. nori-hrm 및 이후 프로젝트에서 공통으로 사용한다.

## 수록 플러그인

### view-memory

프로젝트의 기존 프론트엔드(CSS/JS/템플릿)를 스캔해 **디자인 토큰·공통 컴포넌트 패턴·파일 규칙**을 프로젝트 메모리에 증류해 둔다. 매 세션 수십 개 스타일시트를 다시 읽지 않고도 새 화면을 기존 디자인과 일관되게 만들기 위한 스킬.

- 스킬 자체에는 특정 프로젝트 값이 전혀 없다 → 어떤 프로젝트/스택에 넣어도 실행 시점에 그 프로젝트를 학습한다 (이식성).
- **지문(fingerprint) 게이트**: 프론트엔드가 실제로 바뀌었을 때만 재스캔. 안 바뀌었으면 메모리 재사용 = 토큰 절감.
- 모드: 기본(지문 자동감지) / `refresh`(강제 재스캔).

이 repo는 스킬의 **정본 보관소**다. 사용은 각 프로젝트의 `.claude/skills/`로 **복사**해서 한다 (plugin install로 꺼내 쓰지 않는다).

## 사용 (프로젝트에 복사)

스킬을 쓸 프로젝트의 `.claude/skills/`로 폴더를 복사한다:

```
cp -r plugins/view-memory/skills/view-memory <project>/.claude/skills/view-memory
```

복사 후 그 프로젝트에서 `/view-memory` 실행. 스킬이 업데이트되면 같은 명령으로 다시 복사해 덮어쓴다.

## (선택) plugin install로 쓰는 방법

복사 대신 Claude Code 플러그인으로 설치해 쓰는 것도 가능하다. 이 팀은 복사 방식을 쓰지만, 참고용으로 남긴다:

```
/plugin marketplace add https://github.com/bong0524-tech/bong-claude-skills.git
/plugin install view-memory@bong-marketplace
```

## 구조

```
bong-claude-skills/
├─ .claude-plugin/
│  └─ marketplace.json          # 마켓플레이스 + 플러그인 목록
└─ plugins/
   └─ view-memory/
      ├─ .claude-plugin/plugin.json
      └─ skills/view-memory/SKILL.md
```
