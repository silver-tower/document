## person (사용자)

| 필드명          | 필드 설명      | 타입           | 속성                        | 코멘트                       | 비고  |
|--------------|------------|--------------|---------------------------|---------------------------|-----|
| person_id    | 사람 고유 ID   | INT          | PRIMARY KEY               | AUTO_INCREMENT            | 식별자 |
| name         | 이름         | VARCHAR(100) | NOT NULL                  |                           |     |
| birth_date   | 생년월일       | DATE         | NOT NULL                  | YYYY-MM-DD 형식             |     |
| phone_number | 연락처        | VARCHAR(20)  | NOT NULL,UNIQUE           | 사용자 전화번호                  |     |
| email        | 비상 연락처 이메일 | VARCHAR(255) | NULLABLE                  | 이메일                       |     |
| gender       | 성별         | ENUM         | NOT NULL                  | M(남), F(여), U(기타)         |     |
| status       | 현재 상태      | ENUM         | NOT NULL                  | ACTIVE, INACTIVE, DELETED |     |
| created_at   | 생성일시       | DATETIME     | DEFAULT now()             | 가입 날짜                     |     |
| updated_at   | 갱신일시       | DATETIME     | DEFAULT current_timestamp | 마지막 정보 수정 기록              |     |

```sql
CREATE TABLE person
(
    person_id    INT AUTO_INCREMENT PRIMARY KEY COMMENT '사람 고유 ID',
    name         VARCHAR(100) NOT NULL COMMENT '이름',
    birth_date   DATE         NOT NULL COMMENT '생년월일 (YYYY-MM-DD 형식)',
    phone_number VARCHAR(20)  NOT NULL COMMENT '연락처',
    email        VARCHAR(255) DEFAULT NULL COMMENT '비상 연락처 이메일',
    gender       ENUM('M', 'F', 'U') NOT NULL COMMENT '성별',
    status       ENUM('ACTIVE', 'INACTIVE', 'DELETED') NOT NULL DEFAULT 'ACTIVE' COMMENT '현재 상태 (정상, 비활성화, 삭제)',
    created_at   DATETIME     DEFAULT now() COMMENT '생성일시',
    updated_at   DATETIME     DEFAULT current_timestamp ON UPDATE current_timestamp COMMENT '갱신일시'
) COMMENT='Person 테이블 - 사용자 정보를 저장';

CREATE INDEX idx_name_phone ON person (name, phone_number);
CREATE INDEX idx_status ON person (status);
```

### 사용자 상태 정의

| **상태**   | **값**      | **설명**                                   |
|----------|------------|------------------------------------------|
| **정상**   | `ACTIVE`   | 정상적으로 시스템을 이용할 수 있는 상태                   |
| **비활성화** | `INACTIVE` | 사용자가 서비스 이용을 중단했거나, 관리자가 비활성화 상태로 변경한 경우 |
| **삭제**   | `DELETED`  | 어카운트 삭제가 완료되었으며 복구가 불가능한 상태              |
