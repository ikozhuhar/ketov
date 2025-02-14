<p align="center">
<img src="https://github.com/ikozhuhar/ketov/blob/main/img/ketov-linux.png">
</p>

<details>
<p><summary><b> :large_blue_diamond: ГЛАВА 1. Архитектура ОС Linux</b></summary></p>

Архитектура ОС Linux состоит из трех уровней: **Уровень пользователя**, **Уровень Ядра** и **Аппаратный уровень**. Два главных режима работы: `kernel space` и `user space`. Главное отличие между уровнем пользователя и ядра состоит в привилегиях доступа к аппаратных ресурсам памяти и устройствам ввода-вывода, к которым разрешен полный доступ из режи­ма ядра и ограниченный доступ из режима пользователя.



#### :diamond_shape_with_a_dot_inside: _Компоненты User Space_

**Kernel space** обеспечивает распределение ресурсов между пользователями и предоставляет базовый интерфейс для доступ к ресурсам.

Функции ядра доступны в **user mode** с помощью системных вызовов. **Системные вызовы** выполняются в ядре, а вызывается из **user space** с помощью библиотеки **libc.so**. 

Функции выполняющиеся в **user space** доступны с помощью библиотечных вызовов и выполняются в самих библиотеках, например, **libz.so** и  **libbz2.so**



#### :diamond_shape_with_a_dot_inside: _Компоненты Ядра_

Компоненты Ядра в основном обеспечивают распределение ресурсов, что приводит к появлению **менеджеров** или под­систем управления _процессов_, _памяти_, _ввода-вывода_ и _менеджера файловой системы_.

- **Менеджер (подсистема) процессов** распределяет время ЦП между выполняющимися задачами.

- **Менеджер (подсистема) ввода-вывода** распределяет доступ к устройствам ввода-вывода между процессоми.

- **Менеджер (подсистема) памяти** распределяет пространство ОЗУ между процессами.

- **Файловый (подсистема) Менеджер** предоставляет процессам интерфейс файлового доступа к дискам (hdd). **Особое значение** менеджера файлов состоит в том, что с помощью файлового интерфейса процессам предоставляется доступ к другим подсистемам. Например, доступ к CD/DVD-накопителя через `/dev/sr0`, к мыши через `/dev/input/mouse`. Доступ к физ памяти через /dev/mem, доступ процессов к страницам памяти друг друга через `/proc/PID/mem`, а доступ к обнаруженным Ядром устройств через псевдофайловую систему `sysfs` каталога `/sys`.

Кроме указанных задач все менеджеры в совокупности предоставляют процессам средства межпроцессорного взаимодействия, такие как **сигналы**, **каналы**, **сокеты** и **разделяемая память**.



#### :diamond_shape_with_a_dot_inside: _Аппаратный уровень_

**Аппаратный уровень** состоит из всех периферийных устройств, таких как оперативная память, жесткий диск, процессор и т.д.



#### :diamond_shape_with_a_dot_inside: _Трассировка системных и библиотечных вызовов_

Для наблюдения за обращениями программ к услугам ядера, т. е. за системными вызовами, служит утилита `strace`, предна­значенная для трассировки — построения трасс выполнения той или иной программы.

![image](https://github.com/user-attachments/assets/b737c689-cfc7-49db-93cd-9d501537e0f0)



#### :diamond_shape_with_a_dot_inside: _Резюме_

Архитектура ОС Linux состоит из трех уровней: уровня пользователя, уровня ядра и аппаратного уровня. Уровни взаимодействуют между собой с помощью интерфейсов. В качестве интерфейса между уровнем пользователя и уровнем ядра выступают - **системные вызовы**. Они позволяют программам выполнять низкоуровневые операции, которые требуют привилегий ядра.

**Примеры системных вызовов:**

**Файловые операции**: open() (открытие файла), read() (чтение данных из файла), write() (запись данных в файл), close() (закрытие файла).  
**Процессные операции**: fork() (создание нового процесса путём клонирования текущего процесса), exec() (замена текущего процесса новым процессом), wait() (ожидание завершения дочернего процесса).  
**Операции с памятью**: brk() (изменение размера сегмента данных процесса), mmap() (отображение файлов или устройств в память).  
**Сетевые операции**: socket() (создание нового сокета), bind() (привязка сокета к адресу), listen() (ожидание соединений на сокете), accept() (принятие входящего соединения).

Интерфейсом между уровнем ядра и аппаратным уровнем выступают **драйверы устройств**. Ядро обрабатывает аппаратные прерывания, сигналы, поступающие от периферии, процессора, памяти и так далее. Кроме того, **система управления устройствами** на уровне ядра действует как низкоуровневый интерфейс между оборудованием и операционной системой.

![image](https://github.com/user-attachments/assets/d8cda107-d0c9-4377-93d7-7f70d5fd13a3)
![image](https://github.com/user-attachments/assets/65ec448f-06e7-4233-a62c-4e3f927dbf5b)
![image](https://github.com/user-attachments/assets/9ec41b9e-aef4-48fb-b026-068bc4871735)

</details>





<details>
<p><summary><b> :large_blue_diamond: ГЛАВА 2. Пользовательское окружение ОС Linux</b></summary></p>

На персональных ПК, для взаимодействия с пользователем используется клавиатура, видео-адаптер и монитор, которые формируют консоль. Консоль используется драйвером виртуальных интерфейсов для организации нескольких физических терминалов.

Узнать имя текущего терминала (а точнее, имя спец файла устройства) можно командой `tty`, а список всех терминальных входов пользователей - команды `users`, `who`, `w`.

![image](https://github.com/user-attachments/assets/f2ee75e7-aa0e-4f72-98d2-193ef98d287b)
![image](https://github.com/user-attachments/assets/865566e4-7a8f-40fe-9126-f8bb68870b40)

### [:diamond_shape_with_a_dot_inside:](#toc) Пользователи и группы

```ruby
finger ikozhuhar
```

![image](https://github.com/user-attachments/assets/5cba7054-6762-47c1-8386-d48d57eec97b)

</details>






<details>
<p><summary><b> :large_blue_diamond: ГЛАВА 3. Подсистема управления файлами и вводом-выводом</b></summary></p>

Linux, базируются на одной универсальной идее о том, что информация есть файл, откуда бы эта информация в систему ни поступала. При помощи файлов обеспечивается доступ к информации на устройствах хранения, к информации с устройств связи принимаемой в реальном времени, информации из любых других источников.

- **Одни файлы обеспечивают доступ к информации**, хранимой на разнообразных носителях: магнитных дисках и дискетах, оптических CD/DVD/BD, твердотельных «дисках» и пр.
- **Другие файлы обеспечивают доступ к информации**, поступающей из/в устройств ввода-вывода — клавиатур, манипуляторов «мышь», тачпадов, сенсорных экранов, последовательных и параллельных портов, видеокамер, звуковых карт и пр.
- **Особенные файлы обеспечивают доступ к информации** о сущностях ядра операционной системы (процессы, нити, модули, драйвера и пр.).

<br/>

### [:diamond_shape_with_a_dot_inside:](#toc) 3.1 Путевые имена файлов

Например, 
- Каталог `/bin (binary)` предназначен для системных программ общего назначения.
- Каталог `/usr/bin` - предназначен для прикладных программ общего назначения.
- Каталог `/usr/local/bin` - предназначен для локально установленных прикладных программ общего назначения, а каталог `bin` внутри домашних каталогов пользователей - для программ персонального назначения.
- Каталоги `/sbin`, `/usr/sbin`, `/usr/local/sbin` - предназначены для программ системного администрирования, системных прикладных, и локально установленных. Расшифровываются каталоги как `superuser binary`.
- Каталог `/home` является контейнером домашних каталогов пользователей.
- Каталог `/var` предназначен для динамических данных таких как логи и почта, а каталог `/tmp` для временных файлов.
- Каталоги `/dev`, `/proc`, `/sys` содержат специальные файлы устройств и файлы псевдофайловых систем `proc` и `sysfs`

<br />

### [:diamond_shape_with_a_dot_inside:](#toc) 3.2 Типы файлов

Файлы различаются по типам указывающим источник информации:

- **Обычные файлы** и каталоги обеспечивают хранение информации на разных носителях.
- **Специальные файлы** позволяют обмениватся информацией с разными аппаратными устройствами ввода-вывода.
- **Именнованые каналы и файловые сокеты** предназначены для обмена информацией между процессом одной программы и процессами других программ.

Символом `-` обозначается обычный файл, символом `b` или `с` — специальные файлы блочного (`block`) или символьного (`character`) устройства, символом `р` — именованный канал (`pipe`), символом `s` - сокет (socket), а символом `I` — символическая ссылка (`link`).

**Обычные файлы** хранят в себе пользовательскую информацию: текст, изображения, звук, видео и прочие данные в виде набора байтов.

```ruby
file /usr/share/man/man1/file.1.gz
file 72fc0484-9d38-4743-a48f-97bcf4caf061.jpeg
```

![image](https://github.com/user-attachments/assets/caf8ff09-ac5a-4b17-9c42-5e93cb4a342c)


**Каталоги** в отличии от файлов содержат в себе таблицу имен файлов и соответствующих им номеров индексных дескрипторов (`inode`). Каждый `inode` содержит метаданные и список стандартных свойств файла и его местоположение в файловой системе. Полный набор метаданных позволяет получить команда `stat`.

```ruby
ls -ai
stat .profile
```

![image](https://github.com/user-attachments/assets/a5c3408b-66fa-4788-9f6f-68464c78a501)

<br>

### Ссылки

**Жёсткая ссылка** это два разных имени указывающие на одни и те же метаданные. При добавлении нового имени файла в метаданных увеличивается счётчик количества имён. При удалении, сначала удаляется имя файла, потом именьшается счётчик и только после этого высвобожжаются метаданные. Удаление не происходит _вообще_ если у файла остались имена (жёсткие сылки) и не происходит сразу если файл открыт каким-то процессом. Помиотреть кем открыт файл можно командой `lsof astra-linux-l.3-special-edition-snolensk-disk3-devel.iso`

**Чтобы создать жёсткую ссылку в Linux, нужно использовать команду ln без опции -s.**

Синтаксис команды: `ln целевой_файл имя_жёсткой_ссылки.`

Например, чтобы создать жёсткую ссылку с именем `hardlinktofile` на файл `myfile.txt`, нужно выполнить команду `ln myfile.txt hardlinktofile`.

Ограничением жёсткой ссылки является её локальность в рамках своей файловой системы в силу значимости номеров индексных дескрипторов(`inode`).

Для преодоления этого ограничения существуют символические ссылки, которые содержат в себе путевое имя к целевого файлу. В случае удаления целевого файла символическая ссылка будет указывать в _никуда_ и называться **сиротой**.

**Чтобы создать символическую ссылку в Linux, нужно использовать команду ln с опцией -s.**

**Для создания ссылки на файл** нужно открыть терминал и ввести команду: `ln -s source_file symbolic_link`. В строке `source_file` указать имя существующего файла, для которого нужно создать символическую ссылку, а `symbolic_link` — имя самой ссылки. Если не указать второе имя, команда `ln` создаст новую ссылку в текущей папке.

**Для создания ссылки на папку** команда такая же, только первым параметром указывается имя папки, а вторым — ссылка. Например, чтобы создать символическую ссылку из папки `/mnt/my_drive/movies` в папку `~/my_movies`, нужно ввести `ln -s /mnt/my_drive/movies ~/my_movies`.

Чтобы убедиться, что символическая ссылка создана успешно, можно ввести команду `ls -l symbolic_link`. 

<br>

### Специальные файлы устройств

Специальные файлы предназначены для ввода и вывода данных с аппаратных устройств. Настоящую работу по вводу-выводу делает **драйвер**, а специальные файлы являются "порталами" связи с драйверами. Различают символьные и блочные специальные файлы устройств.

**Блочные специальные файлы** — это тип файлов в UNIX-подобных операционных системах, которые обеспечивают буферизованный доступ к аппаратным компонентам, таким как жёсткие диски, съёмные носители и т.д..

Они используются для передачи данных, разделённых на пакеты фиксированной длины — блоки, размером 512, 1024, 4096 или 8192 байтов. Типичным примером подобных устройств являются магнитные диски.

В Linux **блочные файлы** обозначаются буквой `b`, а символьные — буквой `c`. Они находятся в каталоге `/dev` (`от англ. devices — устройства`), который содержит интерфейсы работы с драйверами ядра. 

**Символьные специальные файлы** обеспечивают небуферизованный доступ к ядру и аппаратным компонентам, таким как клавиатура, монитор, принтеры. Это значит, что они могут передавать за раз лишь один символ.

Примерами таких устройств являются терминалы (в том числе, системная консоль), последовательные устройства, некоторые виды магнитных лент.

Для символьных файлов предусмотрена буква `c (character)`. Определить их можно по первому символу в выводе команды `ls -l`.

Специальные файлы не хранят данные на диске, а только передают их между процессами и устройствами. Поэтому размер специальных файлов всегда равен нулю.

**Все драйверы ядра** пронумерованы главными (`major`) числами, а аппаратные устройства, находящиеся под их управлением, дополнительными (`minor`) числами.

**Major и minor числа нужны для идентификации устройств в операционной системе Linux.**

![image](https://github.com/user-attachments/assets/6cfc4b0f-24a3-4b9e-988a-bbe5d32f2d55)

https://www.kernel.org/doc/Documentation/admin-guide/devices.txt

**Major число** указывает, какой драйвер используется для доступа к оборудованию. Каждому драйверу назначается уникальный `major` номер, все файлы устройств с одинаковым `major` номером управляются одним и тем же драйвером.

**Minor число** используется драйвером для различения между различными устройствами, которые он контролирует. Например, если у жёсткого диска четыре раздела, то у каждого раздела будут отдельные `minor` номера, в то время как `major` число будет одним, потому что для всех разделов используется один и тот же драйвер хранилища.

Таким образом, **major число определяет тип устройства, а minor число — конкретное устройство в рамках этого типа**. 

```ruby
# Показывает драйверы и другие модули, которые загружены в данный момент
lsmod

# Информация о заданном модуле
modinfo
```

<br>

### Именованные каналы и файловые сокеты

Именованные каналы и файловые сокеты являются простейшими средствами межпроцессного взаимодействия (IPC, InterProcess Communication) и служат программам для обмена информацией между собой.

**Каналы и сокеты** используют для передачи данных от процесса к процессу оперативную память ядра операционной системы, а не память накопителя, как обычные файлы.

Основное отличие **именованного канала** от **сокета** состоит в способе передачи данных. Через именованный канал организуется однонаправленная (симплексная) передача без мультиплексирования, а через сокет — двунаправленная (дуплексная) мультиплексированная передача.

**Именованный канал** обычно используют при взаимодействии процессов по схеме «поставщик — потребитель» (producer-consumer), когда один потребитель принимает информацию от одного поставщика. Например, программы `halt(8)`, `shutdown(8)`, `reboot(8)`, `poweroff(8)` и `telinit(8)` передавали ранее посредством именованного канала `/dev/initctl` команды перезагрузки, выключения питания и другие диспетчеру `init(8)1`, который и выполнял соответствующие действия.

**Сокет используют** при взаимодействии по схеме «клиент — сервер» (client-server), т. е. один сервер принимает и отправляет информацию от многих и ко многим (одновременно) клиентам. Например, в целях сбора событий, службы операционной системы `сгоn(8)`, служба печати `cupsd(8)` и `logger(1)` передают посредством файлового сокета `/dev/log` сообщения о событиях службе журнализации `systemd-journald(8)`.

<br>

### [:diamond_shape_with_a_dot_inside:](#toc) 3.3 Файловые дескрипторы

**Файловый дескриптор — это натуральное число, которое является идентификатором потока ввода-вывода**. Дескриптор может быть связан с файлом, каталогом, сокетом. 

Основными операциями, предоставляемыми ядром программам для работы с файлами, являются системные вызовы (интерфейсами) `open(2)`, `read(2)`, `write(2)` и `close(2)`. Эти системные вызовы предназначены для открытия и закрытия файла, для чтения из файла и записи в файл. Дополнительный системный вызов `ioctl(2) (input output control)` используется для управления драйверами устройств и, как следствие, применяется в основном для специальных файлов устройств.

При запросе процесса на открытие файла системным вызовом `ореn(2)` для запросившего процесса создается так называемый **файловый дескриптор** (описатель, от англ, descriptor). Файловый дескриптор «содержит» информацию, описывающую файл, например `inode` файла, номера `major` и `minor` устройства, на котором располагается файловая система файла, режим открытия файла и прочую служебную информа­ цию. При последующих операциях `read(2)` и `write(2)` доступ к самим данным файла происходит с использованием файлового дескриптора.

Когда процесс открывает файл или устройство, операционная система создаёт дескриптор файла для отслеживания открытого ресурса. Этот дескриптор служит ссылкой, через которую процесс может читать, записывать или управлять этим ресурсом.

По умолчанию Unix-оболочки связывают файловый дескриптор `0` с потоком стандартного ввода (клавиатура), файловый дескриптор `1` — с потоком стандартного вывода (терминал), и файловый дескриптор `2` — со стандартным выводом ошибок (диагностические и отладочные сообщения, информация об ошибках). 

```ruby
lsof -p $$
```

![image](https://github.com/user-attachments/assets/12ab8a1f-f18c-405b-bea3-ca741dee8246)

<br>

### [:diamond_shape_with_a_dot_inside:](#toc) 3.4 Файловые системы

#### Дисковые файловые системы

Разные файловые системы fs(5), как упоминалось ранее, предназначены для хранения информации на внешних носителях и преследуют различные цели, например обеспечивают надежное хранение. До сих пор носителями информации являются магнитные или оптические диски,- благодаря чему файловые системы, размещаемые на них, зачастую называются «дисковыми» файловыми системами.

В Linux на текущий момент времени используются «родные» файловые системы Ext2, Ext3 и Ext4, специально разработанные ReiserFS и Reiser4, а также заимствованные XFS и JFS.

Для CD/DVD-дисков, применяются файловые системы ISO 9660 и udf. Для USB-flash-накопителей в используются заимствованные файловые системы FAT и NTFS в силу использования этих накопителей как мобильных средств переноса данных между разными компьютерами с различными операционными системами.

### Сетевые файловые системы

Сетевые файловые системы, равно как и дисковые, обеспечивают хранение информации на внешнем носителе, которым в этом случае выступает файловый сервер, доступный по протоколу NFS, CIFS/SMB.

Одноименные файловые системы nfs и cifs/smb используются для монтирования файлов сервера в дерево каталогов клиента 

```ruby
mount -t nfs 182.168.1.16:/share/video /mnt/nas/video
mount -t cifs -о usernane=guest //182.168.1.10/share/photos /mnt/nas/photos
```

#### Специальные файловые системы

Развитие идеи файла как единицы обеспечения доступа к информации привело к тому, что абстракцию файловой системы перенесли и на другие сущности, доступ к которым стал организовываться в виде иерархии файлов. **Например, информацию о процессах, нитях и прочих сущностях ядра** операционной системы и используемых ими ресурсах предоставляет программам виде файлов **псевдофайловая система ргос(5)**. Таким же образом, **информацию об аппаратных, устройствах,
обнаруженных ядром** операционной системы на шинах PCI, USB  SCSI и пр., предоставляет **псевдофайловая система sysfs**.

```ruby
# Псевдофайловая система procfs
strace -fe open,openat uptime
cat /proc/uptime
cat /proc/loadavg

# Псевдофайловая система sysfs
strace -fe open,openat Ispci -nn
```

#### Внеядерные файловые системы

Для реализации «файлового» доступа к произвольным источниками информации используются так называемые «внеядерные» файловые системы FUSE (Filesystem in USErspace), реализуемые не ядерными модулями файловых систем (как ext4, nfs, ргос и пр.), а обычными программами, запущенными в обычных процессах и работающими вне ядра.

В примере из листинга 3.26 показано, как можно без распаковки смонтировать сжатый архив исходных текстов ядра linux-4.2.3.tar.xz и прочитать отдельный файл fuse.txt.

```ruby
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.3.9.tar.xz
file llnux-5.3.9.tar.xz
archivemount ltnux-5.3.9.tar.xz ~/mnt/archive
less linux-5.3.9/Docunentation/filesystems/fuse.txt
```

При помощи сетевого протокола SSH можно смонтировать часть дерева каталогов (домашний каталог пользователя jake) с удаленного узла jake@grex.org в каталог ~/mnt/net локального дерева каталогов.

```ruby
sshfs jake@grex.org: ~/mnt/net
```

В примере из листинга 3.29 показано, как можно монтировать файловые системы fuse друг поверх друга в стек — смонтировать содержимое дерева каталогов FTP-сервера mirror.yandex.ru, далее смонтировать содержимое ISO-образа FreeBSD-12.1-RELEASE-and64-dvdl.iso, а затем смонтировать архив исходных текстов src.txz. При чтении файла страницы руководства ls.l файловые системы будут прозрачно и на лету (!) извлекать файл из архива, архив из образа и образ с сервера без предварительных скачиваний и распаковываний.

```ruby
curlftpfs mirror.yandex.ru ~/mnt/net
fuseiso -/mt/rat/firafed/relBBSBs/ISD-]MflfE/12.VFreEBSD-32.l-FBflfiE-ard64-cKdl.iso ~/mt/ad
archivenount ~/mnt/cd/usr/freebsd-dist/src.txz ~/mnt/archive
```

#### Семантика режима доступа разных типов файлов

Права доступа `г, w, х` д л я обычных файлов представляются чем-то интуитивно понятным, но для других типов файлов это не совсем так. Например, каталог содержит список имен файлов, поэтому право `w` для каталога — это право записи в этот список и право стирания из этого списка, что трансформируется в право удаления файлов из каталога и создания файлов в каталоге. Аналогично, право `r` для каталога — это право просмотра списка имен его файлов. И наконец, право `х` для каталога является правом прохода в каталог, т. е. позволяет обращаться к файлам внутри каталога по их имени.

Для жестких ссылок права доступа не существуют вовсе — они просто являются теми же правами, что и права целевого файла, в силу того что права доступа хранятся в метаданных. Для символических ссылок семантика прав сохранена такой же, как и у жестких ссылок, с тем лишь различием, что права символических ссылок существуют отдельно от целевых файлов, но никогда не проверяются (см. symlink(7)). Для изменения прав доступа самих символических ссылок даже не существует специальной команды — при использовании chmod(1) со ссылкой всегда будут изменяться права целевого файла.

Для специальных файлов устройств, именованных каналов и сокетов право `х` не определено, а права `r` и `w` стоит воспринимать как права ввода и вывода информации на устройство и как права передачи и приема информации через средство взаимодействия.


<br>

### [:diamond_shape_with_a_dot_inside:](#toc) 3.5. Дискреционное разграничение доступа

**Дискреционные механизмы разграничения доступа** используются для разграничения прав доступа процессов как обычных пользователей, так и для ограничения прав системных программ (например, служб операционной системы), которые работают от лица псевдопользовательских учетных записей.

**По умолчанию пользователем-владельцем файла** становится пользователь, создавший файл, а группой-владельцем файла становится его первичная группа. Изменить пользователя-владельца файлов может только суперпользователь root при помощи команды chown(1), а группу-владельца — владелец файла при помощи команды chgrp(l), но только на ту к которой он сам принадлежит.

**Проверка режима** доступа при операциях с файлами проверяется «слева направо» до первого совпадения. Если пользователь, осуществляющий операцию с файлом, является его владельцем, тогда используются только права владельца. В противном случае проверяется членство пользователя, осуществляющего операцию с файлом, в группе-владельцев файла, и тогда используются только права группы-владельцев. В других случаях используются права для всех остальных, а для суперпользователя root вообще никакие проверки не осуществляются.

Назначается режим доступа файлов при их создании программой, создавшей файл, исходя из назначения файла, но с учетом пожеланий (точнее, нежеланий) пользователя. Пользователь может выразить свое нежелание назначать вновь создаваемым файлам те или иные права доступа для тех или иных субъектов, установив так называемую реверсивную маску доступа:

```ruby
umask
umask -S
unask g-w,o-rwx
```

Изменять режима доступа разрешено непосредственному пользователю — владельцу файла, но не членами группы-владельцев.


#### Семантика режима доступа разных типов файлов

Права доступа `г`, `w`, `х` для обычных файлов представляются чем-то интуитивно понятным, но для других типов файлов это не совсем так. Например, каталог содержит список имен файлов, поэтому право `w` для каталога — это право записи в этот список и право стирания из этого списка, что трансформируется в право удаления файлов из каталога и создания файлов в каталоге. Аналогично, право `r` для каталога — это право просмотра списка имен его файлов. И наконец, право `х` для каталога является правом прохода в каталог, т. е. позволяет обращаться к файлам внутри каталога по их имени.

**Для жестких ссылок** права доступа не существуют вовсе —; они просто являются теми же правами, что и права целевого файла, в силу того что права доступа хранятся в метаданных. **Для символических ссылок** семантика прав сохранена такой же, как и у жестких ссылок, с тем лишь различием, что права символических ссылок существуют отдельно от целевых файлов, но никогда не проверяются (см. symlink(7)). Для изменения прав доступа самих символических ссылок даже не существует специальной команды — при использовании chmod(l) со ссылкой всегда будут изменяться права целевого файла.

**Для специальных файлов устройств**, именованных каналов и сокетов право `х` не определено, а права `г` и `w` стоит воспринимать как права ввода и вывода информации на устройство и как права передачи и приема информации через средство взаимодействия.

### 🔥 Дополнительные атрибуты

Помимо базовых прав доступа `г`, `w` и `х`, для решения отдельных задач разграничения доступа используют дополнительные атрибуты `s`, Set user/group ID (`SUID` Set User ID или `SGID`, Set Group ID) — **атрибут неявного делегирования полномочий** и `t`, `sTicky` — «липучка», **атрибут ограниченного удаления**.

Типичной задачей, требующей неявного делегирования полномочий, является проблема невозможности изменения пользователями свойств своих учетных записей, которые хранятся в двух файлах-таблицах — `passwd(5)` и `shadow(5)`, доступных на запись (и чтение) только суперпользователю `root`. Однако команды `passwd(1)`, `chsh(1)` и `chfn(1)`, будучи запущены обычным пользователем, прекрасно изменяют пароль в таблице `/etc/shadow` и свойства пользовательской записи в таблице `/etc/passwd` за счет передачи полномочий пользователя — владельца программы тому пользователю, который ее запускает.

```ruby
ls -la /etc/passwd /etc/shadow
ls -la /usr/bin/passwd /usr/bin/chfn
```

За счет использования атрибута `SUID` получается, что пользователям, запускающим программы `chfn(1)`, `chsh(1)` и `passwd(1)`, для их исполнения временно делегируются права владельца этих программ **(суперпользователя root)** так, как будто сам суперпользователь их запустил.

Именно за счет механизма `SUID/SGID` различные команды позволяют обычным, непривилегированным пользователям, выполнять сугубо суперпользовательские действия. Так, например, `su(1)` и `sudo(1)` позволяют выполнять команды одним пользователям от лица других пользователей, `mount(8)`, `umount(8)` и `fusermount(l)` — монтировать и размонтировать файловые системы, `ping(8)` и` traceroute(l)` — выполнять диагностику сетевого взаимодействия, `at(1)` и `crontab(1)` — сохранять в «системных» каталогах отложенные и периодические задания, и т. д

Однако для каталогов атрибут `SGID` имеет совсем другой смысл. По умолчанию владельцем файла становится тот пользователь (и его первичная группа), который запустил программу, создавшую файл. Но для файлов, создаваемых в «общих» для какой-то группы пользователей, каталогах, логичнее было бы назначать группой-владельцем создаваемых файлов эту общую группу.

```ruby
bubblegum@ubuntu:~$ cd /srv/kingdon
bubblegum@ubuntu:/srv$ id
uid=1005(bubblegum) gid=1005(bubblegum) rpynnw=1005(bubblegum),1007(candy)

bubblegum@ubuntu:/srv/kingdom$ chgrp candy .
bubblegum@ubuntu:/srv/klngdom$ chmod g-ws .
bubblegum@ubuntu:/srv/kingdom$ Is -Id .
```

В примере выше за счет SGID-атрибута каталога владельцем всех файлов, помещаемых в этот каталог, автоматически назначается группа-владелец самого каталога, а создатель (владелец) файла может теперь назначать нужные права доступа для всех членов этой группы к своему файлу — либо неявно при помощи реверсивной маски `(umask)` доступа, либо явно при помощи команды `chmod(1)`

**sTicky атрибут-«липучка»** `t (sTicky)` служит для ограничения действия базового разрешения `w` записи в каталоге. Например, временный каталог `/tmp` предназначается для хранения временных файлов любых пользователей и поэтому доступен на запись всем пользователям. Однако право записи в каталог дает возможность не только создавать в нем новые файлы, но и удалять любые существующие файлы (любых пользователей), что совсем не кажется логичным. Именно атрибут `t` ограничивает возможность удалять чужие файлы, т. е. файлы, не принадлежащие пользователю, пытающемуся их удалить.

```ruby
finn@ubuntu:/srv/kingdom$ id
uid=1001(finn) gid=1001(finn) rpynnbt=1001(flnn),1007(candy)
finn@ubuntu:/srv/klngdan$ ls -la

bubblegum@ubuntu:/srv/kingdom$ chmod +t .
bubblegum@ubuntu:/srv/klngdom$ touch bananaguard1

finn@ubuntu:/srv/kingdom$ rm bananaguardl
rm: невозможно удалить «bananaguardl»: Операция не позволена
```




<br><br><br><br><br><br><br><br>










### Резюме

**Файловая система**

**Файловая система Linux** — это способ организации, **хранения и управления данными** на разных носителях информации: жёстких дисках, SSD, USB-накопителях и других устройствах хранения.

Она определяет, как данные сохраняются, читаются, изменяются и удаляются. Файловая система также управляет метаданными: именами файлов, их размерами, разрешениями доступа и временными метками. **В Linux на каждый раздел диска можно установить свою файловую систему**, которая определяет порядок и метод организации информации.

**В основе файловой системы Linux лежит иерархическая структура**, которая напоминает дерево. Все начинается с корневого каталога «/», который служит начальной точкой для всех других файлов и каталогов.

В Линукс ВСЕ есть файл. При помощи файлов обеспечивается доступ к информации на устройствах хранения (записанной ранее), информации с устройств связи (принимаемой из каналов связи в реальном времени), информации из любых других источников. Файл, таким образом, является единицей обеспечения доступа к информации.

Виды файлов:

1. Файлы доступа к информации, хранимой на разнообразных носителях: магнитных дисках и дискетах, оптических CD/DVD/BD, твердотельных «дисках» и пр.
2. Файлы доступа к информации, поступающей из/в устройств ввода-вывода — клавиатур, манипуляторов «мышь», тачпадов, сенсорных экранов, последовательных и параллельных портов, видеокамер, звуковых карт и пр.
3. Файлы доступа к информации о сущностях ядра операционной системы (процессы, нити, модули, драйвера и пр.).


Путевые имена файлов: 

- `/bin/, /usr/bin, /usr/local/bin` - системные программы общего назначения
- `/sbin, /usr/sbin, /usr/local/sbin` - super user binaries программы системного администрирования
- `/lib, /usr/lib, /usr/local/lib` - системные и прикладные библиотеки
- `/etc` - конфигурационные файлы
- `/home` - домашнии каталоги юзеров
- `/var` - хранилище динамических данных
- `/tmp` - хранилище временных файлов
- `/dev, /proc, /sys` - файлы устройств и файлы псевдофайловых систем `proc` и `sysfs`

Типы файлов

Файлы различаются по типам указывающим на источник информации. Вот некоторые из них:

1. **Обычные файлы (символ `-`)**. Это самый обычный тип файлов, который чаще всего используется. Сюда относятся данные, текст, исходный код программ, медиаматериалы и прочее.
2. **Именованные каналы (символ `p`)**. Необходимы для межпроцессного взаимодействия, позволяя одному процессу передавать данные другому.
3. **Файлы устройств** (символы `c` и `b`). Содержат в себе символьные (char devices) и блочные (block devices) файлы устройств, которые предоставляют внешние аппаратные устройства (например, HDD, принтеры и прочие).
4. **Ссылки**. Включают два типа ссылок. **Символические ссылки**, или «симлинки», функционируют как ярлыки, указывающие на другие файлы или папки. **Жёсткие ссылки**, в свою очередь, создают альтернативные пути доступа к одним и тем же физическим данным на диске.
5. **Каталоги**. Это своего рода папки, где хранятся ссылки на файлы и другие каталоги. Они помогают организовать данные, распределяя их по разным «отсекам», чтобы было легче найти нужную информацию.
6. **Сокеты**. Специальные файлы для обмена данными между разными процессами, как внутри одной системы, так и между разными компьютерами. Это своеобразные «почтовые ящики» для программ, через которые они могут «пересылать» друг другу информацию.
7. **Двери (Doors)**. Механизм в некоторых операционных системах, предназначенный для взаимодействия между программными процессами. 
</details>





<details>
<p><summary><b> :large_blue_diamond: ГЛАВА 4. Управление процессами и памятью</b></summary></p>



</details>
