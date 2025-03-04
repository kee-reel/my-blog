---
language: ru
title: Linux. Основы работы с командной строкой
date: 2021-08-11
tag: linux
lang: ru
---

Скорее всего ты читаешь этот текст в окне браузера, а значит используешь графическую оболочку при работе с операционной системой. Сейчас ты открываешь окна и кликаешь курсором на кнопки и иконки приложений, но так было не всегда.

50 лет назад все пользователи компьютера видели перед собой примерно такую картину:

![Старый компьютер с коммандной строкой](/assets/images/old-computer-cli.png)

Представь, например, что даже какая-нибудь Евлампия Сергеевна из отдела бухгалтерии работала через командную строку, чтобы сформировать квартальный отчёт. 

Окей, раньше просто не было графических интерфейсов, и у Евлампии Сергеевны не было альтернатив -- сейчас то зачем с этим заморачиваться? Отвечаю: 

* Если ты будешь настраивать сервер, который может находиться на другом конце Земного шара, у тебя не будет альтернатив, кроме как удалённо подключиться к его консоли
* Если ты хочешь автоматизировать какой-то сценарий работы с компьютером -- самым быстрым способом будет написать скрипт, который будет исполнять прописанные тобой команды
* Если ты хочешь лучше понять как работает твой компьютер и что-то в нём улучшить -- для командной строки существует огромное множество программ, позволяющих заглянуть в самые глубинные аспекты работы твоего компьютера
* Если ты хочешь научиться комфортно работать даже на самом слабом компьютере -- командная строка на несколько порядков производительнее графического интерфейса

Если я тебя убедил, то подключай свой нейроинтерфейс, кибер-ниндзя -- сейчас ты узнаешь как:

* [Смотреть содержимое директории](#ls)
* [Переходить по директориям](#cd)
* [Создавать директории](#mkdir)
* [Создавать и редактировать файлы](#nano)
* [Копировать файлы/директории](#cp)
* [Перемещать и переименовывать файлы/директории](#mv)
* [Удалять файлы/директории](#rm)

Это является **базой**, на основе которой ты будешь управлять своим компьютером через командную строку.

# Открываем командную строку

> В английском языке есть устоявшиеся сокращения:
> * Графическая оболочка -- GUI (Graphic User Interface), графический пользовательский интерфейс
> * Командная строка -- CLI (Command Line Interface), интерфейс командной строки
>
> Также, командную строку называют "Терминал" ("Terminal") и "Консоль" ("Console").

Независимо от того, сидишь ты на MacOS, Windows или Linux, "под капотом" графическая оболочка и командная строка делают одно и то же -- взаимодействуют с операционной системой.

![Графическая оболочка и командная строка делают одно и то же](/assets/images/gui-cli-os.png)

Окей, как двигать курсор и нажимать на кнопки ты уже знаешь, а вот какие команды надо в этой консоли писать совершенно не ясно. Тут никакого волшебства -- придётся запоминать команды. Это не так сложно как может показаться на первый взгляд, и через пару-десятков повторений ты будешь писать их на автомате.

Чтобы открыть командную строку нужно нажать комбинацию `Ctrl+Alt+T` или найти приложение "Terminal" среди установленных приложений.

# Приглашение командной строки и домашняя директория

Появится окно командной строки, в котором ты увидишь надпись вроде:

```bash
kee-reel@blog:~$
```
Вот как она расшифровывается:

| Символ | Значение |
|---|---|
| kee-reel | имя пользователя, указанное при установке ОС
| @ | разделяющий символ
| home-computer | имя компьютера, указанное при установке ОС
| : | разделяющий символ |
| ~ | текущая директория |
| $ | текущий режим доступа ($ - обычный пользователь, # - администратор) |

Эта надпись называется Prompt (на русский можно перевести как "приглашение", но такой термин не используется, а просто говорят "промт").

Самой важной информацией тут является то, что находится на месте `~` -- так как нам необходимо как-то понимать где мы сейчас находимся в файловой системе.

Вот, например, как текущая директория выглядит в графической оболочке ОС Windows:

![Windows Explorer](/assets/images/windows-explorer.png)

А в командной строке это выглядело бы так:

```bash
kee-reel@blog:~/Documents/Андрей/Моя Работа$
```

По-умолчанию, когда ты открываешь командную строку, ты находишься в "домашней" директории текущего пользователя. В ней ещё лежат всякие папки, вроде "Documents", "Downloads" и прочее.

Чтобы сократить запись в Prompt'е, домашняя директория обозначается символом "~" (тильда). На самом деле домашняя директория "~" находится в "/home/kee-reel" (у каждого пользователя будет своя папка в "/home").

> Если ты не понял что значат все эти "/home", то поищи статью про структуру файловой системы в Linux.

# Выполняем команды

Если ты захочешь написать какую-то команду, то она будет дописываться после Prompt'а:

```bash
kee-reel@blog:~$ моя-команда
```

{{< ref ls >}}
### ls - содержимое директории

`ls` (от `LiSt`) -- узнать какие папки/файлы у нас есть в текущей директории:

```bash
kee-reel@blog:~$ ls
Desktop
Documents
Downloads
Music
```

> Также, можно указать путь до директории, в которой надо вывести список файлов: `ls Documents/OtherFolder/`

{{< ref cd >}}
### cd - сменить директорию

`cd` (от `Change Directory`) -- сменить директорию относительно текущей:

Давай перейдём в папку `Documents`:

```bash
kee-reel@blog:~$ cd Documents
```

Можно увидеть, что текущая в директория в prompt тоже изменилась:

```bash
kee-reel@blog:~/Documents$ 
```

Чтобы вернуться в предыдующую директорию, надо написать:

```bash
kee-reel@blog:~$ cd ..
```

Prompt изменится соответствующе:

```bash
kee-reel@blog:~$ 
```

> Можно указывать переход сразу через несколько папок: cd Documents/MyFolder/Programming

Иногда тебе надо пройти несколько директорий, но ты не знаешь точный путь. В этом случае **ТАК НЕ НАДО ДЕЛАТЬ**:

```bash
kee-reel@blog:~$ cd folder1
kee-reel@blog:~/folder1$ ls
folder2
some_other_folder

kee-reel@blog:~/folder1$ cd folder2
kee-reel@blog:~/folder1/folder2$ ls
folder3
freakn_folder666

kee-reel@blog:~/folder1/folder2$ cd folder3
kee-reel@blog:~/folder1/folder2/folder3$
```

В этой ситуации, просто нажми 2 раза клавишу "Tab", когда напишешь имя директории:

```bash
kee-reel@blog:~$ cd folder1                 # Нажал 2х"Tab"
folder2                                     # Показались директории в "folder1"
some_other_folder

kee-reel@blog:~$ cd folder2/f               # Написал "f", нажал 2х"Tab"
kee-reel@blog:~$ cd folder1/folder2         # "folder2" сам автодополнился, нажимаю 2х"Tab" ещё раз
folder3
freakn_folder666

kee-reel@blog:~$ cd folder1/folder2/f       # Написал "f", нажал 2х"Tab"
folder3                                     # Мне вывалились те же самые директории, 
freakn_folder666                            # потому что первая буква у них одинаковая

kee-reel@blog:~$ cd folder1/folder2/fo      # Написал "fo", нажал 2х"Tab"
kee-reel@blog:~$ cd folder1/folder2/folder3 # ???
kee-reel@blog:~/folder1/folder2/folder3$    # PROFIT!
```

> В таком формате сложно передать как это работает на деле -- просто попробуй, тебе понравится!

{{< ref mkdir >}}
### mkdir - создать директорию

`mkdir` (от `MaKe DIRectory`) -- создать новую папку в текущей директории:

```bash
kee-reel@blog:~$ mkdir ultra
```

Её можно увидеть через команду `ls`:

```bash
kee-reel@blog:~$ ls
Desktop
Documents
Downloads
Music
ultra
```

{{< ref nano >}}
### nano - текстовый редактор

Скорее всего у тебя уже установлен редактор `nano` -- это консольный текстовый редактор.

Давай создадим с его помощью файл `test.txt` в папке `~/ultra`:

```bash
kee-reel@blog:~/ultra$ nano test.txt
```

Откроется окно редактора:

![Редактор nano](/assets/images/nano-editor.png)

Попробуй написать там что угодно. Ввод работает как в любом другом редакторе -- печатаем буквы, создаём новую строку через `Enter`, перемещаем курсор стрелочками клавиатуры, стираем через `Backspace` и `Delete`.

Когда наиграишься с файлом, закрой его -- для этого надо нажать комбинацию `Ctrl+X`. При этом тебя спросят:

```bash
Save modified buffer?
Y Yes
N No        ^X Cancel
```

Нажми `Y`, чтобы сохранить, `N` -- закрыть, но не сохранять, `Ctrl+X` -- отменить закрытие.

Допустим ты нажал `Y` -- тебя спросят про имя файла, с которым ты хочешь его сохранить:

`File name to Write: test.txt`

Вроде всё устраивает -- жми `Enter`.

Всё, файл сохранён -- попробуй вывести список файлов:

```bash
kee-reel@blog:~/ultra$ ls
test.txt
```

{{< ref cp >}}
### cp - копировать файл или папку

`cp` (от `CoPy`) -- скопировать файл или папку:

```bash
kee-reel@blog:~/ultra$ cp test.txt test_1.txt
```

Можно проверить:

```bash
kee-reel@blog:~/ultra$ ls
test_1.txt
test.txt
```

Чтобы скопировать папку, надо добавить опцию `-r`:

```bash
kee-reel@blog:~$ cp -r ultra mega # сделал копию папки ultra с названием mega
kee-reel@blog:~$ cd mega          # перешёл в папку mega
kee-reel@blog:~/mega$ ls          # вывел содержимое папки mega
test_1.txt
test.txt
```

Опции программы, вроде `-r`, встречаются довольно часто -- это просто дополнительные спецификаторы, которые позволяют управлять поведением программы. В дальнейшем мы познакомимся с нимим поближе.

{{< ref mv >}}
### mv - переместить файл или папку

`mv` (от `MoVe`) -- переместить файл или папку:

```bash
kee-reel@blog:~/ultra$ mv test.txt ../    # переместил файл test.txt в вышестоящую папку (там домашняя папка)
```

Кроме перемещения эта команда может ещё переименовывать файлы:

```bash
kee-reel@blog:~/ultra$ mv test_1.txt test_666.txt    # переименовал файл test.txt в файл test_666.txt
```

Если надо переместить папку, то опцию `-r` указывать не надо (в отличии от `cp`):

```bash
kee-reel@blog:~$ mv mega super-mega    # переименовал папку mega в папку super-mega
```

{{< ref rm >}}
### rm - удалить файл или папку

`rm` (от `ReMove`) -- удалить файл или папку:

```bash
kee-reel@blog:~/ultra$ rm test_666.txt    # удалил файл test_666.txt
```

Чтобы удалить папку **со всеми файлами в ней**, надо указать опцию `-r`:

```bash
kee-reel@blog:~$ rm -r ultra    # удалил папку ultra
```

# **НИКОГДА** не исполняй команду "rm" для директории, начинающейся со "/" -- так ты удалишь все файлы на компьютере

# Вот так **НЕЛЬЗЯ** делать: "rm -rf /"
> Флаг "-f" позволяет не вызывать запрос подтверждения при удалении системных файлов.

![rm rf](/assets/images/rmrf.png)


# [](#header-1)Вывод

Итого, ты выучил 7 команд:

* ls - содержимое директории
* cd - сменить директорию
* mkdir - создать директорю
* nano - текстовый редактор
* cp - копировать файл/директорию
* mv - переместить или переименовать файл/директорию
* rm - удалить файл/директорию

Если ты всё понял, то это супер-круто, если нет -- попробуй ещё поиграться с командами.

Дальше мы посмотрим какие вообще директории существуют в Linux -- это не займёт много времени, и положительно скажется общем понимании работы операционной системы.

Если что -- пиши, я помогу и постараюсь объяснить лучше.
