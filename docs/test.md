---

title: 文章标题 

status: draft

order: 10

categories: 

- 分类1

tags:

- 标签1
-  标签2

---



| Table Name | Attributes                                                   |
| ---------- | ------------------------------------------------------------ |
| classroom  | building, room_number, capacity                              |
| department | dept_name, building, budget                                  |
| course     | course_id, title, dept_name, credits                         |
| instructor | ID, name, dept_name, salary                                  |
| section    | course_id, sec_id, semester, year, building, room_number, time_slot_id |
| teaches    | ID, course_id, sec_id, semester, year                        |
| student    | ID, name, dept_name, tot_cred                                |
| takes      | ID, course_id, sec_id, semester, year, grade                 |
| advisor    | s_ID, i_ID                                                   |
| time_slot  | time_slot_id, day, start_hr, start_min, end_hr, end_min      |
| prereq     | course_id, prereq_id                                         |