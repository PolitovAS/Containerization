# Домашняя работа №1
## Механизмы пространства имен
### Вызов chroot на практике.

1. Создаем директорию "testfolder":  
    ```
    mkdir testfolder
    ```
    ![mkdir testfolder](pic/image-1.png)

2. Создаем директорию "bin":
    ```
    mkdir testfolder/bin
    ```
    ![mkdir testfolder/bin](pic/image-5.png)

3. Копируем исполняемый файл командного интерпретатора /bin/bash в папку   "testfolder/bin":  
    ```
    cp /bin/bash testfolder/bin
    ```
    ![cp /bin/bash testfolder/bin](pic/image-2.png)

4. Создаем необходимые директории "testfolder/lib" и "testfolder/lib64":  
    ```
    mkdir testfolder/lib
    mkdir testfolder/lib64
    ```
    ![mkdir testfolder/lib](pic/image-3.png)

5. Копируем необходимые библиотеки в папку "testfolder/lib" и "testfolder/lib64":  
    ```
    cp /lib/x86_64-linux-gnu/libtinfo.so.6 testfolder/lib
    cp /lib/x86_64-linux-gnu/libc.so.6 testfolder/lib
    cp /lib64/ld-linux-x86-64.so.2 testfolder/lib64/
    ```
    ![cp /lib/x86_64-linux-gnu/libtinfo.so.6 testfolder/lib](pic/image-4.png)

6. Запускаем команду chroot для изменения корневой папки:  
    ```
    chroot testfolder /bin/bash
    ```
    ![chroot testfolder /bin/bash](pic/image-6.png)


### Сетевое пространство имен

1. Просмотрим информацию о сетевых интерфейсах, включая IP адреса, маски подсетей, MAC адреса и другую информацию:
    ```
    ip a
    ```
    ![ip a](pic/image-7.png)
    ![ip a](pic/image-8.png)

2. Создаём сетевое пространство имен:
    ```
    ip netns add testns
    ```
    ![ip netns add testns](pic/image-9.png)

3. Запускаем новый процесс bash в пространсте имен сети под названием "testns":
    ```
    ip netns exec testns bash
    ```
    ![ip netns exec testns bash](pic/image-10.png)

4. Проверяем:
    ```
    ip a
    ```
    ![ip a](pic/image-11.png)


### Изоляция с помощью разграничивающей утилиты "unshare"

1. Запускаем новый процесс bash в новом пространстве имен (namespace) сети и процессов, а также монтируем новую файловую систему proc.<br>"--net" создает новое пространство имен сети;<br>"--pid" создает новое пространство имен процессов;<br>"--fork" создает новый процесс-потомок;<br>"--mount-proc" монтирует файловую систему proc в новом пространстве имен процессов.<br>В результате мы получаем изолированное окружение для запуска процесса bash, где он не может взаимодействовать с другими процессами и сетевыми интерфейсами в системе.
    ```
    unshare --net --pid --fork --mount-proc /bin/bash
    ```
    ![unshare --net --pid --fork --mount-proc /bin/bash](pic/image-12.png)

2. Проверяем изоляцию по сети:
    ```
    ip a
    ```
    ![ip a](pic/image-13.png)

3. Проверяем изоляцию по процессам:
    ```
    ps aux
    ```
    ![ps aux](pic/image-14.png)