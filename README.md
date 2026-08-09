# boxwine-content

와인 테이스팅 노트 앱 **BoxWine**의 학습 콘텐츠 저장소.

앱 APK에는 콘텐츠를 번들하지 않는다. 앱이 실행 중에 이 저장소의 raw URL에서 받아 로컬에
캐시하므로 **앱을 다시 빌드하지 않고도** 항목·이미지를 추가·수정할 수 있다.

## 파일

| 파일 | 용도 |
|---|---|
| `wine_glossary.json` | 학습 콘텐츠 본체. 현재 `_meta.version` **6**, 151개 항목 |
| `images/<카테고리>/*.jpg` | 항목별 헤더 이미지. 현재 **145장** |
| `wine_names_ko.json` | 와인 이름 한글 표기 사전. 1MB급이라 번들하지 않고 원격 전용 |

`wine_glossary.json`의 카테고리 — 품종 34 · 시음 용어 24 · 주요 지역 39 · 향맛 분류 5 ·
와인 스타일 11 · 등급 체계 11 · 양조 기법 3 · 명가 24.

앱이 받아가는 주소:

- `https://raw.githubusercontent.com/binbox01/boxwine-content/main/wine_glossary.json`
- `https://raw.githubusercontent.com/binbox01/boxwine-content/main/wine_names_ko.json`
- 이미지는 JSON의 `image` 상대경로를 위 JSON과 같은 디렉터리의 `images/` 아래에서 찾는다

## 갱신 방법

편집은 **앱 저장소의 `assets/content/`에서** 하고 스크립트로 여기에 밀어 넣는다.
이 저장소를 직접 고치면 다음 동기화 때 덮어써진다(이미지는 `robocopy /MIR`로 미러링).

```powershell
# 앱 저장소 루트에서 실행

# 복사만 하고 변경 내역 확인
powershell -File scripts/sync_content_repo.ps1

# 복사 + 커밋 + 푸시
powershell -File scripts/sync_content_repo.ps1 -Push -Message "content: 품종 3종 추가 (v7)"
```

⚠️ 내용을 바꿨으면 `wine_glossary.json`의 `_meta.version`을 **반드시 +1** 한다.
앱은 이 값이 캐시된 버전보다 클 때만 갱신하므로, 올리지 않으면 바뀐 내용이 반영되지 않는다.

## 이미지 라이선스

실사진은 Wikimedia Commons의 **CC BY · CC BY-SA · CC0 · PD** 자료만 쓰고, 각 항목의
`imageCredit`에 출처·저자·라이선스를 남긴다(현재 135장).

커먼즈에 있을 수 없는 개념도(향·맛 분류 스틸라이프 등)는 AI로 생성하고 `source`를
`AI 생성`으로 밝힌다 — 원본 페이지·라이선스가 없으므로 `url`·`license`는 비운다(현재 10장).
**실존하는 산지·생산자는 AI로 만들지 않는다.** 없는 풍경을 지어내는 것이고 실사진과
구분되지 않기 때문이다.
