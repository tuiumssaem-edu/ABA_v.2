🧠 ABA v.2 - 행동분석 관찰기록 & AI 코칭 시스템

특수교육 및 응용행동분석(ABA) 기반 실시간 표적행동 트래커 & Gemini 지능형 중재 리포트 웹앱

GitHub Repository: tuiumssaem-edu/ABA_v.2

✨ 주요 기능

원터치 3점 척도 관찰 기록

학생명 및 등록된 표적행동 드롭다운 연동 선택

행동 관측 즉시 [약 (1점)], [중 (2점)], [강 (3점)] 대형 버튼으로 시·분·초 타임스탬프 기록

관찰 집중을 돕는 가벼운 비프음 사운드 피드백(Tone.js) 및 토스트 알림

구글 스프레드시트 자동 연동 (학생별 시트 자동 분리)

구글 앱스 스크립트(GAS) Webhook 연동 지원

데이터 기록 시 학생 이름을 가진 탭(Sheet)을 자동 감지 및 신규 생성

표준 헤더 자동 작성: A1: 학생명, B1: 표적행동, C1: 타임스탬프, D1: 강도

시간대별(00시~23시) 빈도 및 강도 시각화

하루 중 행동이 집중되는 피크 시간대(Peak Hours) 막대그래프 분석

시간대별 평균 강도(折선) 및 강도 3단계 도넛 차트 제공

전체 학생 및 특정 학생 필터링

Gemini AI 행동분석 & 상담 코칭 리포트

누적된 타임스탬프 및 강도 통계를 기반으로 맞춤형 임상 리포트 자동 생성

분석 항목: 행동 패턴 심층 진단(선행요인 가설), 3점 척도 위험도 평가, 개별화 행동중재계획(BIP), 학부모 상담 스크립트

우측 상단 설정창(⚙️)에서 개인 Gemini API Key 손쉬운 등록 지원

데이터 유실 방지 & 내보내기

한글 깨짐 방지 UTF-8 BOM 적용 CSV 즉시 다운로드 지원

리포트 원클릭 복사 및 인쇄/PDF 저장 지원

📁 저장소 권장 파일 구조

저장소 루트에 index.html로 배치하면 GitHub Pages를 통해 즉시 무료 웹 호스팅이 가능합니다.

ABA_v.2/
├── index.html       # 웹앱 단일 실행 파일 (behavior_tracker.html의 이름을 index.html로 변경)
└── README.md        # 프로젝트 안내 문서


🚀 GitHub Pages 웹 배포 방법 (무료 호스팅)

이 저장소(tuiumssaem-edu/ABA_v.2)에 index.html 파일을 업로드합니다.

깃허브 저장소 상단의 [Settings] 탭으로 이동합니다.

좌측 사이드바에서 [Pages] 메뉴를 클릭합니다.

Build and deployment 항목의 Branch 설정을 main (또는 master) 브랜치, 폴더는 / (root)로 지정하고 [Save]를 클릭합니다.

1~2분 후 상단에 생성되는 웹 주소(https://tuiumssaem-edu.github.io/ABA_v.2/)로 접속하여 사용합니다.

📊 구글 스프레드시트 백엔드 연동 가이드

새 Google 스프레드시트를 만듭니다.

상단 메뉴 [확장 프로그램] → [Apps Script]를 클릭합니다.

기존 코드를 모두 지우고 아래 코드를 붙여넣습니다:

function doPost(e) {
  try {
    var data = JSON.parse(e.postData.contents);
    var studentName = data.student || "미지정";
    var behavior = data.behavior || "미지정";
    var timestamp = data.timestamp || Utilities.formatDate(new Date(), "GMT+9", "yyyy-MM-dd HH:mm:ss");
    var intensity = data.intensity || "보통";

    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName(studentName);
    
    // 학생 전용 시트가 없으면 생성 후 헤더 세팅
    if (!sheet) {
      sheet = ss.insertSheet(studentName);
      sheet.getRange(1, 1, 1, 4).setValues([["학생명", "표적행동", "타임스탬프", "강도"]]);
      sheet.getRange(1, 1, 1, 4).setFontWeight("bold").setBackground("#f1f5f9");
      sheet.setFrozenRows(1);
    }

    sheet.appendRow([studentName, behavior, timestamp, intensity]);

    return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ status: "error", message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}


우측 상단 [배포] → [새 배포]를 선택합니다.

유형: 웹 앱 (Web App)

설명: ABA v2 Webhook

다음 사용자 권한으로 실행: 나 (내 계정)

액세스 권한이 있는 사용자: 모든 사용자 (Anyone)

발급된 웹 앱 URL을 복사한 뒤, 웹앱의 [구글 시트 연동 설정] 모달에 붙여넣고 저장합니다.

🔑 Gemini API 키 설정

Google AI Studio에서 무료 API 키를 발급받습니다.

웹앱 우측 상단 톱니바퀴(⚙️ 설정)를 클릭합니다.

Gemini API Key 입력란에 키를 붙여넣고 [키 저장]을 누릅니다.

키는 브라우저 세션에 안전하게 보관되며, 학생별 AI 분석 리포트 생성에 사용됩니다.

📄 라이선스

이 프로젝트는 교육 및 특수교육 현장의 행동 관찰 지원을 위해 자유롭게 사용 및 수정할 수 있습니다.
