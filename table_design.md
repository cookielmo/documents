#テーブル設計
####Article Table

|   name   |       type      | null | KEY | default |    remarks     |
| :------- | :-------------- | :--: | :-: | :------ | :------------- |
| id       | UNSIGNED BIGINT |  NG  |  PK |         | Auto Increment |
| title    | VARCHAR(20)     |  NG  |     |         |                |
| url      | VARCHAR(20)     |  NG  |     |         |                |
| img      | VARCHAR(20)     |      |     |         |                |
| date     | DATE            |      |     |         |                |
| category | UNSIGNED INT    |      |  FK |         |                |
| author   | VARCHAR(20)     |  NG  |     |         |                |

####Category Table

| name |     type     | null | KEY | default |    remarks     |
| :--- | :----------- | :--: | :-: | :------ | :------------- |
| id   | UNSIGNED INT |  NG  |  FK |         | Auto Increment |
| name | VARCHAR(20)  |  NG  |     |         |                |

