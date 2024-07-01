# Jawaban Query Rekruitment

1. Tampilkan daftar siswa beserta kelas dan guru yang mengajar kelas tersebut:
   ```
   SELECT s.name AS nama_siswa, s.age, cl.name AS nama_kelas, t.name AS nama_guru, t.subject
   FROM teachers AS t 
   INNER JOIN classes AS cl ON cl.teacher_id = t.id
   INNER JOIN students AS s ON s.class_id = cl.id
   ```

2. Tampilkan daftar kelas yang diajar oleh guru yang sama.
   ```
   SELECT c2.name AS nama_kelas
   FROM classes c1
   JOIN classes c2 ON c1.teacher_id = c2.teacher_id AND c1.id <> c2.id
   GROUP BY c2.name
   ORDER BY c2.name
   ```

3. buat query view untuk siswa, kelas, dan guru yang mengajar
   ```
   CREATE  OR REPLACE VIEW `view_siswa_kelas_guru` AS
   SELECT s.name AS nama_siswa, s.age, cl.name AS nama_kelas, t.name AS nama_guru, t.subject as subjek_guru
   FROM teachers AS t 
   INNER JOIN classes AS cl ON cl.teacher_id = t.id
   INNER JOIN students AS s ON s.class_id = cl.id;
   ```
   
4. buat query yang sama tapi menggunakan store_procedure
   ```
   DROP procedure IF EXISTS `proc_siswa_kelas_guru`;

   DELIMITER $$
   CREATE PROCEDURE `proc_siswa_kelas_guru` ()
   BEGIN
     SELECT s.name AS nama_siswa, s.age, cl.name AS nama_kelas, t.name AS nama_guru, t.subject as subjek_guru
     FROM teachers AS t 
     INNER JOIN classes AS cl ON cl.teacher_id = t.id
     INNER JOIN students AS s ON s.class_id = cl.id;
   END$$

   DELIMITER ;
   ```
   
5. buat query input, yang akan memberikan warning error jika ada data yang sama pernah masuk
   ```
   SELECT COUNT(*) INTO @existing_count
   FROM students
   WHERE name = 'Budi';

   INSERT INTO students (name, age, class_id)
   SELECT 'Budi', 16, 1
   FROM dual
   WHERE @existing_count = 0;
   
   SELECT IF(@existing_count = 0, 'Siswa berhasil dimasukan', 'Siswa sudah pernah dimasukan') AS status;
   ```
