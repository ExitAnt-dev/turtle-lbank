# ExitAnt LBank Turtle — Render Blueprint

**운영자 실계정 검증 중입니다. 완료 안내 전에는 고객 배포용으로 사용하지 마세요.**

이 저장소에는 공개 배포 템플릿만 두며 거래 봇 소스와 파트너 인증 비밀키를 포함하지 않습니다.
출시 이미지가 준비된 후 Render → New → Blueprint에서 `ExitAnt-dev/turtle-lbank`의
`main` 브랜치를 연결하고 루트 `render.yaml`을 선택합니다.

Docker Hub의 `exitant/exitant-lbank-turtle:latest`를 사용하는 단일 Background Worker입니다.
기존 OKX 템플릿과 같은 Oregon / Starter 구성을 사용합니다.
SQLite 주문·설정 상태를 재배포 후에도 보존하기 위해 `/data`에 1GB 영구 디스크가 필요합니다.
컴퓨팅과 디스크 비용은 Render 생성 화면에서 확인하세요.

사용자별 `API_KEY`, `SECRET_KEY`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`를
Render 보안 환경변수에 입력합니다. LBank는 OKX의 PASSPHRASE를 사용하지 않습니다.
본인의 거래 키로 직접 레퍼럴을 검증하므로 별도 파트너 키는 입력하지 않습니다.

생성 시 입력하는 값은 위 네 가지뿐입니다(OKX 템플릿과 동일). 실행 모드와 상태 파일 경로는 이미지에 내장돼 있어 따로 설정하지 않습니다.
프로그램 시작 후 Telegram 개인 채팅에서 인증·설정·시작을 완료해야 합니다.
OKX의 현재 활성 전략인 채널 청산, 4분할 진입, 보관/재투입 규칙을 구현했습니다.
실계정 주문·취소·손절·이체 검증은 운영자가 진행 중입니다. 완료 안내 전에는 소액 시드로만 사용하세요.

이미지 태그를 업데이트한 것만으로 Render가 자동 재배포하지는 않습니다.
출시 후 업데이트는 Render에서 최신 이미지로 수동 배포하거나 해당 서비스의 배포 훅을 사용합니다.
Blueprint는 서비스 생성/설정 경로이며 배포 훅이 최초 Blueprint 연결을 대신하지 않습니다.

공식 문서: [Render Blueprint](https://render.com/docs/blueprint-spec),
[이미지 배포](https://render.com/docs/deploying-an-image).
