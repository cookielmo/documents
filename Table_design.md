#テーブル設計
####Article Table

|   name   |       type      | null | KEY |  U  |    default     | remarks |
| :------- | :-------------- | :--: | :-: | :-: | :------------- | ------- |
| id       | UNSIGNED BIGINT |  NG  |  PK | Yes | Auto Increment |         |
| title    | VARCHAR(20)     |  NG  |     |     |                |         |
| url      | VARCHAR(20)     |  NG  |     |     |                |         |
| img      | VARCHAR(20)     |      |     |     |                |         |
| date     | DATE            |      |     |     |                |         |
| category | UNSIGNED INT    |      |  FK |     |                |         |
| author   | VARCHAR(20)     |  NG  |     |     |                |         |

####Category Table

| name |     type     | null | KEY |  U  |    default     | remarks |
| :--- | :----------- | :--: | :-: | :-: | :------------- | ------- |
| id   | UNSIGNED INT |  NG  |  U  | Yes | Auto Increment |         |
| name | VARCHAR(20)  |  NG  |     | Yes |                |         |

#####DDL
```sql
SET SESSION FOREIGN_KEY_CHECKS=0;

/* Drop Tables */

DROP TABLE IF EXISTS article;
DROP TABLE IF EXISTS category;




/* Create Tables */

CREATE TABLE article
(
    id bigint unsigned NOT NULL AUTO_INCREMENT,
    category bigint unsigned NOT NULL,
    title varchar(20),
    url varchar(20),
    img varchar(20),
    author varchar(20),
    date date,
    PRIMARY KEY (id),
    UNIQUE (id),
    UNIQUE (category)
);


CREATE TABLE category
(
    id bigint unsigned NOT NULL AUTO_INCREMENT,
    name varchar(20),
    PRIMARY KEY (id),
    UNIQUE (id),
    UNIQUE (name)
);



/* Create Foreign Keys */

ALTER TABLE article
    ADD FOREIGN KEY (category)
    REFERENCES category (id)
    ON UPDATE RESTRICT
    ON DELETE RESTRICT
;

```