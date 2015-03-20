#テーブル設計
####Article Table

|   name   |       type      | null | KEY |  U  |    default     | remarks |
| :------- | :-------------- | :--: | :-: | :-: | :------------- | ------- |
| id       | UNSIGNED BIGINT |  NG  |  PK | Yes | Auto Increment |         |
| title    | TEXT            |  NG  |     |     |                |         |
| url      | TEXT            |  NG  |     |     |                |         |
| img      | TEXT            |      |     |     |                |         |
| date     | DATE            |      |     |     |                |         |
| category | UNSIGNED INT    |      |  FK |     |                |         |
| author   | TEXT            |  NG  |     |     |                |         |

####Category Table

| name |     type     | null | KEY |  U  |    default     | remarks |
| :--- | :----------- | :--: | :-: | :-: | :------------- | ------- |
| id   | UNSIGNED INT |  NG  |  U  | Yes | Auto Increment |         |
| name | VARCHAR(128) |  NG  |     | Yes |                |         |

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
    title text,
    url text,
    img text,
    author text,
    date date,
    PRIMARY KEY (id),
    UNIQUE (id),
    UNIQUE (category)
);


CREATE TABLE category
(
    id bigint unsigned NOT NULL AUTO_INCREMENT,
    name varchar(128),
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