mysql> use centre_formation;
Database changed
mysql> select titreform, nomsess, datedebut, datefin from formation
    -> join session
    -> on formation.codeform = session.codeform
    -> ;
+-------------------+--------------+------------+------------+
| titreform         | nomsess      | datedebut  | datefin    |
+-------------------+--------------+------------+------------+
| Programming Java  | Session1101  | 2022-01-02 | 2022-01-14 |
| Programming Java  | Session 1102 | 2022-02-03 | 2022-02-15 |
| Programming Java  | Session 1104 | 2022-10-15 | 2022-10-27 |
| web developpment  | Session 1201 | 2022-03-04 | 2022-03-18 |
| web developpment  | Session 1202 | 2022-04-05 | 2022-04-19 |
| web developpment  | Session 1203 | 2022-11-16 | 2022-11-30 |
| web developpment  | Session 1204 | 2022-12-17 | 2022-12-31 |
| Anglais technique | Session 1301 | 2022-01-06 | 2022-01-21 |
| Anglais technique | Session 1302 | 2022-05-07 | 2022-05-22 |
| Anglais technique | Session 1303 | 2022-06-08 | 2022-06-23 |
| Communication     | Session 1401 | 2022-09-01 | 2022-09-11 |
| Communication     | Session 1402 | 2022-08-08 | 2022-08-18 |
| Base de données   | Session 1501 | 2022-11-11 | 2022-12-01 |
| Base de données   | Session 1502 | 2022-09-12 | 2022-10-02 |
| Soft,skills       | Session 1601 | 2022-09-13 | 2022-09-25 |
| Soft,skills       | Session 1602 | 2022-10-14 | 2022-10-26 |
+-------------------+--------------+------------+------------+
16 rows in set (0.01 sec)

mysql> select titreform, nometu, prenometu from inscription
    -> join session on session.codesess = inscription.codesess
    -> join formation on session.codeform = formation.codeform
    -> join etudiant on etudiant.nom_cinetu = inscription.nom_cinetu
    -> order by titreform;
+-------------------+---------------+--------------+
| titreform         | nometu        | prenometu    |
+-------------------+---------------+--------------+
| Anglais technique | Alami         | Ahmad        |
| Anglais technique | Souni         | Sanaa        |
| Anglais technique | Idrissi Alami | Mohammed     |
| Anglais technique | Toumi         | Aicha        |
| Anglais technique | Ouled thami   | Ali          |
| Base de données   | Alami         | Ahmad        |
| Base de données   | Souni         | Sanaa        |
| Base de données   | Idrissi Alami | Mohammed     |
| Base de données   | Boudiaf       | Fatima Zohra |
| Base de données   | Toumi         | Aicha        |
| Base de données   | Ben Omar      | Abd Allah    |
| Base de données   | Ouled thami   | Ali          |
| Communication     | Souni         | Sanaa        |
| Communication     | Idrissi Alami | Mohammed     |
| Communication     | Boudiaf       | Fatima Zohra |
| Communication     | Toumi         | Aicha        |
| Communication     | Ben Omar      | Abd Allah    |
| Communication     | Ouled thami   | Ali          |
| Programming Java  | Alami         | Ahmad        |
| Programming Java  | Souni         | Sanaa        |
| Programming Java  | Idrissi Alami | Mohammed     |
| Programming Java  | Boudiaf       | Fatima Zohra |
| Programming Java  | Toumi         | Aicha        |
| Programming Java  | Ben Omar      | Abd Allah    |
| Programming Java  | Ouled thami   | Ali          |
| web developpment  | Alami         | Ahmad        |
| web developpment  | Souni         | Sanaa        |
| web developpment  | Idrissi Alami | Mohammed     |
| web developpment  | Toumi         | Aicha        |
| web developpment  | Ben Omar      | Abd Allah    |
| web developpment  | Ouled thami   | Ali          |
+-------------------+---------------+--------------+
31 rows in set (0.00 sec)

mysql> select count(titreform) from inscription
    -> join session on session.codesess = inscription.codesess
    -> join formation on session.codeform = formation.codeform
    -> where titreform = 'web developpment';
+------------------+
| count(titreform) |
+------------------+
|                6 |
+------------------+
1 row in set (0.00 sec)

mysql> SELECT formation.titreform, COUNT(inscription.nom_cinetu)
    -> FROM formation
    -> JOIN session ON formation.codeform = session.codeform
    -> JOIN inscription ON session.codesess = inscription.codesess
    -> WHERE inscription.typecours = 'Distanciel'
    -> GROUP BY formation.titreform
    -> HAVING COUNT(inscription.nom_cinetu) >= 3;
+------------------+-------------------------------+
| titreform        | COUNT(inscription.nom_cinetu) |
+------------------+-------------------------------+
| Programming Java |                             7 |
| Communication    |                             6 |
+------------------+-------------------------------+
2 rows in set (0.00 sec)
0------------ggggggggggggggggg'[[[[[ ]]]]]

mysql> select nomspec, titreform, dureeform, prixform from catalogue
    -> join specialite on catalogue.codespec = specialite.codespec
    -> join formation on catalogue.codeform = formation.codeform
    -> where Active = 1
    -> order by nomspec desc;
+---------------+-------------------+-----------+----------+
| nomspec       | titreform         | dureeform | prixform |
+---------------+-------------------+-----------+----------+
| langues       | Anglais technique |        15 |     3750 |
| GL            | Programming Java  |        12 |     3600 |
| GL            | web developpment  |        14 |     4200 |
| GL            | Base de données   |        20 |     6000 |
| data          | Base de données   |        20 |     6000 |
| Communication | Anglais technique |        15 |     3750 |
| Communication | Communication     |        10 |     2500 |
| Communication | Soft,skills       |        12 |     3000 |
+---------------+-------------------+-----------+----------+
8 rows in set (0.00 sec)

mysql> SELECT YEAR(session.datedebut) AS Annee, MONTH(session.datedebut) AS Mois, SUM(formation.prixform) AS Total_Prix
    -> FROM formation
    -> JOIN session ON formation.codeform = session.codeform
    -> GROUP BY Annee, Mois;
+-------+------+------------+
| Annee | Mois | Total_Prix |
+-------+------+------------+
|  2022 |    1 |       7350 |
|  2022 |    2 |       3600 |
|  2022 |   10 |       6600 |
|  2022 |    3 |       4200 |
|  2022 |    4 |       4200 |
|  2022 |   11 |      10200 |
|  2022 |   12 |       4200 |
|  2022 |    5 |       3750 |
|  2022 |    6 |       3750 |
|  2022 |    9 |      11500 |
|  2022 |    8 |       2500 |
+-------+------+------------+