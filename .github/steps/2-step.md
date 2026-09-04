## 2단계 · 병합 전에 막는다

알림을 사후에 처리하는 것보다, 애초에 들어오지 못하게 하는 것이 싸게 먹힙니다.

### 할 일

`.github/workflows/dependency-review.yml` 을 만드세요.

```yaml
name: Dependency review

on:
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: actions/dependency-review-action@v4
        with:
          fail-on-severity: high
          comment-summary-in-pr: always
```

### 왜 이렇게 하나

`fail-on-severity` 가 이 랩의 핵심입니다.
임계값을 `low` 로 잡으면 PR 이 계속 막혀서 팀이 검사를 꺼버립니다.
`critical` 로만 잡으면 사실상 아무것도 안 막습니다.

Dependency Review 는 **PR 에서 새로 추가되는 의존성**만 봅니다.
이미 들어와 있는 취약점은 Dependabot 알림이 담당합니다. 둘은 역할이 다릅니다.
