# Weverse 1080p URL 추출기

Weverse `playInfo` 응답(XML/JSON)을 붙여넣으면 1080p mp4 주소를 뽑아주는 한 페이지짜리 정적 도구입니다. 모든 처리는 브라우저 안에서만 일어나고, 입력 데이터는 외부로 전송되지 않습니다.

> 이 도구는 **네트워크 요청을 가로채거나 서명을 생성하지 않습니다.** 사용자가 브라우저 개발자도구에서 직접 복사한 응답 텍스트를 파싱하기만 합니다.

## 사용법

1. Weverse 영상 페이지에서 영상을 재생
2. `F12` → **Network** 탭 → 필터창에 `playInfo` 입력
3. 목록에서 **Type이 `xhr`인 행**을 선택 (아래 주의 참고)
4. 그 행을 우클릭 → **Copy** → `Copy response`
5. 페이지의 입력칸에 붙여넣고 **URL 추출** 클릭
6. 나온 카드에서 복사 버튼으로 1080p mp4 주소 복사

응답에 본편(`playback`)과 오디오 필터 버전(`enhanceModePlaybacks`)이 모두 1080p로 들어있으면 각각 따로 표시됩니다.

### 주의할 점

- **preflight 행은 고르지 마세요.** `playInfo`로 필터하면 같은 요청이 여러 줄 뜨는데, Type이 `preflight`인 행은 CORS 사전 요청이라 응답 본문이 비어 있습니다. Type이 `xhr`인 행을 골라야 합니다. 헷갈리면 둘 다 Copy response 해보고 `<MPD`가 들어간 쪽을 쓰면 됩니다.
- **영상을 재생해야 요청이 발생합니다.** 페이지 진입만으로는 `playInfo`가 안 잡힐 수 있습니다.
- **서명은 곧 만료됩니다.** 응답 XML 안의 `expireTime` 시각이 지나면 URL 끝의 `_lsu_sa_` 서명이 무효가 되어 더 이상 동작하지 않습니다. 만료되면 응답을 새로 복사해 다시 붙여넣으세요.

## GitHub Pages 배포

1. 이 폴더 내용을 새 GitHub 리포지토리에 올립니다.

   ```bash
   git init
   git add .
   git commit -m "Add Weverse 1080p extractor"
   git branch -M main
   git remote add origin https://github.com/<사용자명>/<리포명>.git
   git push -u origin main
   ```

2. GitHub 리포 → **Settings** → **Pages**
3. **Source**를 `Deploy from a branch`, 브랜치를 `main` / `/ (root)`로 지정 후 저장
4. 잠시 후 `https://<사용자명>.github.io/<리포명>/` 에서 접속

`index.html`이 루트에 있으므로 별도 빌드 설정 없이 그대로 서빙됩니다.

## 파일 구조

```
.
├── index.html   # 추출기 + 사용 가이드 (단일 파일, 의존성 없음)
└── README.md
```
