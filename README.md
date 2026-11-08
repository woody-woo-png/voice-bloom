# 목소리꽃 · Voice Bloom

목소리의 높낮이·크기·리듬을 읽어 세상에 하나뿐인 꽃나무 그림을 그리는 웹앱.
Live: https://bloom.stockmolt.ai

## 구조
- `index.html` 한 파일에 HTML·CSS·JS가 모두 들어 있음 (서버·빌드 없음)
- `CNAME` GitHub Pages 사용자 도메인 설정 (지우지 말 것)
- 녹음·분석은 모두 브라우저 안에서 처리. 목소리는 어디에도 전송하지 않음

## 주요 기능
| 기능 | 설명 | 코드 위치 (검색어) |
|---|---|---|
| 목소리 분석 | 무음 자르기, 음량 곡선, 음높이(자기상관), 음절 수 | `function analyze` |
| 나무 생성 | 시드 기반 재귀 가지, 꽃 색 8가지 + 보조 색 | `PALETTES`, `function genVoiceTree` |
| 마음 고르기 | 사랑·감사·그리움·응원·미안함 → 꽃말 있는 꽃 모양 | `HEARTS`, `function drawFlower`, `hana:` |
| 한 번에 피우기 | 가족 숲 최대 6그루, 링크 공유 (`#f=` 해시에 나무 모양만 압축) | `compact`, `encodeForest`, `loadFromHash` |
| 매일 키우기 | 하루 한 번 녹음할 때마다 성장, 8번이면 만개. 이 기기 localStorage(`vb.garden`)에만 저장 | `/* ========== 매일 키우기` |
| 한/영 | `T.ko` / `T.en` 사전, `?lang=en` | `const T = {` |

## 고칠 때 주의
- 공유 링크 호환: `compact`/`expand` 필드를 바꾸면 이미 보낸 링크가 깨질 수 있음. 새 필드는 선택 항목으로 추가
- 나무 모양용 rng 호출 순서를 바꾸면 기존 링크의 나무 모양이 달라짐 (새 무작위 값은 별도 rng 사용)
- 화면 그리기는 프레임당 수만 번의 캔버스 호출이 있어 무거움. 자라는 중이 아닐 때는 초당 약 10회만 다시 그림 (`frame.dirty`)

## 테스트 방법
- 문법 검사: `<script>` 부분만 뽑아 `node --check`
- 화면 확인: Chrome headless `--screenshot` (Google Fonts 줄을 뺀 복사본으로 테스트해야 멈추지 않음)
- headless는 시간이 느리게 흘러서 성장 애니메이션 확인 시 `performance.now() / 1000` → `/ 100`으로 바꾼 복사본 사용
- 실제 녹음·저장·카톡 공유는 휴대폰에서 직접 확인

## 배포
`main` 브랜치에 push하면 GitHub Pages가 1~2분 뒤 자동 반영.
