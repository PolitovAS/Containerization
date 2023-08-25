# Домашняя работа №5
## Docker Compose и Docker Swarm
### Создание сервиса, состоящего из 2 различных контейнеров: 1 - веб, 2 - БД (compose)

1. Создаем и открываем для изменения файл docker-compose.yaml;
    ```
    nano docker-compose.yaml
    ```
    ![nano docker-compose.yaml](pic/image.png)

2. Прописываем сервисы;
    ```
    version: '3.9'
    services:

        adminer-web:
            image: adminer:4.8.1
            restart: always
            ports:
                - "6080:8080"

        data-base:
            image: mysql:8.0.31
            restart: always
            environment:
                MYSQL_ROOT_PASSWORD: 12345
    ```
    ![Alt text](pic/image-1.png)

3. Устанавиливаем docker-compose, в нашем случае уже установлен;
    ```
    apt install docker-compose
    ```
    ![apt install docker-compose](pic/image-2.png)
    ![apt install docker-compose](pic/image-3.png)

4. Запускаем проект в фоновом режиме;
    ```
    docker-compose up -d
    ```
    ![docker-compose up -d](pic/image-4.png)
    ![docker-compose up -d](pic/image-5.png)

5. Выводим на экран все доступные контейнеры;
    ```
    docker-compose ps
    ```
    ![docker-compose ps](pic/image-6.png)

6. Проверяем работу в браузере, перейдя по адресу:
    ```
    http://localhost:6080/
    ```
    ![http://localhost:6080/](pic/image-7.png)
    ![http://localhost:6080/](pic/image-8.png)