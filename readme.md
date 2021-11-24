<table style="width: 100%;">
  <tr>
    <td style="text-align: center; border: none;">
    Министерство образования и науки РФ<br>
Государственное бюджетное профессиональное образовательное учреждение Республики Марий Эл<br>
Йошкар-Олинский технологический колледж
</td>
  </tr>
  <tr>
    <td style="text-align: center; border: none; height: 15em;">
    <h2 style="font-size:3em;">Реферат</h2>
      <h3><br><br> по дисциплине "Проектирование и Разработка Информационных Систем"<br><br> Тема:<b>"Чтение/запись файлов в андроиде (открытие галереи/фото)"</b> </h3></td>
  </tr>
  <tr>
    <br><br><td style="text-align: right; border: none; height: 20em;">
      Разработала:<br/>
      Смирнов Евгений<br>
      Группа: И-31<br>
      Преподаватель:<br>
      Колесников Евгений Иванович
    </td>
  </tr>
  <tr>
    <td style="text-align: center; border: none; height: 5em;">
    г.Йошкар-Ола,<br>2021</br></td>
  </tr>
</table>


<div style="page-break-after: always;"></div>


# Содержание

1. [Вывод данных согласно макету.](#Вывод-данных-согласно-макету.)
2. [Пагинация, сортировка, фильтрация, поиск](#Пагинация,-сортировка,-фильтрация,-поиск)
3. [Подсветка элементов по условию. Дополнительные выборки.Массовая смена цены продукции.](#Подсветка-элементов-по-условию.-Дополнительные-выборки.Массовая-смена-цены-продукции.)

# Чтение/запись файлов в андроиде

## Использование внутреннего хранилища

По умолчанию любые файлы, которые вы сохраняете во внутреннем хранилище, являются приватными для вашего приложения. К другим приложениям и пользователям при обычных условиях они не могут быть доступны. Эти файлы удаляются, когда пользователь удаляет приложение .

### **Запись текста в файл**

```String fileName= "helloworld";
      String textToWrite = "Hello, World!";
      FileOutputStream fileOutputStream;

      try {
        fileOutputStream = openFileOutput(fileName, Context.MODE_PRIVATE);
        fileOutputStream.write(textToWrite.getBytes());
        fileOutputStream.close();
      } catch (Exception e) {
        e.printStackTrace();
      } 
```
### **Добавление текста в существующий файл**

Используйте `` Context.MODE_APPEND `` для режима режима `` openFileOutput ``

``` fileOutputStream = openFileOutput(fileName, Context.MODE_APPEND); ```

# Использование внешнего хранилища
«Внешнее» хранилище - это еще один тип хранилища, который мы можем использовать для хранения файлов на устройстве пользователя. Он имеет некоторые ключевые отличия от «внутреннего» хранилища, а именно:

 * Это не всегда доступно. В случае съемного носителя (SD-карта) пользователь может просто удалить хранилище.

 * Это не личное. У пользователя (и других приложений) есть доступ к этому файлу.
 
 * Если пользователь удаляет приложение, файлы, которые вы сохраняете в каталоге, полученном с помощью ``getExternalFilesDir()`` будут удалены.

Чтобы использовать Внешнее хранилище, мы должны сначала получить соответствующее разрешение. Вам нужно будет использовать:

`android.permission.WRITE_EXTERNAL_STORAGE` для чтения и записи
``android.permission.READ_EXTERNAL_STORAGE`` только для чтения

Чтобы предоставить эти разрешения, вам необходимо будет идентифицировать их в вашем `AndroidManifest.xml` таковом

  ``` <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> ```

`**ПРИМЕЧАНИЕ. Они являются опасными разрешениями, если вы используете API-уровень 23 или выше, вам нужно будет запросить разрешение во время выполнения.**`

Перед попыткой записи или чтения из внешнего хранилища вы всегда должны проверить, доступ ли носитель данных.

  ``` String state = Environment.getExternalStorageState();
  if (state.equals(Environment.MEDIA_MOUNTED)) {
      // Available to read and write
  }    
  if (state.equals(Environment.MEDIA_MOUNTED) || 
          state.equals(Environment.MEDIA_MOUNTED_READ_ONLY)) {
      // Available to at least read
  } 
```
При записи файлов во внешнее хранилище вы должны решить, должен ли файл быть общедоступным или приватным. Хотя оба этих типа файлов по-прежнему доступны для пользователей и других приложений на устройстве, существуют различные между ними.

Публичные файлы остаются на устройстве, когда пользователь удаляет приложение. Примером файла, который должен быть сохранен как «Публичный», фотографии, сделанные через ваше приложение.

Частные файлы должны быть удалены, когда пользователь удалит приложение. Эти типы файлов будут называться приложениями и не будут для пользователей или других приложений. Бывший. временные файлы, загруженные / используемые вашим приложением.

Вот как получить доступ к папке « Documents как для общедоступных, так и для частных файлов:

**Общественный**

  ``` // Access your app's directory in the device's Public documents directory
  File docs = new File(Environment.getExternalStoragePublicDirectory(
          Environment.DIRECTORY_DOCUMENTS), "YourAppDirectory");
  // Make the directory if it does not yet exist
  myDocs.mkdirs();
```
**Частный**

``` // Access your app's Private documents directory
File file = new File(context.getExternalFilesDir(Environment.DIRECTORY_DOCUMENTS), 
        "YourAppDirectory");
// Make the directory if it does not yet exist
myDocs.mkdirs();
``` 

# Открытие галереи/фото

Чтобы использовать галерию, мы должны сначала получить соответствующее разрешение в манифесте. Вам нужно будет использовать:

```<uses-permission android:name="android.permission.CAMERA"/>  ```

``` <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> ```

Кода функции для выбора изображения и захвата изображения:

``` fun selectImageInAlbum() {
        val intent = Intent(Intent.ACTION_GET_CONTENT)
        intent.type = "image/*"
        if (intent.resolveActivity(packageManager) != null) {
            startActivityForResult(intent, REQUEST_SELECT_IMAGE_IN_ALBUM)
        }
    }
 fun takePhoto() {
        val intent1 = Intent(MediaStore.ACTION_IMAGE_CAPTURE)
        if (intent1.resolveActivity(packageManager) != null) {
            startActivityForResult(intent1, REQUEST_TAKE_PHOTO)
        }
    }
 companion object {
        private val REQUEST_TAKE_PHOTO = 0
        private val REQUEST_SELECT_IMAGE_IN_ALBUM = 1
    }
``` 



