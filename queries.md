1 Selezionare tutti gli studenti nati nel 1990 (160)
```sql
SELECT * FROM `students` 
WHERE YEAR(`date_of_birth`)=1990;
```
2 Selezionare tutti i corsi che valgono più di 10 crediti (479)
```sql
SELECT * FROM `courses` 
WHERE `cfu` > 10;
```

3 Selezionare tutti gli studenti che hanno più di 30 anni
```sql
/* Mostro prima le età a schermo */
/* SELECT *, DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(), `date_of_birth`)), '%Y') + 0 AS age
FROM `students`; */
/* Modifico la tabella e aggiungo il nuovo valore */
/* ALTER TABLE `students`
ADD age INT;
UPDATE `students`
SET age = DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(), `date_of_birth`)), '%Y') + 0; */
/* Seleziono gli studenti con più di 30 anni */
SELECT *
FROM `students`
WHERE (DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(), `date_of_birth`)), '%Y') + 0) > 30;
```

4 Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
```sql
SELECT *
FROM `courses`
WHERE `period` = 'I semestre' AND `year` = 1;
```

5 Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
```sql
SELECT *
FROM `exams`
WHERE `date` = '2020-06-20' AND HOUR(`hour`) >= HOUR("14:00:00");
```

6 Selezionare tutti i corsi di laurea magistrale (38)
```sql
SELECT *
FROM `degrees`
WHERE `level` = 'magistrale';
```

7 Da quanti dipartimenti è composta l'università? (12)
```sql
SELECT COUNT(`id`)
AS TotalDepartments
FROM `departments`;
```

8 Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
```sql
SELECT COUNT(`id`)
AS teachersWithoutPhone
FROM `teachers`
WHERE `phone`
IS NULL;
```

Group by:

1 Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT YEAR(`enrolment_date`) AS enrolment_year,
 COUNT(`id`) AS students_enrolled 
 FROM `students`
 GROUP BY YEAR(`enrolment_date`);
```
2 Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT COUNT(*)
AS total_teachers, `teachers`.`office_address`
FROM `teachers`
GROUP BY `office_address`;
```

3 Calcolare la media dei voti di ogni appello d'esame
```sql
SELECT AVG(`vote`)
AS vote_average
FROM `exam_student`
GROUP BY `exam_id`;
```

4 Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT COUNT(`id`)
AS total_degrees
FROM `degrees`
GROUP BY `department_id`;
```

Join:

1 Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT `students`.`id`, `students`.`name`, `students`.`surname`, `degrees`.`name` AS corso_di_laurea
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

2 Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT `degrees`.`id`, `degrees`.`name`, `degrees`.`level`, `departments`.`id` AS `department_id`, `departments`.`name`
FROM `degrees`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';
```

3 Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT `courses`.`id`, `courses`.`name`, `teachers`.`id` AS `teacher_id`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname`
FROM `courses`
JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;
```

4 Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT students.*, degrees.*, departments.*
FROM students
JOIN degrees ON students.degree_id = degress.id
JOIN departments ON degrees.departments_id = departments.id
ORDER BY students.name, students.surname;
```

5 Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT degrees.name, courses.name, courses.period, courses.year, courses.cfu, teachers.name, teachers.surname
FROM degrees
JOIN courses ON courses.degree_id = degrees.id
JOIN course_techer ON course_teacher.course_id = courses.id
JOIN teachers ON course_teacher_id = teachers.id;
```

6 Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT DISTINCT teachers.*
FROM teachers
JOIN course_teacher ON course_teacher.teacher_id = teachers.id
JOIN courses ON course_teacher.course_id = courses.id
JOIN degrees ON courses.degree_id = degrees.id
JOIN departments ON degrees.department_id = departments.id
WHERE departments.name = 'Dipartimento di Matematica';
```

BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
```sql
SELECT students.id, students.name, students.surname, courses.name, COUNT(exam_student.vote) AS `attempts_number`, MAX(exam_student.vote) AS `max_vote`
FROM students
JOIN exam_student ON exam_student.student_id = students.id
JOIN exams ON exam_student.exam_id = exams.id
JOIN courses ON exams.course_id = courses.id
GROUP BY course.id, students.id
HAVING max_vote >= 18;
```