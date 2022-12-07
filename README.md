# otuslinuxproff_hw7
домашняя работа по теме "Управление пакетами. Дистрибьюция софта"

Работа выполнялась на системе Manjaro 5.15.78-1, в качестве серверной ОС на виртуальной машине и на VPS - Rockylinux9.
Собирал с помощью Mock из исходного кода пакет Apache HTTP Server 2.4.54 + APR and APR-UTIL(эпоху изменил на #2),
этот пакет установил и запустил на VPS в качестве репозитория, в который положил этот же пакет и сопутсвующие ему

``` rpmdev-setuptree ```  - создаение в домашней директории дерева каталогов для сборки

``` rpmdev-spectool -g -R httpd.spec ```  - Загрузка исходников указанных внутри SPEC-файла  в каталог ~/rpmbuild/SOURCES

``` rpmbuild -bs httpd.spec ``` - создание SRPM-пакета , он будет в каталоге ~/rpmbuild/SRPMS

запуск сборки rpm пакета с помощью Mock:

``` sudo mock -r rocky+epel-9-x86_64 --rebuild ~/rpmbuild/SRPMS/httpd-2.4.54.src.rpm ``` 

![mockresult_list](https://user-images.githubusercontent.com/59445051/206169925-9ef08e37-ddfe-4ad3-ad47-f905d9ab4651.png)


По SCP передал на VPS файлы, запустил сервер и коммандой ``` sudo createrepo . ``` из директории /var/www/html/repo создал репозиторий 

![creeatehttpd_repo](https://user-images.githubusercontent.com/59445051/206175145-27265d00-5ba9-4b03-977f-d915c135a045.png)
