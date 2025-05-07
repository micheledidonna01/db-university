# db-university

Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d'Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.
Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

# Consegna
Dopo aver creato un nuovo database nel vostro MySQL Workbench e aver importato lo schema allegato, eseguite le query del file allegato.

## 1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT 
    *
FROM
    db_primo.students
WHERE YEAR(date_of_birth) = 1990 ;


## 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT 
    *
FROM
    db_primo.courses
WHERE cfu > 10;


## 3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT 
    *
FROM
    db_primo.students
WHERE date_of_birth < "1995-05-07";


SELECT 
    date_of_birth, CURDATE(), YEAR(CURDATE()) - YEAR(date_of_birth) > 30 as eta
FROM
    db_primo.students
ORDER BY date_of_birth;


SELECT *
FROM 
    db_primo.students
WHERE TIMESTAMPDIFF(YEAR, CURDATE() - date_of_birth)




## 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

SELECT 
    *
FROM
    db_primo.courses
WHERE 
	period = "I semestre" AND year = 1;


## 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)

SELECT 
    *
FROM
    db_primo.exams
WHERE
    date = '2020-06-20' AND hour > '14:00';




## 6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT 
    *
FROM
    db_primo.degrees
WHERE
    name LIKE 'corso di laurea magistrale%';


## 7. Da quanti dipartimenti è composta l'università? (12)

SELECT 
    COUNT(*) AS numero_dipartimenti
FROM
    db_primo.departments;


## 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT 
    COUNT(*)
FROM
    db_primo.teachers
WHERE
    phone IS NULL;




# ESERCIZI CON GROUP BY

## 1. Contare quanti iscritti ci sono stati ogni anno

```
SELECT count(id), year
FROM db_primo.courses
GROUP BY year
```

## 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```
SELECT COUNT(id) as teacher_equal_office, office_address 
FROM db_primo.teachers
GROUP BY office_address;
```

## 3. Calcolare la media dei voti di ogni appello d'esame

```
SELECT exam_id, AVG(vote) AS media_voti
FROM db_primo.exam_student
GROUP BY exam_id;
```

## 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```
SELECT degree_id ,COUNT(id) as quanita_corsi
FROM db_primo.courses
GROUP BY degree_id;
```