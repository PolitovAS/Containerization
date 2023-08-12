# Домашняя работа №2
## Механизмы контрольных групп
### Работа с LXC

1. Устанавливаем необходимые пакеты для работы с LXC;
    ```
    apt-get install lxc debootstrap bridge-utils lxc-templates
    ```
    ![apt-get install lxc debootstrap bridge-utils lxc-templates](pic/image.png)
    ![apt-get install lxc debootstrap bridge-utils lxc-templates](pic/image-1.png)

2. Устанавливаем LXD;
    ```
    apt-get install lxd-installer
    ```
    ![apt-get install lxd-installer](pic/image-2.png)

3. Запускаем настройку LXD, где мы просто нажимаем Enter, чтобы использовать значения по умолчанию;
    ```
    lxd init
    ```
    ![lxd init](pic/image-3.png)

4. Проверяем список доступных хранилищ LXC;
    ```
    lxc storage list
    ```
    ![lxc storage list](pic/image-4.png)

5. Создаем контейнер с именем "test123" и используем шаблон Ubuntu. Опция -f указывает на конфигурационный файл, который определяет настройки виртуальной сети;
    ```
    lxc-create -n test123 -t ubuntu -f /usr/share/doc/lxc/example/lxc-veth.conf
    ```
    ![lxc-create -n test123 -t ubuntu -f /usr/share/doc/lxc/example/lxc-veth.conf](pic/image-6.png)
    ![lxc-create -n test123 -t ubuntu -f /usr/share/doc/lxc/example/lxc-veth.conf](pic/image-5.png)

6. Открываем конфигурационный файл контейнера "test123" для редактирования;
    ```
    nano /var/lib/lxc/test123/config
    ```
    ![nano /var/lib/lxc/test123/config](pic/image-7.png)
    ![nano /var/lib/lxc/test123/config](pic/image-8.png)

7. Ограничиваем максимальное количество памяти, которое может использовать контейнер до 256M;
    ```
    lxc.cgroup2.memory.max = 256M
    ```
    ![lxc.cgroup2.memory.max = 256M](pic/image-10.png)

8. Запускаем контейнер с именем "test123" в фоновом режиме;
    ```
    lxc-start -d -n test123
    ```
    ![lxc-start -d -n test123](pic/image-11.png)

9. Подключаемся к контейнеру "test123";
    ```
    lxc-attach -n test123
    ```
    ![lxc-attach -n test123](pic/image-12.png)

10. Показываем использование памяти на хост-системе;
    ```
    free -m
    ```
    ![free -m](pic/image-13.png)

11. Выходим из контейнера;
    ```
    exit
    ```
    ![exit](pic/image-14.png)

12. Проверяем список всех контейнеров, установленных на хост-системе;
    ```
    lxc-ls -f
    ```
    ![lxc-ls -f](pic/image-15.png)

13. Открываем конфигурационный файл контейнера "test123" для редактирования и указываем настройку, которая запускает контейнер автоматически при старте хост-системы;
    ```
    lxc.start.auto = 1
    ```
    ![lxc.start.auto = 1](pic/image-16.png)

14. Останавливаем контейнер "test123";
    ```
    lxc-stop -n test123
    ```
    ![lxc-stop -n test123](pic/image-17.png)

15. Проверяем, что контейнер остановлен выведя список всех контейнеров, установленных на хост-системе;
    ```
    lxc-ls -f
    ```
    ![lxc-ls -f](pic/image-18.png)

16. Перезагружаем хост-систему;
    ```
    reboot
    ```
    ![reboot](pic/image-19.png)

17. Убеждаемся, что контенер запустился автоматически;
    ```
    lxc-ls -f
    ```
    ![lxc-ls -f](pic/image-20.png)

18. Останавливаем контейнер "test123";
    ```
    lxc-stop -n test123
    ```
    ![lxc-stop -n test123](pic/image-17.png)

19. Запускаем контейнер "test123" и записываем логи в файл "log.log";
    ```
    lxc-start -n test123 --logfile log.log
    ```
    ![lxc-start -n test123 --logfile log.log](pic/image-21.png)

20. Перезагружаем хост-систему;
    ```
    reboot
    ```
    ![reboot](pic/image-19.png)

21. Убеждаемся, что логи записываются, т.к. после попытки его запустить, говорится, что конейнер уже запущен;
    ```
    lxc-start -n test123 --logfile log.log
    ```
    ![lxc-start -n test123 --logfile log.log](pic/image-22.png)

22. Останавливаем контейнер "test123";
    ```
    lxc-stop -n test123
    ```
    ![lxc-stop -n test123](pic/image-17.png)

23. Удаляем контейнер "test123" с хост-системы.
    ```
    lxc-destroy -n test123
    ```
    ![lxc-destroy -n test123](pic/image-23.png)