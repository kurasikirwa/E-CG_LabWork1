# Лабораторная работа №1  

Описание функций библиотеки **GL** и **собственных функций** задействованных в лабораторной работе №1
1.  **[glut функции](#glut-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8)**  
    * [glutInit](#glutinit)
    * [glutInitDisplayMode](#glutinitdisplaymode)
    * [glutInitWindowPosition, glutInitWindowSize](#glutinitwindowposition-glutinitwindowsize)
    * [glutCreateWindow](#glutcreatewindow)
    * [glutDisplayFunc](#glutdisplayfunc)
    * [glutMainLoop](#glutmainloop)
    * [glutSwapBuffers](#glutswapbuffers)
2.  **[gl функции](#gl-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8)**
    * [glClearColor](#glclearcolor)
    * [glClear](#glclear)
3.  **[Собственные функции](#%D1%81%D0%BE%D0%B1%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D0%B5-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8)**
    * [RenderSceneCB](#renderscenecb)
4.  **[Список литературы](#%D1%81%D0%BF%D0%B8%D1%81%D0%BE%D0%BA-%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D1%82%D1%83%D1%80%D1%8B)**

***


# glut функции

## glutInit
**`glutInit`** инициализирует библиотеку **GLUT** и согласует сеанс с оконной системой.

### С спецификация
```c
void glutInit(int *argcp, char **argv);
```

### Параметры
#### *argcp*
>Указатель на немодифицированную переменную *argc* программы из **main**. По возвращении значение, на которое указывает *argcp*, будет обновлено, отому что **`glutInit`** извлекает все параметры командной строки, предназначенные для библиотеки **GLUT**.
#### *argv*
>Немодифицированная переменная *argv* программы из файла main.	Как и в случае с *argcp*, данные для argv будут обновляться, потому что **`glutInit`** извлекает любые параметры командной строки, понятные библиотеке **GLUT**.  

### Описание
**`glutInit`** инициализирует библиотеку **GLUT** и согласует сеанс с оконной системой. Во время этого процесса **`glutInit`** может вызвать завершение программы **GLUT** с сообщением об ошибке для пользователя, если **GLUT** не может быть правильно инициализирован. Примеры этой ситуации включают сбой подключения к оконной системе, отсутствие поддержки оконной системой OpenGL и недопустимые параметры командной строки.  

**`glutInit`** также обрабатывает параметры командной строки, но анализ конкретных параметров зависит от оконной системы.

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node10.html)
***

## glutInitDisplayMode
**`glutInitDisplayMode`** устанавливает начальный режим отображения.  

### C Спецификация
```c
void glutInitDisplayMode(unsigned int mode);
```

### Параметры
#### *mode*
>Режим отображения, который может быть представлен одной из следующих констант или комбинацией этих констант с помощью побитового ИЛИ.

|Константа	      |Значение|
|-----------------|--------|
|GLUT_RGB	        |Для отображения графической информации используются 3 компоненты цвета RGB.|
|GLUT_RGBA	      |То же что и RGB, но используется также 4 компонента ALPHA (прозрачность).|
|GLUT_INDEX	      |Цвет задается не с помощью RGB компонентов, а с помощью палитры. Используется для старых дисплеев, где количество цветов например 256. |
|GLUT_SINGLE	    |Вывод в окно осуществляется с использованием 1 буфера. Обычно используется для статического вывода информации.|
|GLUT_DOUBLE	    |Вывод в окно осуществляется с использованием 2 буферов. Применяется для анимации, чтобы исключить эффект мерцания.|
|GLUT_ACCUM	      |Использовать также буфер накопления (Accumulation Buffer). Этот буфер применяется для создания специальных эффектов, например отражения и тени.|
|GLUT_ALPHA	      |Использовать буфер ALPHA. Этот буфер, как уже говорилось используется для задания 4-го компонента цвета - ALPHA. Обычно применяется для таких эффектов как прозрачность объектов и антиалиасинг.|
|GLUT_DEPTH	      |Создать буфер глубины. Этот буфер используется для отсечения невидимых линий в 3D пространстве при выводе на плоский экран монитора.|
|GLUT_STENCIL	    |Буфер трафарета используется для таких эффектов как вырезание части фигуры, делая этот кусок прозрачным. Например, наложив прямоугольный трафарет на стену дома, вы получите окно, через которое можно увидеть что находится внутри дома.|
|GLUT_MULTISAMPLE | Битовая маска для выбора окна с поддержкой мультисэмплинга. Если мультисэмплинг недоступен, автоматически будет выбрано окно без мультисэмплинга. Примечание: как клиентская, так и серверная реализации OpenGL должны поддерживать расширение *GLX_SAMPLE_SGIS* , чтобы мультисэмплинг был доступен.|
|GLUT_STEREO	    |Этот флаг используется для создания стереоизображений. Используется редко, так как для просмотра такого изображения нужна специальная аппаратура.|
|GLUT_LUMINANCE   | Битовая маска для выбора окна с цветовой моделью "яркости". Эта модель обеспечивает функциональность цветовой модели OpenGL RGBA, но зеленый и синий компоненты не сохраняются в буфере кадров. Вместо этого красный компонент каждого пикселя преобразуется в индекс между нулем и glutGet(GLUT_WINDOW_COLORMAP_SIZE)-1 и просматривается в цветовой карте для каждого окна, чтобы определить цвет пикселей в окне. Начальная цветовая карта окон GLUT_LUMINANCE инициализируется как линейная шкала серого, но может быть изменена с помощью процедур цветовой карты **GLUT**.|

#### Описание
Начальный режим отображения используется при создании окон верхнего	уровня, дочерних окон и наложений для определения режима отображения OpenGL для создаваемого окна или наложения.  

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node12.html)
***

## glutInitWindowPosition, glutInitWindowSize
**`glutInitWindowPosition`**, **`glutInitWindowSize`** устанавливаюет начальное положение и размер окна соответственно.

### С спецификация
```c
void glutInitWindowSize(int width, int height);
void glutInitWindowPosition(int x, int y);
```

### Параметры
### *width*
>Ширина в пикселях.
#### *height*
>Высота в пикселях.
#### *x*
>Расположение окна в X пикселе.
#### *y*
>Расположение окна в Y пикселе.

### Описание
Окна, созданные с помощью **[glutCreateWindow](#glutcreatewindow)** , будут запрошены для создания с текущими начальными положением и размером окна.  

Начальное значение состояния **GLUT** начальной позиции окна равно -1 и -1. Если компонент X или Y для начального положения окна отрицателен, фактическое положение окна определяется оконной системой. Начальное значение состояния **GLUT** начального размера окна равно 300 на 300. Компоненты начального размера окна должны быть больше нуля.  

Назначение начальных значений положения и размера окна состоит в том, чтобы предоставить оконной системе предложение относительно начального размера и положения окна. Оконная система не обязана использовать эту информацию. Следовательно, программы **GLUT** не должны предполагать, что окно было создано с указанным размером или положением. Программа **GLUT** должна использовать обратный вызов изменения формы окна, чтобы определить истинный размер окна.  

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node11.html)
***

## glutCreateWindow
**`glutCreateWindow`** создает окно верхнего уровня.

### С спецификация
```c
int glutCreateWindow(char *name);
```

### Параметры
#### *name*
>Строка символов ASCII для использования в качестве имени окна.
>
### Описание
**`glutCreateWindow`** создает окно верхнего уровня. Имя будет предоставлено оконной системе как имя окна. Цель состоит в том, чтобы оконная система пометила окно именем.  

Неявно, текущее окно устанавливается на вновь созданное окно.  

Каждое созданное окно имеет уникальный связанный с ним контекст OpenGL. Изменения состояния связанного с окном контекста OpenGL можно выполнить сразу после создания окна.  

Состояние отображения окна изначально предназначено для отображения окна. Но на состояние отображения окна фактически не действуют до тех пор, пока не будет введен **[glutMainLoop](#glutmainloop)** . Это означает, что до тех пор, пока не будет вызван **[glutMainLoop](#glutmainloop)** , рендеринг в созданное окно неэффективен, поскольку окно еще не может быть отображено.  

Возвращаемое значение представляет собой уникальный небольшой целочисленный идентификатор окна. Диапазон выделенных идентификаторов начинается с единицы. Этот идентификатор окна можно использовать при вызове **glutSetWindow** .

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node16.html)
***

## glutDisplayFunc
**`glutDisplayFunc`** устанавливает обратный вызов дисплея для текущего окна.

### С спецификация
```c
void glutDisplayFunc(void (*func)(void));
```
### Параметры
#### *func*
>Новая функция обратного вызова дисплея.

### Описание
**`glutDisplayFunc`** устанавливает обратный вызов дисплея для текущего окна . Когда **GLUT** определяет, что необходимо повторно отобразить нормальную плоскость окна, вызывается обратный вызов display для окна. Перед обратным вызовом текущее окно устанавливается в окно, которое необходимо повторно отобразить, и (если обратный вызов отображения наложения не зарегистрирован) используемый слой устанавливается в нормальную плоскость. Обратный вызов дисплея вызывается без параметров. Вся нормальная область плоскости должна быть повторно отображена в ответ на обратный вызов (включая вспомогательные буферы, если ваша программа зависит от их состояния).  

**GLUT** определяет, когда должен запускаться обратный вызов дисплея, на основе состояния повторного отображения окна. Состояние повторного отображения окна может быть задано либо явно путем вызова **glutPostRedisplay**, либо неявно в результате повреждения окна, о котором сообщает оконная система. Несколько опубликованных повторных отображений для окна объединяются **GLUT**, чтобы свести к минимуму количество вызванных обратных вызовов дисплея.  

Когда наложение установлено для окна, но не зарегистрирован обратный вызов отображения наложения, обратный вызов дисплея используется для повторного отображения как наложения, так и нормальной плоскости (то есть он будет вызываться, если установлено либо состояние повторного отображения, либо состояние повторного отображения наложения). В этом случае используемый слой не изменяется неявно при входе в обратный вызов дисплея.  

См. **GluOverlayDisplayFunc**, чтобы понять, как могут быть установлены различные обратные вызовы для наложения и нормальной плоскости окна.  

Когда окно создается, для него не существует обратного вызова display. Программист несет ответственность за установку обратного вызова отображения для окна до того, как окно будет показано. Обратный вызов дисплея должен быть зарегистрирован для любого отображаемого окна. Если окно отображается без регистрации обратного вызова отображения, возникает фатальная ошибка. Передача *NULL* в **`glutDisplayFunc`** незаконна с **GLUT 3.0**; нет способа "отменить регистрацию" обратного вызова дисплея (хотя всегда можно зарегистрировать другую подпрограмму обратного вызова).  

По возвращении из обратного вызова дисплея нормальное поврежденное состояние окна (возвращаемое вызовом **glutLayerGet(*GLUT_NORMAL_DAMAGED*)** очищается. Если обратный вызов дисплея не зарегистрирован, поврежденное состояние окна окна (возвращаемое вызовом **glutLayerGet(*GLUT_OVERLAY_DAMAGED*)**) также очищено.  

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node46.html)
***

## glutMainLoop
**`glutMainLoop`** входит в цикл обработки событий GLUT.

### С спецификация
```c
void glutMainLoop(void);
```

### Описание
**`glutMainLoop`** входит в цикл обработки событий **GLUT**. Эта подпрограмма должна вызываться не более одного раза в программе **GLUT**. После вызова эта подпрограмма никогда не вернется. При необходимости он будет вызывать любые зарегистрированные обратные вызовы.

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node14.html)
***

## glutSwapBuffers
**`glutSwapBuffers`** меняет местами буферы текущего окна при двойной буферизации.

### С спецификация
```c
void glutSwapBuffers(void);
```

### Описание
Выполняет замену буфера на слое, используемом для текущего окна . В частности, **`glutSwapBuffers`** переводит содержимое заднего буфера слоя, используемого текущим окном , в содержимое переднего буфера. Содержимое заднего буфера становится неопределенным. Обновление обычно происходит во время вертикального возврата монитора, а не сразу после вызова **`glutSwapBuffers`**.  

Неявный **glFlush** выполняется **`glutSwapBuffers`** перед возвратом. Последующие команды OpenGL могут выполняться сразу после вызова **`glutSwapBuffers`** , но не выполняются до завершения обмена буферами.    

Если используемый слой не имеет двойной буферизации, **`glutSwapBuffers`** не действует.  

[Источник](https://www.opengl.org/resources/libraries/glut/spec3/node21.html)
***

# gl функции

## glClearColor
**`glClearColor`** — укажите четкие значения для цветовых буферов.

### С спецификация
```c
void glClearColor(GLclampf red,
                  GLclampf green,
                  GLclampf blue,
                  GLclampf alpha);

```

### Параметры
### *red, green, blue, alpha*
>Указывает значения красного, зеленого, синего и альфа-фактора, используемые при очистке цветовых буферов. Начальные значения равны 0.

### Описание
**`glClearColor`** задает значения красного, зеленого, синего и альфа-канала, используемые для очистки цветовых буферов. Значения, заданные параметром, зажаты в диапазоне **`glClearColor`** **[0,1]**.

[Источник](https://www.khronos.org/registry/OpenGL-Refpages/es2.0/xhtml/glClearColor.xml)
***

## glClear
**`glClear`** — очистка буферов для заданных значений

### С спецификация
```c
void glClear(	GLbitfield mask);
```

### Параметры
#### *mask*
>Побитовое ИЛИ масок, указывающих на очищаемые буферы. Три маски: *GL_COLOR_BUFFER_BIT*, *GL_DEPTH_BUFFER_BIT*, и *GL_STENCIL_BUFFER_BIT*.
>
### Описание
**`glClear`** задает для области битовой плоскости окна значения, ранее выбранные параметрами **glClearColor**, **glClearDepth**, и **glClearStencil**. Несколько цветовых буферов можно очистить одновременно, выбрав более одного буфера одновременно с помощью **glDrawBuffer**.  

Тест владения пикселем, тест ножниц, дизеринг и буферные маски записи влияют на работу **`glClear`**. Ножничная коробка ограничивает очищенную область. Альфа-функция, функция смешивания, логическая операция, набор элементов, сопоставление текстур и буферизация глубины игнорируются **`glClear`**.  

**`glClear`** принимает один аргумент, который является побитовым ИЛИ нескольких значений, указывающих, какой буфер должен быть очищен.  

Принимает следующие значения:
|GL_COLOR_BUFFER_BIT   | Указывает буферы, включенные в данный момент для цветопередачи.|
|----------------------|----------------------------------------------------------------|
|GL_DEPTH_BUFFER_BIT   | Указывает буфер глубины.                                       |
|GL_STENCIL_BUFFER_BIT | Указывает буфер набора элементов.                              |

Значение, до которого очищается каждый буфер, зависит от значения очистки для этого буфера.

[Источник](https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glClear.xhtml)
***

# Собственные функции
## RenderSceneCB
RenderSceneCB - рендер сцены.
### С спецификация
```c
static void RenderSceneCB()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glutSwapBuffers();
}
```
### Описание
Вызывается **[glClear](#glclear)** с параметром **`(GL_COLOR_BUFFER_BIT)`**, который очищает буфер для *GL_COLOR_BUFFER_BIT*. 

Вызывается **[glutSwapBuffers](#glutswapbuffers)**, который меняет местами буферы текущего окна при двойной буферизации.  

***

# Список литературы
**[OpenGL](https://www.opengl.org/resources/libraries/glut/spec3/spec3.html)**  
**[Khronos](https://www.khronos.org/registry/OpenGL-Refpages/gl4/)**  
