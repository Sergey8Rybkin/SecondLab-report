# Основы работы с Unity
Отчет по лабораторной работе #2 выполнил:
- Рыбкин Сергей Денисович
- РИ-300012

Ссылка на репозиторий с проектом
https://github.com/Sergey8Rybkin/Dragon-Picker

Отметка о выполнении заданий:

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.


Лабораторная работа № 2 Интеграция сервиса для получения данных
профиля пользователя.

Цель работы: создание интерактивного приложения и изучение принципов
интеграции в него игровых сервисов.

Задание 1.
По теме видео практических работ 1-5 повторить реализацию игры на Unity. Привести описание выполненных действий.

Для начала создаём репозиторий нашей игры. Нас интересует Core 3D, называем наш проект Dragon Picker.

Далее же начальным этапом является ознакомление с "Asset Store" и выгрузка от туда первого asset пака.
      -Скачиваем и импортируем
![Снимок экрана (8)](https://user-images.githubusercontent.com/100475554/195134791-88cb51df-468c-4831-b0bc-8287a4631f07.png)

Нас интересует один из префабов драконов. Мой выбор пал на DragonUsurper
Делаем копию и добавляем папку нашей сцены. Так-же из ассет пака нас интересует достать и настроить анимацию дракона. Дублируем Idle анимацию в наш проект. Чтобы наш дракон "ожил" создаём Animator controller, добавляем в него Idle анимацию, и перетаскиванием добавляем AC нашему дракону. Вынеся на сцену можем наблюдать нашего "ожившего" enemy.

![Sh4VVLXHqb](https://user-images.githubusercontent.com/100475554/195137797-f06de2b8-1a7d-4233-ac43-72ad2b551ace.gif)

Создаём яйцо
-Создаём на сцене сферу
-Даём ей значения 1, 1.5, 1
-Создаём материал чтобы покрасить наше яйцо (пипеткой можно в одну из частей дракона)
-Наносим материал

![image](https://user-images.githubusercontent.com/100475554/195140151-24a8c8c3-31ec-4e19-b92c-24a4b9f9f952.png)

Ещё нам нужно чем-то ловить наши яйца. 
Создаём сферу, увеличиваем, размещаем чуть ниже дракона.
Так-же нам для сферы нужен материал
Получаем картинку ниже

![eA02unz84m](https://user-images.githubusercontent.com/100475554/195142330-913da81a-b9ca-41e5-bc8f-eb6d9a5bafb3.gif)

В следующей части задания, нам необходимо выставить удобные значения камеры, и написать скрипт перемещения для нашего дракона.      

Начнём с камеры. Нам нужно сделать так чтобы камера была максимально удобна и информативна для нашего игрока
Задаём ей значения 
![image](https://user-images.githubusercontent.com/100475554/195143191-1685eaf4-68e7-4f78-9d6d-0119e02ae816.png)

И получаем такую игровую сцену. Также настраиваем Aspect ratio 16.9 тк это является стандартным разрешением

![LaRKb07HDf](https://user-images.githubusercontent.com/100475554/195143675-1ce91cc6-910d-4537-b8bf-dd0e3bcd4515.gif)

Переходим к программированию скрипта нашего дракона
На сцене наш дракон должен
1. Перелетать по сцене
2. При этом он не должен улетать за края экрана
3. При этом должен это делать "случайным образом

Создаём C# script.
Для начала инициализируем переменные которые мы сможем изменять при отладке: скорость, время между падением яиц, дистанция на которую дракон перемещается влево и вправо, и шанс изменения направления
Каждый кадр мы хотим чтобы наш дракон перемещался по сцене, и не хотим чтобы он улетал дальше заданной дистанции. Прописываем это в нашем скрипте.
Для случайности перемещения, мы делаем проверку. Каждый кадр передаём случайное значение. Если оно будет меньше нашей заданной величины, обращаем движение дракона.

В результате получаем вот такой скрипт (не законченная версия)
```c#
using UnityEngine;

public class EnemyDragon : MonoBehaviour
{

    public float speed = 1;

    public float timeBetweenEggDrops = 1f;

    public float leftRightDistance = 10f;

    public float changeDirection = 0.01f;
    // Start is called before the first frame update
    void Start()

    }
    // Update is called once per frame
    void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance){
            speed = Mathf.Abs(speed);
        }
        else if (pos.x > leftRightDistance) {
            speed = -Mathf.Abs(speed);
        }
    }


    private void FixedUpdate() {
        if (Random.value < changeDirection) {
            speed *= -1;
        }
    }
}
```
И вот такой результат на сцене

![oGwNFY4Vbr](https://user-images.githubusercontent.com/100475554/195147411-30810bae-46d4-4799-b2c4-46615eff0e9f.gif)

В следующей части задания мы разнообразим свою сцену, а так-же создадим скрипт сбрасывания яйца.

Сперва импортируем новый ассет пак который нам поможет с задачей разнообразия нашей игровой сцены

![Снимок экрана (10)](https://user-images.githubusercontent.com/100475554/195148099-a4025714-63e1-41e6-b52f-e94fc62927e5.png)

На данном этапе нужно создать скрипт сбрасывания яиц, и их уничтожение
Во-первых создадим Plane при пересечении с которым наше яйцо будет уничтожаться, а у игрока в будующем будет вычитаться жизнь. Можем сразу дать магматический материал, чтобы выглядело интереснее и размещаем его на сцене.

![image](https://user-images.githubusercontent.com/100475554/195149662-7f6efd3a-9f8c-4167-bc37-56af314a1025.png)

Далее займёмся нашими яйцами
Нам нужно
1. Чтобы яйца падали из дракона
2. При пересечении поверхности яйца должны пропадать создавая явный эффект
3. Так-же делаем ограничение на котором наши яйца должны уничтожаться в любом случае

Реализуем спавн яиц в нашем скрипте дракона, чтобы мы понимали откуда производить спавн. Так-же добавляем переменную в которую передаём префаб нашего яйца
```c#
using UnityEngine;

public class EnemyDragon : MonoBehaviour
{

    public GameObject DragonEggPrefab;

    public float speed = 1;

    public float timeBetweenEggDrops = 1f;

    public float leftRightDistance = 10f;

    public float changeDirection = 0.01f;
    // Start is called before the first frame update
    void Start()
    {
       Invoke ("DropEgg", 2f);
    }


    
    void DropEgg(){
        Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
        GameObject egg = Instantiate<GameObject>(DragonEggPrefab);
        egg.transform.position = transform.position + myVector;
        Invoke("DropEgg", timeBetweenEggDrops);
    }
    // Update is called once per frame
    
    void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance){
            speed = Mathf.Abs(speed);
        }
        else if (pos.x > leftRightDistance) {
            speed = -Mathf.Abs(speed);
        }
    }


    private void FixedUpdate() {
        if (Random.value < changeDirection) {
            speed *= -1;
        }
    }
}
```

Создаём новый скрипт конкретно для уничтожения яйца. В нём задаём условия при которых яйцо должно уничтожаться. Так-же создаём реакцию в виде Particle System. Она будет наглядно показывать игроку что произошло

Скрипт выглядит так
```c#
using UnityEngine;

public class DragonEgg : MonoBehaviour
{

    public static float bottomY = -37f;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    private void OnTriggerEnter(Collider other) {
        ParticleSystem ps = GetComponent<ParticleSystem>();
        var em = ps.emission;
        em.enabled = true;

        Renderer rend;
        rend = GetComponent<Renderer>();
        rend.enabled = false;
    }
    // Update is called once per frame
    void Update()
    {
        if (transform.position.y < bottomY) {
            Destroy(this.gameObject);
        }
    }
}
```

Так-же добавляем новый материалл нашему яйцу и настраиваем элемент Particle System

![image](https://user-images.githubusercontent.com/100475554/195152378-a5aa77e6-0a5c-4c04-b873-a7a88891d427.png)

Конечный вид на сцене это будет иметь вот такой

![unknown_2022 10 11-21 52_1](https://user-images.githubusercontent.com/100475554/195153763-c9e29694-f28a-4d59-8e54-f5dbb1cabbe7.gif)

Оставшееся задание осталось создать скрипт в котором мы будем выполнять движение нашей "кареткой"
Создаём скрипт с созвучным названием нашей игры
В скрипте прописываем 
-спавн каретки
-её расположение на сцене
-её размер

Инициализируем переменные. Переменную для префаба каретки. Переменную количества щитов (в будующем можно будет увеличивать размер каретки). Переменную расположения. Радиус щита

Прописываем цикл, с помощью которого на нашей сцене будут создаваться щиты, в будующем можно будет так-же бонусами изменять резмер каретки игрока.

```c#
using UnityEngine;

public class DragonPicker : MonoBehaviour
{
    public GameObject energyShieldPrefab;

    public int numEnergyShield = 3;

    public float energyShieldBottomY = -6f;

    public float energyShieldRadius = 1.5f;
    // Start is called before the first frame update
    void Start()
    {
        for (int i = 1; i <= numEnergyShield; i++){
            GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
            tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
            tShieldGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
        }
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```

Последним заданием было ознакомиться с документацией Яндекс.Игр и заполнить заявку добавление сервиса. Заполнив где это было возможно, я получил 73%. Думаю в будующем эта шкала обязательно дойдёт до 100%

![image](https://user-images.githubusercontent.com/100475554/195156076-e343c7e4-99c0-4f48-b4f1-2814203dbff1.png)


Вывод

За данную лабораторную получилось ознакомиться с множеством инструментов и возможностей Unity.
1. Мы ознакомились с ассет стором юнити и добавили два ассет пака в свой проект.
2. Мы научились так-же добавлять необходимое из добавленных ассетов себе на сцену.
3. Научились взаимодействию с анимациями.
4. Научились с помощью скрипта перемещать объекты, спавнить, уничтожать, добавлять Particle System.
5. Ознакомились с функционалом Particle System.

Ссылка на проект https://github.com/Sergey8Rybkin/Dragon-Picker
