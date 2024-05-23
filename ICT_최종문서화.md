ICT 멘토링 최종 문서 작업을 하였다.

### **Users 사용자 정보**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| user_id | INT | PRIMARY KEY, AUTO_INCREMENT | 사용자 고유 식별자 |
| username | VARCHAR(50) | UNIQUE, NOT NULL | 사용자 이름 |
| password | VARCHAR(255) | NOT NULL | 사용자 비밀번호 |
| name | VARCHAR(100) | NULL | 사용자 실명 |
| address | TEXT | NULL | 사용자 주소 |
| phone_number | VARCHAR(20) | NULL | 사용자 전화번호 |
| age | INT | NULL | 사용자 나이 |
| family_history | TEXT | NULL | 가족 병력 |
| medical_conditions | TEXT | NULL | 의료 상태 |
| height | DECIMAL(5,2) | NULL | 사용자 키 |
| weight | DECIMAL(5,2) | NULL | 사용자 몸무게 |
| guardian_name | VARCHAR(100) | NULL | 보호자 이름 |
| guardian_phone | VARCHAR(20) | NULL | 보호자 전화번호 |

### **Notifications 알림**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| alarm_id | INT | PRIMARY KEY, AUTO_INCREMENT | 알림 고유 식별자 |
| user_id | INT | FOREIGN KEY | 사용자 테이블과 연결 |
| alarm_type | VARCHAR(50) | NOT NULL | 알림 유형 (예: 물, 약) |
| message | TEXT | NOT NULL | 알림 메시지 |
| date_time | DATETIME | NOT NULL | 알림 발송 시간 |
| active | TINYINT | NOT NULL | 알림 활성화 상태 (0 또는 1) |

### **ChatLogs 챗봇 대화 기록**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| log_id | INT | PRIMARY KEY, AUTO_INCREMENT | 대화 로그 고유 식별자 |
| user_id | INT | FOREIGN KEY | 사용자 테이블과 연결 |
| session_id | VARCHAR(255) | NOT NULL | 대화 세션 식별자 |
| message | TEXT | NOT NULL | 사용자 또는 챗봇이 보낸 메시지 |
| sender | VARCHAR(50) | NOT NULL | 메시지 보낸 주체 ('user', 'bot') |
| chat_time | DATETIME | NOT NULL | 메시지 전송 시간 |

### **FoodItems 음식 정보**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| food_id | INT | PRIMARY KEY, AUTO_INCREMENT | 음식 고유 식별자 |
| food_name | VARCHAR(100) | NOT NULL | 음식 이름 |
| calories | DECIMAL(10,2) | NULL | 음식 당 열량 (kcal) |
| nutrients | TEXT | NULL | 영양성분 정보 (JSON 형태로 저장 가능) |
| shelf_life_days | INT | NULL | 평균 유통 기한 (일 단위) |

### **DietaryRestrictions 식이 제한**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| restriction_id | INT | PRIMARY KEY, AUTO_INCREMENT | 식이 제한 고유 식별자 |
| disease | VARCHAR(100) | NOT NULL | 질병 명 |
| restricted_food_ids | TEXT | NULL | 제한 음식의 food_id 목록 (쉼표로 구분) |
| alternative_food_ids | TEXT | NULL | 대체 가능 음식의 food_id 목록 (쉼표로 구분) |

### **MealRecords 식사 기록**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| record_id | INT | PRIMARY KEY, AUTO_INCREMENT | 식사 기록 고유 식별자 |
| user_id | INT | FOREIGN KEY | 사용자 테이블과 연결 |
| food_id | INT | FOREIGN KEY | 음식 테이블과 연결 |
| quantity | DECIMAL(10,2) | NOT NULL | 섭취한 양 (단위는 음식에 따라 다름) |
| meal_time | DATETIME | NOT NULL | 식사 시간 |

### **UserAgreements 사용자 약관 동의**

| Column | Type | Constraints | Description |
| --- | --- | --- | --- |
| agreement_id | INT | PRIMARY KEY, AUTO_INCREMENT | 약관 동의 고유 식별자 |
| user_id | INT | FOREIGN KEY | Users 테이블과 연결 |
| terms_agreement | TINYINT | NOT NULL | 서비스 이용 약관 동의 여부 (0: 비동의, 1: 동의) |
| privacy_agreement | TINYINT | NOT NULL | 개인정보 처리방침 동의 여부 (0: 비동의, 1: 동의) |
| agreement_date | DATETIME | NOT NULL | 약관 동의 날짜와 시간 |
| terms_version | VARCHAR(20) | NOT NULL | 동의한 서비스 이용 약관의 버전 |
| privacy_version | VARCHAR(20) | NOT NULL | 동의한 개인정보 처리방침의 버전 |
|  |  |  |  |

---

ERD

![Untitled (6)](https://github.com/dawoon1229/ICT_project/assets/164113758/12ac75b7-64d0-4b7f-9d9a-d4cc86b8e1bf)

DietaryRestrictions 식이 제한 테이블은 어디에 연결 안되어있어도 되나?

ChatGPT

**`DietaryRestrictions`** 테이블은 질병별로 식이 제한과 대체 음식을 관리하는 테이블입니다. 이 테이블은 사용자의 특정 질병 상태에 따라 어떤 음식을 피해야 하며 어떤 음식이 대체 가능한지를 나타내는 정보를 제공하는 역할을 합니다.

### **`DietaryRestrictions` 테이블의 연결 여부**

- **직접적인 외래 키 없음**: **`DietaryRestrictions`**는 질병명을 기반으로 정보를 제공하므로, 직접적으로 **`Users`** 테이블에 연결되어 있지 않습니다. 이는 사용자의 **`medical_conditions`** 필드와 로직적으로 연결되어 있지만, 데이터베이스 스키마 상에서는 외래 키로 직접 연결되지 않습니다.
- **간접적인 정보 연결**: 사용자의 **`medical_conditions`** 필드에 저장된 질병 정보를 통해 **`DietaryRestrictions`**에서 해당 질병에 대한 식이 제한 정보를 조회할 수 있습니다. 이 연결은 애플리케이션 레벨에서 처리되며, SQL 조인이나 프로그래밍 로직을 통해 데이터를 연결합니다.

### **데이터베이스 스키마에서 `DietaryRestrictions`의 역할**

**`DietaryRestrictions`**는 주로 읽기 전용 데이터로 활용됩니다. 사용자의 질병 상태에 따라 식이 제한을 조회하고, 적절한 대체 음식을 제공하는 데 중점을 둡니다. 이를 위해 다음과 같은 방식으로 활용될 수 있습니다:

1. **조회 연산**: 사용자가 특정 음식에 대해 문의할 때, **`DietaryRestrictions`**를 조회하여 해당 음식이 사용자의 질병 상태에 부합하는지 확인합니다.
2. **대체 음식 제공**: 제한된 음식에 대한 대체 음식을 제공할 때, **`DietaryRestrictions`**의 **`alternative_food_ids`**를 사용하여 사용자에게 대체 음식 옵션을 제공합니다.

### **예시: 사용자의 질병에 따른 식이 제한 정보 조회**

```sql
sql코드 복사
SELECT
    DR.disease,
    DR.restricted_food_ids,
    DR.alternative_food_ids
FROM
    DietaryRestrictions DR
WHERE
    DR.disease = 'Diabetes';

```

이 쿼리는 당뇨병(**`Diabetes`**) 환자의 식이 제한과 대체 음식 목록을 조회합니다.

---

[사나래개발일정.xlsx](https://github.com/dawoon1229/ICT_project/files/15415321/default.xlsx)
