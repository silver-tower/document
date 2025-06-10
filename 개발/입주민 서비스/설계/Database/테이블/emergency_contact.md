## emergency_contact (비상 연락처)

| **필드명**      | **필드 설명**    | **타입**       | **속성**                                                | **코멘트**                |
|--------------|--------------|--------------|-------------------------------------------------------|------------------------|
| contact_id   | 비상 연락처 고유 ID | INT          | PRIMARY KEY                                           | AUTO_INCREMENT         |
| resident_id  | 입주민 고유 ID    | INT          | NOT NULL                                              | `Resident`의 고유 식별자와 연결 |
| contact_name | 비상 연락처 이름    | VARCHAR(100) | NOT NULL                                              | 비상 연락 대상 이름            |
| phone_number | 비상 연락처 전화번호  | VARCHAR(20)  | NOT NULL                                              | 비상 연락할 전화 번호           |
| notes        | 메모           | TEXT         | NULLABLE                                              | 추가적으로 기록할 메모           |
| created_at   | 생성일시         | DATETIME     | DEFAULT now()                                         | 생성된 시간                 |
| updated_at   | 갱신일시         | DATETIME     | DEFAULT current_timestamp ON UPDATE current_timestamp | 마지막으로 변경된 시간           |

```sql
CREATE TABLE EmergencyContact
(
    contact_id   INT AUTO_INCREMENT PRIMARY KEY COMMENT '비상 연락처 고유 ID',
    resident_id  INT          NOT NULL COMMENT '입주민 고유 ID (Resident)',
    contact_name VARCHAR(100) NOT NULL COMMENT '비상 연락처 이름',
    phone_number VARCHAR(20)  NOT NULL COMMENT '비상 연락처 전화번호',
    notes        TEXT     DEFAULT NULL COMMENT '비상 연락처 메모',
    created_at   DATETIME DEFAULT now() COMMENT '생성일시',
    updated_at   DATETIME DEFAULT current_timestamp ON UPDATE current_timestamp COMMENT '갱신일시',
    FOREIGN KEY (resident_id) REFERENCES Resident (resident_id) ON DELETE CASCADE
) COMMENT='EmergencyContact 테이블 - 입주민의 비상 연락처를 저장';

CREATE INDEX idx_resident_phone ON EmergencyContact (resident_id, phone_number);
CREATE INDEX idx_contact_name ON EmergencyContact (contact_name);
```
