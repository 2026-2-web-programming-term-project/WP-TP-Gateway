# WP-TP-Gateway

연구실 GPU 서버 자원 예약 및 실시간 모니터링 시스템의 서비스 라우팅과 공통 요청 처리를 담당할 Spring Cloud Gateway 프로젝트입니다.

[프로젝트 전체 소개](https://github.com/2026-2-web-programming-term-project/.github#readme)

## 담당 범위

- Front와 API 사이의 서비스 라우팅
- 원래 요청의 프로토콜·호스트·요청 추적 정보 전달
- 인증·인가 처리 경계, 시간 제한과 오류 응답 정책 정의
- 인스턴스 증감에 대응하는 서비스 발견·로드밸런싱
- 필요한 실시간 통신 경로의 연결 처리

인증과 예약 권한·충돌 검사는 업무 API에서도 수행합니다. Gateway 정책과 최종 라우트는 아직 구현하지 않았습니다.

## 현재 설정

| 항목 | 값 |
| --- | --- |
| Java toolchain | 25 |
| Spring Boot | 4.0.0 |
| Spring Cloud | 2025.1.4 |
| Gradle Wrapper | 9.8.2 |
| 애플리케이션 이름 | `TP-Gateway` |

2026-11-02 기준 초기 프로젝트와 기본 테스트 클래스가 있습니다. 빌드 파일에는 Gateway WebFlux와 WebMVC starter가 모두 선언되어 있습니다. 사용할 실행 방식을 선택하고 의존성·라우팅 설정과 버전 호환성을 검증할 예정입니다.

## 개발 명령

Java 25 환경에서 Gradle Wrapper를 사용합니다.

```bash
./gradlew bootRun
```

```bash
./gradlew test
```

현재 명령은 초기 프로젝트의 개발 진입점입니다. 실행 방식 선택과 설정 검증 전 정상 기동을 보장하는 절차는 아닙니다. Windows에서는 `gradlew.bat`를 사용합니다.

## HA와 HTTPS 설계 방향

EC2 Auto Scaling 환경에서 Gateway도 복수 인스턴스로 운영하는 방향을 검토합니다. ALB·ACM을 HTTPS 진입점으로 사용하는 경우 외부 TLS 연결은 ALB에서 종료하고, Gateway의 내부 TLS 적용 여부는 네트워크 경계와 보안 요구에 따라 정합니다. 각 서비스 포트를 모두 443으로 통일할 필요는 없습니다.

서비스 발견·내부 로드밸런싱, 신뢰할 프록시와 전달 헤더, readiness, 요청 종료·재시도 정책을 함께 정의합니다. 예약 생성·취소 같은 변경 요청은 중복 처리가 발생하지 않도록 재시도 정책을 검증합니다.

백엔드 제출 마감은 **2026-12-04 23:59 KST**입니다. 현재 AWS 배포·장애 복구·부하 테스트는 수행하지 않았습니다.

## 담당 WBS

[GitHub Projects의 저장소별 WBS](https://github.com/orgs/2026-2-web-programming-term-project/projects/1/views/4)에서 `WP-TP-Gateway` 그룹을 확인합니다.

GW-01–04: 실행 방식·라우팅·전달 정보·다중 인스턴스 연결

선행 관계·기간·완료 기준·상태는 GitHub Projects의 작업 카드에서 갱신합니다. [공통 인프라](https://github.com/orgs/2026-2-web-programming-term-project/projects/1)와 [통합 검증](https://github.com/orgs/2026-2-web-programming-term-project/projects/1)도 함께 확인합니다.

## 라이선스

[Apache License 2.0](LICENSE)
