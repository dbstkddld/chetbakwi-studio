# paycheck-app (가칭)

알바·단시간 근로자가 자기 급여를 직접 계산하고, 회사가 준 **급여명세서**·**근로계약서**와 대조하는
Flutter 앱 (Android / iOS, 개발용 Windows 데스크톱 타깃 포함).

**원칙**: 서버 없음 · 로그인 없음 · 모든 데이터는 기기 안 SQLite에 저장 · 명세서 문자 인식도
온디바이스. 앱이 밖으로 보내는 것은 사용자가 직접 누른 백업·공유와 광고 요청뿐이다. 휴대폰 자체 백업은
OS가 사용자 계정에 보관한다 — Android는 종단 간 암호화될 때 DB만(사진·설정 제외), iOS는 iCloud 백업
기본값(`docs/security-review.md` S-06).

## 빠른 시작

```bash
flutter pub get                                      # 의존성 (packages/paycalc 는 path 의존)
flutter test && (cd packages/paycalc && dart test)   # 앱 77 + 엔진 55
flutter run -d windows --dart-define=DEMO=true       # 데모 데이터로 실행(별도 DB, 광고 꺼짐)
```

`lib/db.dart`를 고쳤다면 `dart run build_runner build --delete-conflicting-outputs`로 drift 코드젠을
다시 돌린다.

## 문서

| 문서 | 내용 |
|---|---|
| [PROGRESS.md](PROGRESS.md) | 진행 단계·할 일·결정·사건 기록 — **관리 파일은 여기 하나** |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | 구조·데이터 모델·계산 규칙·테스트 지도·릴리즈 체크리스트 |
| [docs/RESEARCH.md](docs/RESEARCH.md) | 조사 결과·기능 목록·출시 요건 |
| [docs/evidence-research.md](docs/evidence-research.md) | 근무 증거 기능 설계 근거(판례·기술 옵션) |
| [packages/paycalc/RATES.md](packages/paycalc/RATES.md) | 요율·간이세액표 출처와 검산 |
| [docs/security-review.md](docs/security-review.md) | 보안 검토 *(작성 중)* |
| [docs/optimization.md](docs/optimization.md) | 코드·DB 최적화 *(작성 중)* |
| [docs/competitive-analysis.md](docs/competitive-analysis.md) | 경쟁 앱 조사·차별점 *(작성 중)* |

## 라이선스

**미정.** 라이선스 파일을 두기 전까지 저작권은 저자에게 있으며 별도 이용 허락은 없다.
