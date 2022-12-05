Selezionare tutti gli studenti nati nel 1990 (160)
```sql
SELECT * FROM `students` 
WHERE YEAR(`date_of_birth`)=1990;
```
Selezionare tutti i corsi che valgono più di 10 crediti (479)
```sql
SELECT * FROM `courses` 
WHERE `cfu` > 10;
```

Selezionare tutti gli studenti che hanno più di 30 anni
```sql
/* Mostro prima le età a schermo */
SELECT *, DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(), `date_of_birth`)), '%Y') + 0 AS age
FROM `students`;
/* Modifico la tabella e aggiungo il nuovo valore */
ALTER TABLE `students`
ADD age INT;
UPDATE `students`
SET age = DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(), `date_of_birth`)), '%Y') + 0;
/* Seleziono gli studenti con più di 30 anni */
SELECT *
FROM `students`
WHERE `age` > 30;
```

Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
Selezionare tutti i corsi di laurea magistrale (38)
Da quanti dipartimenti è composta l'università? (12)
Quanti sono gli insegnanti che non hanno un numero di telefono? (50)