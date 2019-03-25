
# Описание
####если запущен с параметром `magnets_for_abuses`, то:
1. Читает содержимое вкдалки 'Abuses' из таблицы
<a href="https://docs.google.com/spreadsheets/d/1GyBjpFrhetXLuxibq2vt5PcrBmLPnUqOlLgl_9UuDo8/edit?ts=59877357#gid=0">
Рабочий для ноута</a> <br>
Это те файлы, которые не открываются из WordPress
2. Фильтрует записи по наличию подстроки `Error uploading file ....`
Эта строка означает, что файл в каталоге был, но теперь его нет и его надо пере-закачать.
3. Ищет на  https://1337x.to/ Magnet-ссылку по названию файла + №сезона + №эпизода. Например:
`"The.Good.Doctor.S02E13"`
4. Скачивает Magnet-ссылки. `"download"."workers"` - количество одновременных скачиваний.  
5. Если файл не скачался за `"download"."screenRefreshInterval_msec"` , то он помечается как FAILED 
6. Загружает скачанный файл по FTP на OpenLoad в папку `Transit` 
7. Средствами OpenLoad'a (Remote Upload) копирует файл из `Transit` в `Added Links` 
8. Обновляет соответствующий пост на WordPress новой, рабочей ссылкой на файл. 

<br>

####если запущен с параметром `magnets_for_updates`, то:
1. Читает содержимое вкдалки 'Update Post' из таблицы
<a href="https://docs.google.com/spreadsheets/d/1GyBjpFrhetXLuxibq2vt5PcrBmLPnUqOlLgl_9UuDo8/edit?ts=59877357#gid=0">
Рабочий для ноута</a> <br>
2. Фильтрует записи по наличию подстроки `- sorry try again later -`
Эта строка означает, что файла ещё нет и его надо скачать/добавить на OpenLoad.
3. Ищет на  https://1337x.to/ Magnet-ссылку по названию файла + №сезона + №эпизода. Например:
`"The.Good.Doctor.S02E13"`
4. Скачивает Magnet-ссылки. `"download"."workers"` - количество одновременных скачиваний.  
5. Если файл не скачался за `"download"."screenRefreshInterval_msec"` , то он помечается как FAILED 
6. Загружает скачанный файл по FTP на OpenLoad в папку `Transit` 
7. Средствами OpenLoad'a (Remote Upload) копирует файл из `Transit` в `Added Links` 
8. Обновляет соответствующий пост на WordPress новой, рабочей ссылкой на файл.
9. В случае успеха пишет во вкдалке 'Update Post' в колонке B ссылку на файл на OpenLoad.   

#
https://github.com/atomashpolskiy/bt

mvn clean package -DskipTests

java -jar RestoreFilesMagnet.jar
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=1044 -jar RestoreFilesMagnet.jar magnets_for_abuses
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=1044 -jar RestoreFilesMagnet.jar magnets_for_updates

