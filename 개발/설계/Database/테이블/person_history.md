## person_history (사용자 변경 이력)

| 필드명               | 필드 설명    | 타입           | 속성               | 코멘트                       | 비고             |
|-------------------|----------|--------------|------------------|---------------------------|----------------|
| person_history_id | 변경이력 ID  | INT          | PRIMARY KEY      | AUTO_INCREMENT            | 식별자            |
| person_id         | 사람 고유 ID | INT          | UNIQUE, NOT NULL | person.person_id          | person 테이블의 pk |
| name              | 이름       | VARCHAR(100) | NOT NULL         |                           | 변경 전 데이터       |
| birth_date        | 생년월일     | DATE         | NOT NULL         | YYYY-MM-DD 형식             | 변경 전 데이터       |
| phone_number      | 연락처      | VARCHAR(20)  | NOT NULL         | 사용자 전화번호                  | 변경 전 데이터       |
| gender            | 성별       | ENUM         | NOT NULL         | M(남), F(여), U(기타)         | 변경 전 데이터       |
| status            | 상태       | ENUM         | NOT NULL         | ACTIVE, INACTIVE, DELETED | 변경 전 데이터       |
| created_at        | 생성일시     | DATETIME     | NOT now()        | 가입 날짜                     | 변경 전 데이터       |

```sql
CREATE TABLE person_history (
    person_history_id INT AUTO_INCREMENT PRIMARY KEY COMMENT '변경이력 ID',
    person_id INT NOT NULL COMMENT '사람 고유 ID',
    name VARCHAR(100) NOT NULL COMMENT '이름 (변경 전 데이터)',
    birth_date DATE NOT NULL COMMENT '생년월일 (변경 전 데이터)',
    phone_number VARCHAR(20) NOT NULL COMMENT '연락처 (변경 전 데이터)',
    gender ENUM('M', 'F', 'U') NOT NULL COMMENT '성별 (M: 남, F: 여, U: 기타)',
    status ENUM('ACTIVE', 'INACTIVE', 'DELETED') NOT NULL COMMENT '현재 상태 (변경 전 데이터)',
    created_at DATETIME NOT NULL COMMENT '생성일시 (변경 전 데이터)',
    history_created_at DATETIME DEFAULT current_timestamp COMMENT '변경이력 생성 일시',
    FOREIGN KEY (person_id) REFERENCES person(person_id) ON DELETE CASCADE
) COMMENT='person 변경 이력을 저장';
```
