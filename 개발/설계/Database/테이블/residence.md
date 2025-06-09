## Residence (거주지)

| **필드명**      | **필드 설명** | **타입**      | **속성**                    | **코멘트**                                                                               |
|--------------|-----------|-------------|---------------------------|---------------------------------------------------------------------------------------|
| residence_id | 거주지 고유 ID | INT         | PRIMARY KEY               | AUTO_INCREMENT                                                                        |
| building     | 동 이름      | VARCHAR(10) | NOT NULL                  | 거주지가 위치한 건물 이름                                                                        |
| floor_number | 층 수       | VARCHAR(10) | NOT NULL                  | 몇 층인지 나타냄                                                                             |
| room_number  | 호수        | VARCHAR(10) | NOT NULL                  | 특정 층의 몇 호인지 나타냄                                                                       |
| status       | 거주지 상태    | ENUM        | NOT NULL                  | ACTIVE(사용 중), INACTIVE(비어 있음), PENDING(승인 대기), UNAVAILABLE(사용 불가), DELETED(삭제)로 상태 분류 |
| notes        | 추가 메모     | TEXT        | NULLABLE                  | 거주지 관련 비고 및 관리자가 남긴 추가 정보                                                             |
| created_at   | 생성일시      | DATETIME    | DEFAULT now()             | 레코드가 생성된 일시                                                                           |
| updated_at   | 갱신일시      | DATETIME    | DEFAULT current_timestamp | 레코드가 마지막으로 갱신된 일시                                                                     |

```sql

CREATE TABLE Residence
(
    residence_id INT AUTO_INCREMENT PRIMARY KEY COMMENT '거주지 고유 ID',
    building     VARCHAR(10) NOT NULL COMMENT '동 이름',
    floor_number VARCHAR(10) NOT NULL COMMENT '층 수',
    room_number  VARCHAR(10) NOT NULL COMMENT '호수',
    status       ENUM('ACTIVE', 'INACTIVE', 'PENDING', 'UNAVAILABLE', 'DELETED') NOT NULL 
        DEFAULT 'INACTIVE' COMMENT '거주지 상태 (사용 중, 비어 있음, 승인 대기, 사용 불가, 삭제)',
    notes        TEXT COMMENT '거주지에 대한 추가 메모',
    created_at   DATETIME DEFAULT now() COMMENT '생성일시',
    updated_at   DATETIME DEFAULT current_timestamp ON UPDATE current_timestamp COMMENT '갱신일시'
) COMMENT='Residence 테이블 - 거주지 정보를 저장';

CREATE INDEX idx_building_floor_unit ON Residence (building_name, floor_number, unit_number);
CREATE INDEX idx_status ON Residence (status);

```

### 거주지 상태 정의

| **상태**    | **값**         | **설명**                                           |
|-----------|---------------|--------------------------------------------------|
| **사용 중**  | `ACTIVE`      | 현재 사용자가 거주 중인 상태. 예시: 입주민이 동-층-호에 실제로 거주 중.      |
| **비어 있음** | `INACTIVE`    | 현재 거주자가 없는 상태. 신규로 생성되었거나 기존 입주민이 퇴거하여 비어 있는 상태. |
| **승인 대기** | `PENDING`     | 사용자가 거주지 신청을 했으나 관리자가 승인을 대기 중인 상태.              |
| **사용 불가** | `UNAVAILABLE` | 특정 이슈(리모델링, 공사 등)로 인해 임시적으로 사용이 불가한 상태.          |
| **삭제**    | `DELETED`     | 더 이상 사용되지 않는 거주지로, 데이터가 영구적으로 삭제된 상태.            |


