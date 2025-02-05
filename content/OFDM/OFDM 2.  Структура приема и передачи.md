---
title: OFDM 2.  Структура приема и передачи
draft: false
tags:
  - DSP
  - OFDM
date: 2025-01-07
permalink: ofdm-2
---
 Покомпонентый разбор приемника и передатчика для OFDM.

Рассмотрим два подхода: чисто аналоговый и цифровой с использованием дискертных преобразований Фурье.

![[ofdm_rx_tx.png]]



>[!note] *Пример* 
>Пусть $T$ - количество отсчетов необходимых для одного символа т.е его длительность и пусть у нас есть $10$ символов, которые мы хотим передать. Тогда длительность всего сообщения будет $10T$. 
>Разделим исходное сообщение на $10$ новых, каждое длительностью $10T$.  Считаем, что вся доступная полоса частот $W = 1$. Тогда $10$ несущих частот для новых сообщений имеют вид $f_k = k\frac{1}{10T}$, где $k = 0, 1, \dots 9$. 

## S/P conversion 

S/P - serial to parallel. Последовательно параллельный преобразователь.

В примере, деление исходного сообщения на $10$ новых - есть последовательно параллельное преобразование(**S/P conversion**). Мы реобразовали один быстрый поток символов, в много медленных параллельных потоков.

## Модуляция

У нас есть $10$ сообщений и есть $10$ несущих волн $e^{\frac{i k 2 \pi}{T}t}$.

Следуя верхней схеме сообщение для передачи будет иметь такой вид:

$$
s(t) = \sum_{n=0}^9 c_{n} (t) \ e^{\frac{i n 2 \pi}{T}t} \tag{1}
$$ 

^77014e

$c_n(t)$ -  символ $n$-го сообщения в момент времени $t$.

В этой схеме есть существенный недостаток - для практической реалиазци необходимо  $10$ генераторов опорных колебаний.

### Дискретное преобразование Фурье

Посмотрим на альтернатинвую реализацию. Можем считат, что наше отсчеты нашего сообщения взяты только в моменты времени $t_k =\frac{kT}{10}$.

Тогда [[#^77014e |(1)]] примет вид:

$$
s(t_k) = \sum_{n=0}^{9} c_n \  e^{\frac{i n 2 \pi k}{T}} \tag{2}
$$

^da55ea

Формула [[#^da55ea|(2)]] - называется **Обратное дискретное преобразование Фурье**.
Если взять экспоненту со знаком "минус" получается **Прямое дискретное преобразование Фурье**.

Так как дискретное преобразование Фурье переводит вектор из $n$ элементов в новый вектор их $n$ элементов, но мы должны передавать сообщение в канал последовательно - символ за символом, поэтому в нижней схеме после преобразования Фурье находится блок **P/S conversion** обратный для S/P.

## Прием

Все блоки аналогичны блокам передачи, за исключением преобразование Фурье. В случае приема используется **прямое преобразование Фурье**.