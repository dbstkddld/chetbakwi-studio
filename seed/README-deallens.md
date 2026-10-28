# DealLens

Android/iPhone용 가격표 OCR·정확한 단위가격 계산 앱. 현재는 Android 실기기에서 검증 중인 개발 프로토타입이다. 진행 상황과 재개 지점은 [PROGRESS.md](PROGRESS.md), 제품 명세는 [docs/PRODUCT_SPEC.md](docs/PRODUCT_SPEC.md)를 읽는다.

## 현재 동작

- 한국어 ML Kit로 기기 안에서 가격표 후보 인식. 원본 사진·OCR 글자는 저장하지 않는다.
- 같은 후보의 3프레임 안정화, 1초 만료, 초기화/정지 세대 분리.
- 인식은 제안만 표시한다. 사용자가 최종 총 결제액·낱개 용량·총 받는 수량·적용 조건을 확인한 후 유리수로 단가 계산.
- 스캔·비교·기록 하단 메뉴, 가격 의미·행사·조건 편집, 최대 4개 비교와 명시 확인 후 최저 단가·동률 표시.
- SQLite로 명시 추가한 비교와 저장한 가격을 복원한다. 기록 상세·이전 가격 차이·삭제 실행취소·전체 삭제를 제공한다. 저장 실패 시 미저장 상태를 표시하며 손상된 원본을 자동 초기화하지 않는다.
- 기록 화면의 테스트 광고·동의 코드는 기본 비활성이다(`DEALLENS_TEST_ADS=false`). 실광고·결제·Jev·개인 구매 추천은 활성화하지 않았다.
- 0.2.1+3 Android 출시 전 검증 빌드. 실제 가격표 검증은 사용자 지시로 연기했으며 출시 준비 항목은 [출시 점검표](docs/RELEASE_CHECKLIST.md)를 따른다.

## 실행 검사

Flutter 3.47.4 / Dart 3.13.3 기준. 의존성은 `pubspec.lock`으로 고정한다.

```powershell
flutter pub get
dart analyze lib test
dart test/pricing_check.dart
dart test/tag_parser_check.dart
flutter test
```

Windows의 한글 프로젝트 경로는 Android Gradle에서 거절된다. `scripts/build_android.ps1`은 공용 SDK를 수정하지 않고 pub cache와 같은 드라이브의 새 영문 snapshot으로 빌드한다. 실행 후 출력된 APK 경로를 사용한다.

```powershell
./scripts/build_android.ps1 -Flutter D:/dev/flutter/bin/flutter.bat
```

연결된 다른 프로젝트의 에뮬레이터를 건드리지 않도록 설치/실행은 항상 실제 테스트폰 serial을 `adb -s`에 지정한다. 공개 배포용 서명·패키지 ID 확정은 별도 작업이다.

## 검증 경계

실제 매장 가격표 정확도·영역 좌표·최초 오프라인·15분 지속 사용·접힘/회전은 별도 평가가 필요하다. 한국어 모델의 APK 포함과 OCR 호출 성공만으로 정확도 목표를 통과했다고 보지 않는다.

iOS 15.5 최소 설정과 Korean CocoaPods 의존성을 구성했다. Windows 환경이므로 CocoaPods 해결·Xcode 빌드·iPhone 실행은 미검증이다.

## 최신 출시 전 검증

자동검사147개와 4KB 에뮬레이터 핵심 흐름을 통과했지만, **16KB 네이티브 RELRO5건 실패로 공개 출시를 차단**한다. [QA 결과](docs/PRELAUNCH_QA.md), [호환성 감사](docs/ANDROID_16KB_AUDIT.md)를 먼저 확인한다.

서명 없는 최적화 산출물 검증: `./scripts/build_android.ps1 -Mode Release -Artifact Appbundle -Unsigned`. 공개 서명은 `docs/RELEASE_NATIVE_AUDIT.md`의 네 가지 환경변수로 별도 제공한다. debug키 자동 대체는 허용하지 않는다.
