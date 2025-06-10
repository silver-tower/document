## resident (입주민)

| **필드명**       | **필드 설명**          | **타입**   | **속성**                     | **코멘트**                                           |
|---------------|--------------------|----------|----------------------------|---------------------------------------------------|
| resident_id   | 입주민 고유 ID          | INT      | PRIMARY KEY                | AUTO_INCREMENT                                    |
| person_id     | 사용자 ID (Person)    | INT      | NOT NULL, UNIQUE           | `person`의 고유 식별자와 연결                              |
| residence_id  | 거주지 ID (Residence) | INT      | NOT NULL                   | `residence`의 고유 식별자와 연결                           |
| move_in_date  | 입주 날짜              | DATE     | NOT NULL                   | 실제 입주 날짜                                          |
| move_out_date | 퇴거 날짜              | DATE     | NULLABLE                   | 미입력 상태일 경우, 현재 입주 중임.                             |
| status        | 입주민 상태             | ENUM     | NOT NULL DEFAULT 'PENDING' | PENDING, APPROVED, DEACTIVATED, TERMINATED로 상태 분류 |
| notes         | 추가 메모              | TEXT     | NULLABLE                   | 입주민에 대한 지정 메모(예: 특수 요청, 내부 비고).                   |
| created_at    | 생성일시               | DATETIME | DEFAULT now()              | 생성된 시간                                            |
| updated_at    | 갱신일시               | DATETIME | DEFAULT current_timestamp  | 마지막으로 변경된 시간                                      |

```sql
CREATE TABLE resident
(
    resident_id   INT AUTO_INCREMENT PRIMARY KEY COMMENT '입주민 고유 ID',
    person_id     INT  NOT NULL UNIQUE COMMENT '사용자 ID (Person)',
    residence_id  INT  NOT NULL COMMENT '거주지 ID (Residence)',
    move_in_date  DATE NOT NULL COMMENT '입주 날짜',
    move_out_date DATE     DEFAULT NULL COMMENT '퇴거 날짜 (미입력 시 현재 입주 중 상태)',
    status        ENUM('PENDING', 'APPROVED', 'REJECTED', 'DEACTIVATED', 'TERMINATED') NOT NULL DEFAULT 'PENDING' COMMENT '입주민 상태 (승인 대기, 승인 완료, 거절, 비활성화, 종료(퇴거))',
    notes         TEXT COMMENT '입주민에 대한 추가 메모',
    created_at    DATETIME DEFAULT now() COMMENT '생성일시',
    updated_at    DATETIME DEFAULT current_timestamp ON UPDATE current_timestamp COMMENT '갱신일시',
    FOREIGN KEY (person_id) REFERENCES Person (person_id) ON DELETE CASCADE,
    FOREIGN KEY (residence_id) REFERENCES residence (residence_id) ON DELETE CASCADE
) COMMENT='resident 테이블 - 입주민 정보를 저장';

-- 퇴거 날짜를 기준으로 자주 조회하므로 인덱스 추가
CREATE INDEX idx_moveoutdate ON resident (move_out_date);
-- 추가로 거주지 기반 조회를 고려한 인덱스
CREATE INDEX idx_residence ON resident (residence_id);
```