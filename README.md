# Soldier Management SQL - Group README

An army training school database: ranks, units, missions, soldiers, and which soldier served on which mission. All data is fictional.

## One file per person
| Student | Student no. | P | U | M | Soldier 32 rank | Soldier 33 unit | File | Database |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Mucunguzi felic| 202510599 | 6 | 1 | 2 | 1 | 2 | `sms_202510599.sql` | `sms_202510599` |
| ishimwe yousra joyce | 202513181 | 7 | 4 | 3 | 2 | 5 | `sms_202513181.sql` | `sms_202513181` |
| carlos Goodluck MUKAMA | 202511910 | 5 | 2 | 1 | 6 | 3 | `sms_202511910.sql` | `sms_202511910` |

P = last digit; U = (last two digits mod 6) + 1; M = (P mod 5) + 1. Every person's file uses their own values, so queries C4, C7, C8, D3 and soldiers 31-33 differ per file.

## Same design in all three files
Each `sms_<student number>.sql` is a complete hand-in file and builds its own database `sms_<student number>` (as the exam requires). The three databases share the identical design below, so the files are one design with three personal versions.

| Table | Key | Links |
| --- | --- | --- |
| ranks | rank_id | 1 rank : many soldiers |
| units | unit_id | 1 unit : many soldiers |
| missions | mission_id | many soldiers : many missions via soldier_missions |
| soldiers | soldier_id (auto) | FK rank_id, FK unit_id |
| soldier_missions | (soldier_id, mission_id) | FK soldier, FK mission, role required |

Rules enforced: unique names and service numbers, rank level 1-6, end_date NULL (ongoing) or >= start_date, status only Active / On Leave / Retired (default Active), enlistment after birth, foreign keys everywhere, no soldier twice on one mission.

## Inside each file (in order)
1. First line `SELECT DATABASE(), CURRENT_USER(), NOW();` as the exam requires
2. Part A: create database + 5 tables
3. Part B: B1 base data, B2 `SOURCE soldiers_seed.sql;`, B3 soldiers 31-33 and their missions
4. Part C: C1-C10 queries
5. Part D: EXPLAIN, index `idx_soldiers_last_name`, view `v_soldier_profile`, D3
6. Change Order placeholder

Every statement has a comment; every SELECT has a `Rows returned` line.

## How to run (PowerShell, in this folder)
Put the teacher's `soldiers_seed.sql` here first, then:
```powershell
mysql -u root -p -t -e "source sms_202511196.sql"
mysql -u root -p -t -e "source sms_202510787.sql"
mysql -u root -p -t -e "source sms_202510585.sql"
```

## Before hand-in (each person, own file only)
1. Draw the ER diagram on paper and hand it in first.
2. Run your file, replace every `<N>` with the real row count.
3. Write the D1 "what changed" comment from your real EXPLAIN output.
4. Add the Change Order at the end of your file.
5. Soldier 31 uses a placeholder birth/enlistment date - change it, and the first/last name if the split is wrong.
