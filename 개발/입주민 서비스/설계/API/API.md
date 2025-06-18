## API 공통 응답 형식

### 성공시

``` json
{
    "success": true,
    "timestamp": "2025-06-18T23:29:51.823355",
    "data": {
        ...생략...
    },
    "uuid": "ee5b7a7e-0c18-4033-ba96-74dbf3e57837"
}
```

### 실패시

``` json
{
    "success": false,
    "timestamp": "2025-06-18T23:28:15.885038",
    "error": {
        "code": "U4002",
        "message": "중복된 사용자입니다."
    },
    "uuid": "a57fb101-669a-4278-be5a-a3194c09b8b5"
}
```

## API 목록

| **도메인**                | **API 이름**       | **HTTP 메서드** | **엔드포인트**                                      | **설명**                                               |
|------------------------|------------------|--------------|------------------------------------------------|------------------------------------------------------|
| **users**              | 회원가입             | POST         | `/api/v1/users`                                | 새로운 사용자를 등록하고 계정을 생성합니다.                             |
| **users**              | 회원 정보 조회         | GET          | `/api/v1/users/{personId}`                     | 사용자의 정보를 반환합니다.                                      |
| **users**              | 회원 정보 수정         | PUT          | `/api/v1/users/{personId}`                     | 사용자의 정보를 수정합니다.                                      |
| **users**              | 회원 리스트 조회        | GET          | `/api/v1/users`                                | 모든 회원 정보를 페이지네이션 기능과 함께 제공합니다. (예: `page=1&size=10`) |
| **residents**          | 입주민 등록 요청        | POST         | `/api/v1/residents/register`                   | 사용자가 동/층/호를 선택하여 입주민 등록 요청을 만듭니다.                    |
| **residents**          | 입주민 상태 확인        | GET          | `/api/v1/residents/status`                     | 사용자의 입주민 등록 상태(PENDING, APPROVED 등)를 반환합니다.          |
| **residents**          | 입주민 승인 요청 리스트 조회 | GET          | `/api/v1/admin/residents/requests`             | 관리자가 처리해야 할 입주민 등록 요청 목록을 조회합니다.                     |
| **residents**          | 입주민 승인 처리        | PATCH        | `/api/v1/admin/residents/{residentId}/approve` | 특정 입주민의 등록 요청을 승인하거나 거절합니다.                          |
| **residents**          | 입주민 상태 변경        | PATCH        | `/api/v1/admin/residents/{residentId}/status`  | 특정 입주민의 상태를 DEACTIVATED 등으로 변경합니다.                   |
| **residences**         | 거주지 리스트 조회       | GET          | `/api/v1/residences`                           | 모든 거주지(동-층-호) 정보를 상태와 함께 반환합니다.                      |
| **residences**         | 거주지 메모, 상태 변경    | PATCH        | `/api/v1/admin/residences/{residenceId}`       | 특정 거주지의 메모와 상태를 ACTIVE, INACTIVE 등으로 변경합니다.          |
| **residences**         | 건물 추가            | POST         | `/api/v1/admin/residences/building`            | 건물명, 층수, 층당 호실수를 기반으로 새 건물 및 관련 데이터를 DB에 저장합니다.      |
| **emergency_contacts** | 비상 연락처 등록        | POST         | `/api/v1/emergency-contacts`                   | 사용자가 비상 연락처 정보를 추가합니다.                               |
| **emergency_contacts** | 비상 연락처 조회        | GET          | `/api/v1/emergency-contacts`                   | 사용자의 모든 비상 연락처 정보를 반환합니다.                            |
| **emergency_contacts** | 비상 연락처 수정        | PUT          | `/api/v1/emergency-contacts/{contactId}`       | 특정 비상 연락처 정보를 수정합니다.                                 |
| **emergency_contacts** | 비상 연락처 삭제        | DELETE       | `/api/v1/emergency-contacts/{contactId}`       | 특정 비상 연락처를 삭제합니다.                                    |
| **admin**              | 관리자 대시보드 조회      | GET          | `/api/v1/admin/dashboard`                      | 관리자용 요약 및 분석 데이터를 대시보드 형식으로 반환합니다.                   |

