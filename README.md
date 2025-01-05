<p align="center">
<img src="https://github.com/ikozhuhar/ketov/blob/main/img/ketov-linux.png">
</p>

<details>
<p><summary><b> :large_blue_diamond: Глава 1. Архитектура ОС Linux</b></summary></p>

В Линукс два главных режима работы: kernel space и user space. Главное отличие в оежимах работы состоит в привилегиях доступа к аппаратных ресурсам. 

#### Компоненты User Space 

Kernel space обеспечивает распределение ресурсов между пользователями и предоставляют базовый интерфейс для доступ к ресурсам.

Функции ядра доступны в user mode с помощью системных вызовов. Системные вызовы выполняются в ядре, а вызывается из user space с помощью библиотеки **libc.so**. 

Функции выполняющиеся в user space доступны с помощью библиотеках вызовов и выполняются в самих библиотеках, например, **libz.so** и  **libbz2.so**

#### Компоненты Ядра

Компоненты Ядра в основном обеспечивают распределение ресурсов, что приводит к появлению **менеджеров** _процессов_, _памяти_, _ввода-вывода_ и _менеджера файловой системы_.

**Менеджер процессов** распределяет время ЦП между выполняющимися задачами.

**Менеджер ввода-вывода** распределяет доступ к устройствам ввода-вывода между процессоми.

**Менеджер памяти** распределяет пространство ОЗУ между процессами.

**Файловый Менеджер** предоставляет процессам интерфейс файлового доступа к hdd.  Особое значение менеджера файлов состоит в том, что с помощью файлового интерфейса процессам предоставляется доступ к другим подсистемам. Например, доступ к CD/DVD-накопителя через `/dev/sr0`, к мыши через `/dev/input/mouse`. Доступ к физ памяти через /dev/mem, доступ роцессов  к страницам памяти друг друга через /proc/PID/mem, а доступ к обнаруженным Ядром устройств через псевдофайловую систему `sysfs` каталога `/sys`.

Кроме указанных задач все менеджеры в совокупности предоставляют процессам средства межпроцессорного взаимодействия, такие как сигналы, каналы, сокеты и разделяемая память.

</details>



<details>
<summary><b> :large_blue_diamond: Глава 2. Пользовательское окружение ОС Linux</b></summary>

На персональных ПК, для взаимодействия с пользователем используется клавиатура, видео-адаптер и монитор, которые формируют консоль. Консоль используется драйвером виртуальных интерфейсов для организации нескольких физических терминалов.

Узнать имя текущего терминала (а точнее, имя спец файла устройства) можно командой tty, а список всех терминальный входов пользователей - команды users, who, w.

</details>
