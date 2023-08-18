# Домашняя работа №3
## Введение в Docker
### Установка Docker

1. Обновляем список пакетов в системе;
    ```
    apt update
    ```
    ![apt update](pic/image.png)

2.  Устанавливаем необходимые пакеты для использования репозитория по HTTPS;
    ```
    apt install apt-transport-https ca-certificates curl software-properties-common
    ```
    ![apt install apt-transport-https ca-certificates curl software-properties-common](pic/image-1.png)
    ![apt install apt-transport-https ca-certificates curl software-properties-common](pic/image-2.png)

3. Загружаем официальный GPG-ключ Docker и добавляем его в систему;
    ```
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```
    ![curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg](pic/image-3.png)

4. Добавляем репозиторий Docker в список источников пакетов в системе;
    ```
    echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    ![echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null](pic/image-4.png)

5. Обновляем список пакетов, чтобы включить информацию о пакетах Docker из добавленного репозитория;
    ```
    apt update
    ```
    ![apt update](pic/image-5.png)

6. Устанавливаем Docker;
    ```
    apt install docker-ce
    ```
    ![apt install docker-ce](pic/image-6.png)
    ![apt install docker-ce](pic/image-7.png)

7. Добавляем текущего пользователя в группу docker, чтобы избежать использования sudo для запуска Docker команд;
    ```
    sudo usermod -aG docker $USER
    ```
    ![sudo usermod -aG docker $USER](pic/image-8.png)

8. Перезагружаем группы пользователя, чтобы изменения в группе docker вступили в силу;
    ```
    newgrp docker
    ```
    ![newgrp docker](pic/image-9.png)

9. Проверяем версию установленного Docker.
    ```
    docker --version
    ```
    ![docker --version](pic/image-10.png)

### Тестирование контейнера с использованием образа "cowsay"

1. Запускаем Docker;
    ```
    sudo systemctl start docker
    ```
    ![sudo systemctl start docker](pic/image-11.png)

2. Запускаем контейнер с использованием образа "cowsay";
    ```
    docker run docker/whalesay cowsay Hello, Docker!
    ```
    ![docker run docker/whalesay cowsay Hello, Docker!](pic/image-12.png)

3. Запускаем контейнер с рисунком слона;
    ```
    docker run docker/whalesay cowsay -f elephant "Hello, Docker!"
    ```
    ![docker run docker/whalesay cowsay -f elephant "Hello, Docker!"](pic/image-13.png)

4. Запускаем контейнер с рисунком пингвина;
    ```
    docker run docker/whalesay cowsay -f tux "Hello, Docker!"
    ```
    ![docker run docker/whalesay cowsay -f tux "Hello, Docker!"](pic/image-14.png)

5. Запускаем контейнер с рисунком дракона;
    ```
    docker run docker/whalesay cowsay -f dragon "Hello, Docker!"
    ```
    ![docker run docker/whalesay cowsay -f dragon "Hello, Docker!"](pic/image-15.png)

6. Запускаем контейнер с рисунком кота;
    ```
    docker run docker/whalesay cowsay -f kitty "Hello, Docker!"
    ```
    ![docker run docker/whalesay cowsay -f kitty "Hello, Docker!"](pic/image-16.png)

### Хранение данных в контейнерах Docker: Руководство с пояснениями

1. Запускаем контейнер из образа Ubuntu и входим в него;
    ```
    docker run -it -h GB --name gb-test ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test ubuntu:22.10](pic/image-17.png)

2. Смотрим содержимое корневой директории;
    ```
    ls -l
    ```
    ![ls -l](pic/image-18.png)

3. Создадем новую директорию в корне:
    ```
    mkdir /example
    ```
    ![mkdir /example](pic/image-19.png)

4. Создаем файл "passwords.txt";
    ```
    touch /example/passwords.txt
    ```
    ![touch /example/passwords.txt](pic/image-20.png)

5. Добавляем в него какие-либо данные.
    ```
    echo "123test" >> /example/passwords.txt
    ```
    ![echo "123test" >> /example/passwords.txt](pic/image-21.png)

6. Перейдем в директорию example;
    ```
    cd example/
    ```
    ![cd example/](pic/image-22.png)

7. Просмотрим содержимое файла passwords.txt;
    ```
    cat passwords.txt
    ```
    ![cat passwords.txt](pic/image-23.png)

8. Выходим из контейнера;
    ```
    exit
    ```
    ![exit](pic/image-24.png)

9. Останавливаем контейнер;
    ```
    docker stop gb-test
    ```
    ![docker stop gb-test](pic/image-25.png)

10. Запускаем контейнер;
    ```
    docker start gb-test
    ```
    ![docker start gb-test](pic/image-26.png)

11. Выполняем команду bash внутри контейнера;
    ```
    docker exec -it gb-test bash
    ```
    ![docker exec -it gb-test bash](pic/image-27.png)

12. Просмотрим содержимое файла passwords.txt;
    ```
    cat /example/passwords.txt
    ```
    ![cat /example/passwords.txt](pic/image-28.png)
    Наши данные сохранятся, так как мы не пересоздавали контейнер.

13. Выходим из контейнера;
    ```
    exit
    ```
    ![exit](pic/image-24.png)

14. Останавливаем контейнер;
    ```
    docker stop gb-test
    ```
    ![docker stop gb-test](pic/image-25.png)

15. Удалаяем контейнер;
    ```
    docker rm gb-test
    ```
    ![docker rm gb-test](pic/image-29.png)

16. Запускаем контейнер из образа Ubuntu и входим в него;
    ```
    docker run -it -h GB --name gb-test ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test ubuntu:22.10](pic/image-30.png)

17. Просмотрим содержимое файла passwords.txt;
    ```
    cat /example/passwords.txt
    ```
    ![cat /example/passwords.txt](pic/image-31.png)
    В этот раз наши данные будут утеряны, так как контейнер был удален.

### Использование внешнего хранилища

1. Создаем директорию;
    ```
    mkdir testfolder
    ```
    ![mkdir testfolder](pic/image-32.png)

2. Монтируем еее к контейнеру;
    ```
    docker run -it -h GB --name gb-test -v /home/al/testfolder:/otherway ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test -v /home/al/testfolder:/otherway ubuntu:22.10](pic/image-33.png)

3. Добавляем данные в подмонтированную директорию;
    ```
    echo "$HOSTNAME" >> /otherway/test.txt
    ```
    ![echo "$HOSTNAME" >> /otherway/test.txt](pic/image-34.png)

4. Просмотрим содержимое файла test.txt в контейнере;
    ```
    cat otherway/test.txt
    ```
    ![cat otherway/test.txt](pic/image-35.png)

5. Выходим из контейнера;
    ```
    exit
    ```
    ![exit](pic/image-36.png)

6. Проверяем доступность данных с локальной системы;
    ```
    cat testfolder/test.txt
    ```
    ![cat testfolder/test.txt](pic/image-37.png)

7. Удаляем контейнер;
    ```
    docker rm gb-test
    ```
    ![docker rm gb-test](pic/image-38.png)

8. Создаем его снова, подмонтировав директорию;
    ```
    docker run -it -h GB --name gb-test -v /home/al/testfolder:/otherway ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test -v /home/al/testfolder:/otherway ubuntu:22.10](pic/image-39.png)
9. Просмотрим содержимое файла test.txt в контейнере;
    ```
    cat otherway/test.txt
    ```
    ![cat otherway/test.txt](pic/image-40.png)
    Данные по-прежнему доступны. Самый надежный способ хранения данных в контейнерах - использование внешних хранилищ. Важно избегать хранения важных данных внутри контейнеров, чтобы предотвратить потерю информации.

### Часть-2 Хранение данных в контейнерах Docker: Практическое руководство

1. Создаем папку;
    ```
    mkdir ~/docker-mount-example
    ```
    ![mkdir ~/docker-mount-example](pic/image-41.png)

2. Создаем файл test.txt в этой папке с некоторым содержимым;
    ```
    echo "This is the host test.txt file" > ~/docker-mount-example/test.txt
    ```
    ![echo "This is the host test.txt file" > ~/docker-mount-example/test.txt](pic/image-42.png)

3. В домашней директории создаем файл test.txt, который также понадобится для монтирования в контейнер, но с другим содержимым;
    ```
    echo "This is the root test.txt file" > ~/test.txt
    ```
    ![echo "This is the root test.txt file" > ~/test.txt](pic/image-43.png)

4. Создаем контейнер из образа ubuntu:22.10 и задаем ему имя и hostname:
    ```
    docker run -it -h GB --name gb-test ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test ubuntu:22.10](pic/image-44.png)

5. Выходим из контейнера;
    ```
    exit
    ```
    ![exit](pic/image-45.png)

6. Останавливаем и удаляем контейнер;
    ```
    docker stop gb-test && docker rm gb-test
    ```
    ![docker stop gb-test && docker rm gb-test](pic/image-46.png)

7. Создаем контейнер из образа ubuntu:22.10, задаем ему имя и hostname и монтируем ранее созданную папку с хоста в контейнер;
    ```
    docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10](pic/image-47.png)

8. Смотрим содержимое текстового файла в контейнере;
    ```
    cat /container-mount/test.txt
    ```
    ![cat /container-mount/test.txt](pic/image-48.png)

9. Останавливаем и удаляем контейнер;
    ```
    docker stop gb-test && docker rm gb-test
    ```
    ![docker stop gb-test && docker rm gb-test](pic/image-49.png)

10. Создаем контейнер из образа ubuntu:22.10, задаем ему имя и hostname и монтируем созданный ранее текстовый файл из домашней директории внутрь смонтированной папки в контейнере;
    ```
    docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10
    ```
    ![docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10](pic/image-50.png)

11. Смотрим содержимое текстового файла в контейнере;
    ```
    cat /container-mount/test.txt
    ```
    ![cat /container-mount/test.txt](pic/image-51.png)
    Мы создали контейнер и монтировали папку docker-mount-example внутрь контейнера. Затем мы монтировали файл test.txt из домашней директории внутрь этой папки в контейнере. При просмотре содержимого файла в контейнере, мы видим данные из файла в домашней директории.