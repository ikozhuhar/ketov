<p align="center">
<img src="https://github.com/ikozhuhar/ketov/blob/main/img/ketov-linux.png">
</p>

<details>
<p><summary><b> :large_blue_diamond: Глава 1. Архитектура ОС Linux</b></summary></p>

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
<p><summary><b> :large_blue_diamond: Глава 2. Пользовательское окружение ОС Linux</b></summary></p>

На персональных ПК, для взаимодействия с пользователем используется клавиатура, видео-адаптер и монитор, которые формируют консоль. Консоль используется драйвером виртуальных интерфейсов для организации нескольких физических терминалов.

Узнать имя текущего терминала (а точнее, имя спец файла устройства) можно командой `tty`, а список всех терминальных входов пользователей - команды `users`, `who`, `w`.

![image](https://github.com/user-attachments/assets/f2ee75e7-aa0e-4f72-98d2-193ef98d287b)
![image](https://github.com/user-attachments/assets/865566e4-7a8f-40fe-9126-f8bb68870b40)

### Пользователи и группы

```ruby
finger ikozhuhar
```

<br/>

### Подсистема управления файлами и вводом-выводом

Linux, базируются на одной универсальной идее о том, что информация есть файл, откуда бы эта информация в систему ни поступала. 
- **Одни файлы обеспечивают доступ к информации**, хранимой на разнообразных носителях: магнитных дисках и дискетах, оптических CD/DVD/BD, твердотельных «дисках» и пр.
- **Другие файлы обеспечивают доступ к информации**, поступающей из/в устройств ввода-вывода — клавиатур, манипуляторов «мышь», тачпадов, сенсорных экранов, последовательных и параллельных портов, видеокамер, звуковых карт и пр.
- **Особенные файлы обеспечивают доступ к информации** о сущностях ядра операционной системы (процессы, нити, модули, драйвера и пр.).


</details>
