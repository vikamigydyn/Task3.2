## Лабораторная работа №3.2. Работа с базой данных.
## Выполнила: Мигыдын Вика, ИСП-211о

## 1. Обзор
Приложение состоит из двух активностей: основной активности для добавления и обновления студентов и второй активности для отображения списка студентов.
## 2. Структура
- **MainActivity**: Основная активность, содержащая кнопки для добавления нового студента, обновления последнего студента и перехода к списку студентов.
  
![скрин1 (2)](https://github.com/user-attachments/assets/b3e22cfb-bf57-485d-a136-76ef27726567)

- **MainActivity2**: Вторая активность, отображающая список студентов с их именами и временем добавления.
  
![скрин2 (2)](https://github.com/user-attachments/assets/ace16cf8-688a-4bfc-8054-732049e5592c)

## 3. MainActivity
На данном экране расположены кнопки:
- **Кнопка 1 "Посмотреть"**: При нажатии запускает `MainActivity2` для отображения списка студентов.
  
` btnShowRecords.setOnClickListener(v -> {
    Intent intent = new Intent(MainActivity.this, MainActivity2.class);
    startActivity(intent);
});`
- **Кнопка 2 "Добавить"**: При нажатии добавляет нового студента с именем "Новая Фамилия", "Новое Имя" и "Новое Отчество" в базу данных.
  
`btnAddRecord.setOnClickListener(v -> {
    ContentValues contentValues = new ContentValues();
    contentValues.put("surname", "Новая Фамилия");
    contentValues.put("name", "Новое Имя");
    contentValues.put("patronymic", "Новое Отчество");
    long newRowId = db.insert("classmates", null, contentValues);
    if (newRowId != -1) {
        Toast.makeText(MainActivity.this, "Одногруппник добавлен", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(MainActivity.this, "Ошибка добавления одногруппника", Toast.LENGTH_SHORT).show();
    }
});`

![скрин3 (2)](https://github.com/user-attachments/assets/4076dd57-082b-4935-8f29-5741c26eecfa)

![скрин4 (1)](https://github.com/user-attachments/assets/d2dc61fd-e3d5-4d2f-898a-c9dac089341b)

- Кнопка 3 "Обновить": При нажатии обновляет информацию о последнем добавленном студенте на "Иванов Иван Иванович".
  
`private boolean updateLastRecord() {
    Cursor cursor = db.rawQuery("SELECT * FROM classmates ORDER BY ID DESC LIMIT 1", null);
    boolean updated = false;
    if (cursor != null && cursor.moveToFirst()) {
        @SuppressLint("Range") int id = cursor.getInt(cursor.getColumnIndex("ID"));
        ContentValues contentValues = new ContentValues();
        contentValues.put("surname", "Иванов");
        contentValues.put("name", "Иван");
        contentValues.put("patronymic", "Иванович");
        int rowsAffected = db.update("classmates", contentValues, "ID = ?", new String[]{String.valueOf(id)});
        updated = rowsAffected > 0;
    }
    cursor.close();
    return updated;
}`

![скрин5 (2)](https://github.com/user-attachments/assets/732a5619-01de-4176-acd9-0a073378fd42)

![скрин6 (2)](https://github.com/user-attachments/assets/a36feef7-e833-413b-b893-673f37b17f3a)

## 4. MainActivity2
Второй экран создан для отображения списка студентов. Он состоит из ListView, который заполняется данными из базы данных:

Список студентов отображает фамилию, имя, отчество и время добавления каждого студента.
## 5. База данных
Приложение использует SQLite для хранения информации о студентах. При первом запуске создается таблица classmates, в которую добавляются 5 студентов с фамилиями, именами и отчествами "Фамилия1" до "Фамилия5".
