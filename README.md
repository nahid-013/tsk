<!-- 1 -->
SELECT 
    CONCAT(p.last_name, ' ', p.first_name) AS "ФИО пациента",
    e.create_date AS "Дата обращения",
    a.name AS "Наименование действия"
FROM 
    Patients p
JOIN 
    Events e ON p.id = e.patient_id
JOIN 
    Actions a ON e.id = a.event_id
WHERE 
    p.last_name = 'Иванов' AND p.first_name = 'Иван';

<!-- 2 -->

ALTER TABLE Patients
ADD COLUMN sex TINYINT(1) DEFAULT 0 NOT NULL COMMENT '0-неопределено, 1-М, 2-Ж';

UPDATE Patients
SET sex = CASE
    WHEN first_name IN ('Иван', 'Александр', 'Дмитрий') THEN 1  -- Примеры мужских имен
    WHEN first_name IN ('Мария', 'Анна', 'Елена') THEN 2       -- Примеры женских имен
    WHEN patronymic LIKE '%ович' OR patronymic LIKE '%евич' THEN 1 -- Отчества для мужчин
    WHEN patronymic LIKE '%овна' OR patronymic LIKE '%евна' THEN 2   -- Отчества для женщин
    ELSE 0 -- Неопределено
END;