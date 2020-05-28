# Руководство по сборке Qt со статической компоновкой под Windows

## Необходимые компоненты

- Online installer(https://www.qt.io/download-qt-installer?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4)
- Visual Studio 2017 или 2019(https://visualstudio.microsoft.com/ru/downloads/)
- Исходники Qt
- Active Perl последней версии
- Python 2.7+

## Порядок сборки

1. Скачиваем QT Online Installer(https://www.qt.io/download-qt-installer?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4):
//Изображение


2. Запускаем и путём нажатия "Далее" попадаем на это окно(изображение ниже)(смотрим на установленные галочки)(Версия qt 5.9-5.15):

//Изображение с галочками


3. Устанавливаем с стандартными настройками Perl и Python
4. Открываем x64 Native Tools Command Prompt

   ![](images/01.png)

5. Создаем папку для сборки

   ```shell script
   mkdir build
   cd build
   ```

6. Запускаем конфигурационный скрипт

   ```shell script
   <Путь до исходников Qt>\configure.bat -debug-and-release -static -static-runtime -opensource -confirm-license -platform win32-msvc -qt-zlib -qt-libpng -qt-libjpeg -nomake examples -nomake tests -no-opengl -skip qtscript -prefix "C:\Qt\5.12.2-msvc"
   ```

   Ключи:
   - `skip <module>` - исключает из процесса сборки отдельный подмодуль
   - `nomake examples` - исключает из процесса сборки примеры программ
   - `nomake tests` - исключает из процесса сборки тесты
   - `platform <platform>` - определяет платформу, для которой будет собран Qt, в данном случае Windows с MSVC
   - `no-opengl` - не использовать OpenGL для отрисовки интерфейса
   - `qt-zlib`, `qt-libpng`, `qt-libjpeg` - использовать библиотеки `zlib`, `libpng`, `libjpeg` поставляемые вместе с Qt
   - `opensource` - использовать open source вариант Qt
   - `confirm-license` - автоматически принять лицензию Qt
   - `debug-and-release`, `release`, `debug` - варианты сборок
   - `static`, `static-runtime` - включить статическую компоновку Qt и runtime
   - `prefix <prefix>` - путь до папки, в которую будут скопированы откомпилированные файлы Qt

7. Запускаем сборку Qt (займет достаточно много времени)

   ```shell script
   nmake
   ```

8. Устанавливаем Qt

   ```shell script
   nmake install
   ```

## Настройка QtCreator

1. Открываем «Параметры», выбираем «Комплекты» и переходим на вкладку «Профили Qt»

   ![](images/02.png)

2. Нажимаем кнопку «Добавить…» и указываем путь до `qmake.exe`

   ![](images/03.png)

3. Вводим название для нового профиля и нажимаем кнопку «Применить»

   ![](images/04.png)

4. Переходим на вкладку «Комплекты». Копируем комплект с соответствующим компилятором (в данном случае MSVC)

   ![](images/05.png)

5. В свойствах нового комплекта выбираем новое название, а также выбираем только что добавленный нами профиль Qt. Нажимаем «ОК»

   ![](images/06.png)

## Подключение комплекта к существующему проекту

1. Открываем проект в QtCreator. Переключаемся в режим «Проекты»

   ![](images/07.png)

2. В списке профилей выбираем новый профиль Qt

   ![](images/08.png)

3. Для сборки с нужным комплектом слева внизу выбираем наш профиль Qt и запускаем сборку

   ![](images/09.png)
