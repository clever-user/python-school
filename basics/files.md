# Управління файлами в  Python

## Що таке файли, каталоги та шляхи?

Це прості речі, які багато користувачів комп'ютера вже знають, але я їх розгляну, щоб переконатися, що ви їх також знаєте.

### Файли

- У кожного файла є **ім‘я’**, наприклад `hello.py`, `mytext.txt` або
    `coolimage.png`. Зазвичай ім'я закінчується **розширенням**
    яке описує вміст, наприклад, `py` для Python,` txt` для тексту або `png`
    для "портативної мережевої графіки".
- Тільки імена, що ідентифікують файли, не можуть існувати два файли з однаковим ім'ям. Ось чому у файлів також є
    **розташування**. Ми поговоримо про це трохи більше.
- Файли мають **вміст **, який складається з [байтів](https://www.youtube.com/watch?v=Dnd28lQHquU).

### Директорії/папки

Папки є способом групування файлів. У них також є ім'я та місце розташування як у файла, але замість того, щоб вони містили дані безпосередньо як файли, вони містять інші файли та каталоги.

### Шляхи

Папки та файли мають шлях, наприклад, `C:\Users\me\hello.py`. Це просто означає, що є папка з назвою `C:`, а всередині неї є папка
яка називається `Users`, а всередині неї - папка під назвою` me` і всередині останньої є "hello.py". Наприклад:

```
C:
└── Users
    └── me
        └── hello.py
```

`C:\Users\me\hello.py` - **абсолютний шлях** .  Але є також **відносні шляхи**. Наприклад, якщо ми знаходимося в `C:\Users`,` me\hello.py`
такий самий як `C:\Users\me\hello.py`. Місце, де ми знаходимось, іноді
називається **поточний каталог **, **робочий каталог** або **поточний робочий каталог**.

До цих пір ми говорили про шляхи Windows, але не всі комп'ютери працюють Windows Наприклад, еквівалент `C:\Users\me\hello.py` є
`/home/me/hello.py` в Ubuntu, і якщо я знаходжуся в` /home`, `me/hello.py`
такий самий, як `/home/me/hello.py`.

```
/
└── home
    └── me
        └── hello.py
```

## Запис у файл

Давайте напишемо hello world у файл.

```python
>>> with open('hello.txt', 'w') as f:
...     print("Hello World!", file=f)
...
>>>
```

Здається, цей код нічого не зробив. Але насправді він створив "hello.txt" десь на нашій системі. У Windows це, ймовірно, в `C:\Users\YourName`, і в більшості інших систем він повинен бути в `/home/yourname`. Ви можете відкрити його за допомогою блокноту або будь-якого іншого простого текстового редактора, 

Так як цей код працює?

Перш за все, ми відкриваємо шлях за допомогою `open`, і це дає нам об'єкт файл Python, який призначений для змінної `f`.

```python
>>> f
<_io.TextIOWrapper name='hello.txt' mode='w' encoding='UTF-8'>
>>>
```

Об'єкти файлу не такі самі, як шляхи та імена файлів, тому, якщо ми спробуємо використовувати `'hello.txt'`, як ми використовували` f`, це не працює.

Перший аргумент, який ми передали `open`, був шлях, за яким ми хотіли записати файл. Наш шлях більше схожий на назву файлу, ніж на шлях, тому файл опинився в поточному робочому каталозі.

Другий аргумент - "w". Це коротка вказівка для написання, і це означає що ми створимо новий файл. Є ще кілька режимів, які ми можемо використовувати:

| Режим | Назва режиму | Опис режиму                                                  |
| ----- | ------------ | ------------------------------------------------------------ |
| `r`   | read         | Читання з існуючого файлу.                                   |
| `w`   | write        | Запис у файл. **Якщо файл існує, його старий вміст буде видалено**. |
| `a`   | append       | Запис у кінець файлу зі збереженням старого вмісту.          |

Але що таке `with file as f`? Це просто чудовий спосіб переконатися, що файл закривається незалежно від того, що станеться. Як ми бачимо, файл був дійсно закритий.

```python
>>> f.closed
True
>>>
```

When we had opened the file we could just print to it. The print is just
like any other print, but we also need to specify that we want to print
to the file we opened using `file=f`.

## Читання з файлу

Після відкриття файлу з режимом `r` ми можемо читати його в циклі, так само, як список. Тож давайте прочитаємо всі рядки у файлі який ми створили.

```python
>>> lines = []
>>> with open('hello.txt', 'r') as f:
...     for line in f:
...         lines.append(line)
...
>>> lines
['Hello World!\n']
>>>
```

Спроба відкрити неіснуючий файл з `w` або` a` створює файл, але роблячи з `r` дає помилку. Ми дізнаємось більше про помилки [згодом](exceptions.md).

```python
>>> with open('this-doesnt-exist.txt', 'r') as f:
...     print("It's working!")
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: 'this-doesnt-exist.txt'
>>>
```

Отже, тепер у нас є hello world у змінній `lines`, але це `['Hello World!\n']` замість `[Hello World!']`. Отже, що таке`\n`? Що воно там робить?

`\n` означає новий рядок. Зауважте, що воно має бути зі зворотною косою рискою, тож `/n` не має жодного спеціального значення, а `\n` має. Коли ми писали файл, print() насправді додав `\n` до кінця кожного рядка. Рекомендовано закінчувати вміст файлів символом нового рядка, але це не обов'язково.

Давайте подивимося, як це працює, якщо у файлі є декілька рядків.

```python
>>> with open('hello.txt', 'w') as f:
...     print("Hello one!", file=f)
...     print("Hello two!", file=f)
...     print("Hello three!", file=f)
...
>>> lines = []
>>> with open('hello.txt', 'r') as f:
...     for line in f:
...         lines.append(line)
...
>>> lines
['Hello one!\n', 'Hello two!\n', 'Hello three!\n']
>>>
```

There we go, each of our lines now ends with a `\n`. When we for
loop over the file it's divided into lines based on where the `\n`
characters are, not based on how we printed to it.

But how to get rid of that `\n`? [The rstrip string
method](https://docs.python.org/3/library/stdtypes.html#str.rstrip) is
great for this:

```python
>>> stripped = []
>>> for line in lines:
...     stripped.append(line.rstrip('\n'))
...
>>> stripped
['Hello one!', 'Hello two!', 'Hello three!']
>>>
```

It's also possible to read lines one by one. Files have [a readline
method](https://docs.python.org/3/library/io.html#io.TextIOBase.readline)
that reads the next line, and returns `''` if we're at the end of the file.

```python
>>> with open('hello.txt', 'r') as f:
...     first_line = f.readline()
...     second_line = f.readline()
...
>>> first_line
'Hello one!\n'
>>> second_line
'Hello two!\n'
```

There's only one confusing thing about reading files. If we try
to read the same file object twice we'll find out that it only gets read
once:

```python
>>> first = []
>>> second = []
>>> with open('hello.txt', 'r') as f:
...     for line in f:
...         first.append(line)
...     for line in f:
...         second.append(line)
...
>>> first
['Hello one!\n', 'Hello two!\n', 'Hello three!\n']
>>> second
[]
>>>
```

File objects remember their position. When we tried to read the
file again it was already at the end, and there was nothing left
to read. But if we open the file again, we get a new file object that
is in the beginning and everything works.

```python
>>> first = []
>>> second = []
>>> with open('hello.txt', 'r') as f:
...     for line in f:
...         first.append(line)
...
>>> with open('hello.txt', 'r') as f:
...     for line in f:
...         second.append(line)
...
>>> first
['Hello one!\n', 'Hello two!\n', 'Hello three!\n']
>>> second
['Hello one!\n', 'Hello two!\n', 'Hello three!\n']
>>>
```

Usually it's best to just read the file once, and use the
content we have read from it multiple times.

If we need all of the content as a string, we can use [the read
method](https://docs.python.org/3/library/io.html#io.TextIOBase.read).

```python
>>> with open('hello.txt', 'r') as f:
...     full_content = f.read()
...
>>> full_content
'Hello one!\nHello two!\nHello three!\n'
>>>
```

We can also open full paths, like `open('C:\\Users\\me\\myfile.txt', 'r')`.
The reason why we need to use `\\` when we really mean `\` is that
backslash has a special meaning. There are special characters like
`\n`, and `\\` means an actual backslash.

[comment]: # "GitHub's syntax highlighting screws up with backslashes."

```
>>> print('C:\some\name')
C:\some
ame
>>> print('C:\\some\\name')
C:\some\name
>>>
```

Another way to create paths is to tell Python to escape them by
adding an `r` to the beginning of the string. In this case the `r`
is short for "raw", not "read".

```python
>>> r'C:\some\name'
'C:\\some\\name'
>>>
```

If our program is not meant to be ran on Windows and the paths
don't contain backslashes we don't need to double anything or use
`r` in front of paths.

```python
>>> print('/some/name')
/some/name
>>>
```

Doing things like `open('C:\\Users\\me\\myfile.txt', 'r')` is not
recommended because the code needs to be modified if someone wants to
run the program on a different computer that doesn't have a
`C:\Users\me` folder. Just use `open('myfile.txt', 'r')`.

## Examples

This program prints the contents of files:

```python
while True:
    filename = input("Filename or path, or nothing at all to exit: ")
    if filename == '':
        break

    with open(filename, 'r') as f:
        # We could read the whole file at once, but this is
        # faster if the file is very large.
        for line in f:
            print(line.rstrip('\n'))
```

This program stores the user's username and password in a file.
Plain text files are definitely not a good way to store usernames
and passwords, but this is just an example.

```python
# Ask repeatedly until the user answers 'y' or 'n'.
while True:
    answer = input("Have you been here before? (y/n) ")
    if answer == 'Y' or answer == 'y':
        been_here_before = True
        break
    elif answer == 'N' or answer == 'n':
        been_here_before = False
        break
    else:
        print("Enter 'y' or 'n'.")

if been_here_before:
    # Read username and password from a file.
    with open('userinfo.txt', 'r') as f:
        username = f.readline().rstrip('\n')
        password = f.readline().rstrip('\n')

    if input("Username: ") != username:
        print("Wrong username!")
    elif input("Password: ") != password:
        print("Wrong password!")
    else:
        print("Correct username and password, welcome!")

else:
    # Write username and password to a file.
    username = input("Username: ")
    password = input("Password: ")
    with open('userinfo.txt', 'w') as f:
        print(username, file=f)
        print(password, file=f)

    print("Done! Now run this program again and select 'y'.")
```

***

If you have trouble with this tutorial please [tell me about
it](../contact-me.md) and I'll make this tutorial better. If you
like this tutorial, please [give it a
star](../README.md#how-can-i-thank-you-for-writing-and-sharing-this-tutorial).

You may use this tutorial freely at your own risk. See
[LICENSE](../LICENSE).

[Previous](what-is-true.md) | [Next](modules.md) |
[List of contents](../README.md#basics)
