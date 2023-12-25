# da-in-gamedev
# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Трошко Егор Юрьевич
- РИ220936
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- в проекте Unity реализовать перцептрон, который умеет производить вычисления OR, AND, NAND, XOR и дать комментарии о корректности работы
- Задание 2.
- Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.
- Задание 3.
- Построить визуальную модель работы перцептрона на сцене Unity.
- Выводы.

## Цель работы
Разработать и обучить модель перцептрона.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления OR, AND, NAND, XOR и дать комментарии о корректности работы

В проекте я создала пустой GameObject с именем Perceptron и прикрепила к нему ниже написанный скрипт Perceptron:

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
    public double[] input;
    public double output;
}

public class Perceptron : MonoBehaviour
{

    public TrainingSet[] ts;
    double[] weights = { 0, 0 };
    double bias = 0;
    double totalError = 0;

    double DotProductBias(double[] v1, double[] v2)
    {
        if (v1 == null || v2 == null)
            return -1;

        if (v1.Length != v2.Length)
            return -1;

        double d = 0;
        for (int x = 0; x < v1.Length; x++)
        {
            d += v1[x] * v2[x];
        }

        d += bias;

        return d;
    }

    double CalcOutput(int i)
    {
        double dp = DotProductBias(weights, ts[i].input);
        if (dp > 0) return (1);
        return (0);
    }

    void InitialiseWeights()
    {
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = Random.Range(-1.0f, 1.0f);
        }
        bias = Random.Range(-1.0f, 1.0f);
    }

    void UpdateWeights(int j)
    {
        double error = ts[j].output - CalcOutput(j);
        totalError += Mathf.Abs((float)error);
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = weights[i] + error * ts[j].input[i];
        }
        bias += error;
    }

    double CalcOutput(double i1, double i2)
    {
        double[] inp = new double[] { i1, i2 };
        double dp = DotProductBias(weights, inp);
        if (dp > 0) return (1);
        return (0);
    }

    void Train(int epochs)
    {
        InitialiseWeights();

        for (int e = 0; e < epochs; e++)
        {
            totalError = 0;
            for (int t = 0; t < ts.Length; t++)
            {
                UpdateWeights(t);
                Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
            }
            Debug.Log("TOTAL ERROR: " + totalError);
        }
    }

    void Start()
    {
        Train(8);
        Debug.Log("Test 0 0: " + CalcOutput(0, 0));
        Debug.Log("Test 0 1: " + CalcOutput(0, 1));
        Debug.Log("Test 1 0: " + CalcOutput(1, 0));
        Debug.Log("Test 1 1: " + CalcOutput(1, 1));
    }

    void Update()
    {

    }
}
```

### 1) OR
Таблица истинности:
| A | B | A OR B |
| - | - | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Условия для работы перцептрона: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/9cc58074-b0e1-4a74-a9cd-b9e335d9334f)

Зададим одну обучающую эпоху и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/7a7cc44c-2fb6-4b29-8f2e-ceb8bf69a179)

Из скрина выше видим, что значение Total Error не равно 0, следовательно, одной эпохи недостаточно.

Зададим 5 эпох и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/999b7403-74e0-4fdf-b17f-87978f37b323)
Из скрина выше видим, что для обучения модели необходимо 5 эпох.

### 2) AND
Таблица истинности:
| A | B | A AND B |
| - | - | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Условия работы перцептрона: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/7d9b3721-9486-4952-b597-fafdd529b065)

Зададим одну обучающую эпоху и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/4e2e3a41-47e3-49c9-82fb-c36bd7862b99)

Из скрина выше видим, что значение Total Error не равно 0, следовательно, одной эпохи недостаточно.

Зададим 6 эпох и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/5195344c-2d49-4a7b-b647-3d2a85563238)
Из скрина выше видим, что для обучения модели необходимо 6 эпох.

### 3) NAND
Таблица истинности:
| A | B | A NAND B |
| - | - | ------ |
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Условия работы перцептрона: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/270e8b84-fca3-4845-b4cc-8e4cb4513072)

Зададим одну обучающую эпоху и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/4dfc83e3-9b08-4c49-9270-78aca81cb471)

Из скрина выше видим, что значение Total Error не равно 0, следовательно, одной эпохи недостаточно.

Зададим 6 эпох и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/f3e165fa-0a88-4815-84a4-ca6ce091fe2f)
Из скрина выше видим, что для обучения модели необходимо 6 эпох.

### 3) XOR
Таблица истинности:
| A | B | A XOR B |
| - | - | ------ |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Условия работы перцептрона: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/2dcc0df4-5ee5-4153-8e7e-fbe0fe06b46b)

Зададим одну обучающую эпоху и получим такой результат: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/da377c27-4d82-46f8-a762-afac8d7e2a72)

Из скрина выше видим, что значение Total Error не равно 0, следовательно, одной эпохи недостаточно.
Задавая больше эпох (вплоть до 100), значение переменной Total Error всегда равно 4, следовательно, данную модель перцептрона невозможно обучить операции XOR.

## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.

В данном задании я делала 4 попытки в 8 эпохах обучения для каждой операции.

![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/82604e45-a827-4c08-a3fe-58eef0ae3c1f)
![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/0c620798-87e1-46eb-ac4b-1ed25d3fdc5d)
![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/f5e89cfc-95a5-49f8-87ec-07283a472f1d)
![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/e7b1139d-e0d3-4889-a9ca-e986ecd64646)

Количество эпох обучения зависит от решаемой операции. Однако выше показанная статистика не совсем точна, так как взято маленькое количество попыток.

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

Я создал 4 сцены на Unity с названиями операций, которые будут в ней визуализированны: ![image](https://github.com/Eiasav/da-in-gamedev/assets/130223999/bca8a673-8b8c-4a4d-8ea6-d13ba2e59d78)
В каждой сцене по 4 пары кубов (белый - истина, черный - ложь): 
К каждому из верхних кубов был назначен Rigidbody, а к нижним Is Trigger.

### 1) OR
Начальное положение: 

Результат: 


## Выводы

Я познакомился с визуализацией данных из GoogleSheets в Unity и работой со звуковыми эффектами в Unity с помощью скриптов на Python и C#.
