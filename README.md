# otuslinuxproff_hw7
домашняя работа по теме "Управление пакетами. Дистрибьюция софта"

Работа выполнялась на системе Manjaro 5.15.78-1, в качестве серверной ОС на виртуальной машине и на VPS - Rockylinux9.
Собирал с помощью Mock из исходного кода пакет Apache HTTP Server 2.4.54 (https://dlcdn.apache.org/httpd/httpd-2.4.54.tar.gz) + APR and APR-UTIL(эпоху изменил на #2),
этот пакет установил и запустил на VPS в качестве репозитория, в который положил этот же пакет и сопутсвующие ему

Адресс репозитория: http://167.172.37.251/repo/ (будет работать до принятия ДЗ)

На Manjaro для выполнения работ были установлены следующии пакеты:
- rpm-tools 4.18.0-1
- rpmdevtools 9.6-1
- mock 3.4-1.1

## поехали

``` rpmdev-setuptree ```  - создаение в домашней директории дерева каталогов для сборки

далее в директорию httpd-2.4.54/srclib нужно загрузить и переименовать библиотеки:
- APR https://dlcdn.apache.org//apr/apr-1.7.0.tar.gz в - "apr"
- APR-UTIL https://dlcdn.apache.org//apr/apr-util-1.6.1.tar.gz в "apr-util"

конфигурируем:

``` ./configure --with-included-apr" ```

``` rpmdev-spectool -g -R httpd.spec ```  - Загрузка исходников указанных внутри SPEC-файла  в каталог ~/rpmbuild/SOURCES

``` rpmbuild -bs httpd.spec ``` - создание SRPM-пакета , он будет в каталоге ~/rpmbuild/SRPMS

запуск сборки rpm пакета с помощью Mock:

``` sudo mock -r rocky+epel-9-x86_64 --rebuild ~/rpmbuild/SRPMS/httpd-2.4.54.src.rpm ``` 

![mockresult_list](https://user-images.githubusercontent.com/59445051/206169925-9ef08e37-ddfe-4ad3-ad47-f905d9ab4651.png)


По SCP передал на VPS файлы, запустил сервер и коммандой ``` sudo createrepo . ``` из директории /var/www/html/repo создал репозиторий 

![creeatehttpd_repo](https://user-images.githubusercontent.com/59445051/206175145-27265d00-5ba9-4b03-977f-d915c135a045.png)

![repobrowse](https://user-images.githubusercontent.com/59445051/206203644-68cd5855-190a-4144-9ebd-e45891b6ec73.png)


На виртуальной машине в деректори /etc/yum.repo.d создал файл конфиг репозитория:

![repoconfig](https://user-images.githubusercontent.com/59445051/206199843-04489aa5-95a0-45f7-9b06-a8f9ec054004.png)

Результат:

![dnflist](https://user-images.githubusercontent.com/59445051/206201829-06e555de-dbab-41d9-91c7-704836cb1de6.png)

Пакет установлен, сервер запущен, все работает!

![installhttpd](https://user-images.githubusercontent.com/59445051/206205981-0ed05e99-dc18-40c5-af71-9dee880a5694.png)


![statushttpd](https://user-images.githubusercontent.com/59445051/206205586-72e54415-fc27-4aee-a91a-b4d2984fddbb.png)

![itwork](https://user-images.githubusercontent.com/59445051/206205646-85cf4637-e7f3-4714-9107-4255cdbe3c0e.png)


