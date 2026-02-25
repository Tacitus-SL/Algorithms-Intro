# Algorithms-Intro

## Оглавление:
- [1. Time complexity и примеры](#1-что-такое-time-complexity)
  - [Циклы for](#11-цикл-for)
  - [Циклы while](#12-цикл-while)
  - [Комбинация разных циклов](#13-комбинация-разных-циклов)
  - [Оператор if](#14-оператор-if)
  - [Рекурсия](#15-рекурсия)
  - [Мини-таблица](#16-мини-таблица)
  - [Примеры](#17-примеры)
- [2. Master's Theorem](#2-masters-theorem)
- [3. Сортировки](#3-сортировки)
  - [Insertion Sort](#31-insertion-sort)
  - [Bubble Sort](#32-bubble-sort)
  - [Selection Sort](#33-selection-sort)
  - [Merge Sort](#34-merge-sort-divide-and-conquer)
  - [Heap Sort](#35-heap-sort)
  - [Quick Sort](#36-quick-sort)
- [4. Data Structures](#4-data-structures)
  - [Ram-model](#41-ram-model)
  - [Array](#42-array)
  - [Heaps](#43-heaps-priority-queues)
- [5. Randomized Algorithms](#5-randomized-algorithms)
- [6. Hoare Algorithm](#6-hoare-algorithm)
- [7. Median of Medians](#7-median-of-medians)

## 1. Что такое time complexity
**Time complexity** — это оценка того, как растёт время работы алгоритма при увеличении входных данных. Нас интересует асимптотическое поведение при больших n.

Обычно time complexity записывают через Big O. Для определения асимптотики мы всегда берём худший случай и отбрасываем константы и незначащие слагаемые.

Общий алгоритм как считать time complexity:
1) Считаешь, сколько раз выполняется код
2) Выражаешь это через n
3) Берёшь самый быстрорастущий член
4) Записываешь Big O

### 1.1 Цикл `for`:
- **Обычный:**
```c
for (int i = 0; i < n; i++) {
    // O(1)
}
```
тело выполняется n раз ⮕ сложность: O(n)

- **Два вложенных цикла:**
```c
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // O(1)
    }
}
```
внешний: n, внутренний: n, всего: n * n ⮕ O(n²)

- **Внутренний зависит от внешнего:**
```c
for (int i = 0; i < n; i++) {
    for (int j = 0; j < i; j++) {
    }
}
```
Количество итераций:
`0 + 1 + 2 + ... + (n-1) = n(n-1)/2`. Асимптотически это n² ⮕ O(n²)

- **Три вложенных цикла:**
```c
for (int i = 0; i < n; i++)
  for (int j = 0; j < n; j++)
    for (int k = 0; k < n; k++)
```
n * n * n ⮕ O(n³)

- **Последовательные циклы:**
```c
for (int i = 0; i < n; i++) {
}

for (int j = 0; j < n; j++) {
}
```
n + n = 2n ⮕ O(n)

### 1.2 Цикл while
Асимптотика while ничем не отличается от for 
- **Обычный:**
```c
int i = 0;
while (i < n) {
    i++;
}
```
выполняется n раз ⮕ O(n)

- **while с делением:**
```c
while (n > 1) {
    n /= 2;
}
```
Сколько раз делим n на 2, пока не станет 1? log₂(n) ⮕ O(log n)

### 1.3 Комбинация разных циклов:
```c
for (int i = 0; i < n; i++) {      // O(n)
    while (j > 1) {               // O(log n)
        j /= 2;
    }
}
```
n * log n ⮕ O(n log n)

### 1.4 Оператор if
```c
if (x > 0) {
    // O(n)
} else {
    // O(1)
}
```
Берём худший случай ⮕ O(n)

### 1.5 Рекурсия
Линейная рекурсия
```c
void f(int n) {
    if (n == 0) return;
    f(n - 1);
}
```

вызывается n раз ⮕ O(n)

### 1.6 Мини-таблица:
| Код           | Сложность  |
| ------------- | ---------- |
| Один цикл     | O(n)       |
| Два вложенных | O(n²)      |
| Деление на 2  | O(log n)   |
| Цикл + лог    | O(n log n) |
| Константа     | O(1)       |


### 1.7 Примеры:
1) while внутри for внутри if
```c
   for (int i = 0; i < n; i++) {      // O(n)
    if (i == 0) {
        while (j > 1) {            // O(log n)
            j /= 2;
        }
    } else {
        for (int k = 0; k < n; k++) { // O(n)
        }
    }
}
```
Берем худший случай: ветка else → O(n) и внешний for → O(n) ⮕ O(n²)

2) if внутри while внутри for
```c
for (int i = 0; i < n; i++) {      // O(n)
    int j = n;
    while (j > 1) {                // O(log n)
        if (j % 2 == 0) {
            // O(1)
        }
        j /= 2;
    }
}
```
O(n log n)

3) while внутри if, но с ловушкой
```c
for (int i = 0; i < n; i++) {
    if (i == 0) {                  // выполнится 1 раз!
        while (j > 1) {            // O(log n)
            j /= 2;
        }
    }
}
```
while выполняется 1 раз, остальное — константа ⮕ O(n)

4) Все в перемешку
```c
for (int i = 0; i < n; i++) {
    int j = i;
    while (j > 0) {
        if (j % 2 == 0) {
            j /= 2;
        } else {
            j--;
        }
    }
}
```
O(n²)

---

### 2. Master's Theorem
Иногда рекурсию можно представить как $T(n) = aT(n/b)+O(nᵈ)$.

Тогда, сложность алгоритма оценивается в 3 случаях:

1) $O(n^d)$, если $d > log_ba$
2) $O(n^dlog_bn)$, если $d = log_ba$
3) $O(n^{log_ba})$, если $d < log_ba$

---

## 3. Сортировки
### 3.1 Insertion Sort
Идея:
- Считаем, что первые k элементов отсортированы
- Берём следующий элемент и вставляем в правильное место
- Хорошо для почти отсортированных массивов
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr
```
Time complexity: $O(n^2)$

### 3.2 Bubble Sort
Идея:
- Проходим по массиву, сравниваем соседние элементы
- Меняем их местами, если порядок неправильный
- Повторяем, пока массив не отсортирован
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr
```
Time complexity: $O(n^2)$

### 3.3 Selection Sort
Идея:
- Находим минимальный элемент
- Меняем его с первым, потом со вторым, и так далее
- Всегда делает O(n²) сравнений
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```
Time complexity: $O(n^2)$

### 3.4 Merge Sort (Divide and Conquer)
Идея:
- Разделяем массив на две половины
- Сортируем рекурсивно каждую половину
- Сливаем две отсортированные части
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr)//2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Merge
    merged = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            j += 1
    merged += left[i:]
    merged += right[j:]
    return merged
```
Time complexity: $O(n \log n)$

### 3.5 Heap Sort
Идея:
- Строим кучу: Проходим по массиву и для каждого элемента вызываем $insert(x)$
- Извлекаем корень: Забираем значение из $H[0]$ — это наш текущий минимум..
- Восстанавливаем структуру: На место $H[0]$ ставим самый последний элемент кучи $H[n-1]$ и вызываем $sift$ $down(0)$, чтобы этот элемент встал на свое место.
- Повторяем: Делаем так $n$ раз, пока не вытащим все элементы в порядке возрастания.
```python
def heap_sort(arr):
    heap = MinHeap()
    result = []

    # Фаза 1: построение кучи (n вставок)
    for x in arr:
        heap.insert(x)

    # Фаза 2: n извлечений минимума
    while heap.heap:
        result.append(heap.remove_min())

    return result
```
Time complexity: $O(n \log n)$

### 3.6 Quick Sort
Идея:
- Выбираем случайный опорный элемент $x$
- Разбиваем массив на три части: меньше $x$, равные $x$ и больше $x$
- Рекурсивно сортируем части с меньшими и большими элементами
```python
import random

def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    x = random.choice(arr)
    
    less = [i for i in arr if i < x]
    equal = [i for i in arr if i == x]
    greater = [i for i in arr if i > x]
    
    return quick_sort(less) + equal + quick_sort(greater)
```
Time complexity: $O(n \log n)$

---

## 4. Data Structures
### 4.1 RAM model
RAM-модель, что расшифровывается как Random Access Machine, это математическая модель компьютера, в которой все базовые операции выполняются за константное время и используется память со случайным доступом (т. е. к любой ячейке можно обратиться за одинаковое время), чтобы удобно анализировать алгоритмы. 

### 4.2 Array
Array (он же массив) это базовая структура данных, представляющая собой упорядоченный набор элементов одного типа, расположенных последовательно в памяти. Каждый элемент имеет индекс, размер задаётся при создании и не меняется, все элементы имеют один тип.

Чтение (return $a[i]$) и запись ($a[i]=x$) происходят за $O(n)$, поиск по значению и вставка/удаление же за $O(n)$

### 4.3 Heaps (Priority Queues)
Heap (она же куча) это структура данных, которая хранит набор элементов $A$ и поддерживает две основные операции: $insert(x)$ (добавить элемент в набор) и $remove$ $min()$ (найти, извлечь и вернуть минимальный элемент).

**Binary heap**, все операции работают за $O(\log n)$ время.
Структура такая же как и у бинарного дерева. Значение в любом узле меньше или равно значениям его детей. Значит, корень (верхний элемент) всегда минимум.

Кучу удобно хранить в обычном массиве, заполняя элементами сверху вниз, слева направо:

- Корень в $H[0]$
- Левый ребенок узла $i$: $2i + 1$
- Правый ребенок узла $i$: $2i + 2$
- Родитель узла $i$: $(i - 1) // 2$

Операции:
- Insert (Sift Up / Всплытие)
1. Новый элемент $x$ ставится в самый конец массива (в последний свободный лист).
2. Пока он меньше своего родителя, мы меняем их местами (swap).
3. Элемент «всплывает» вверх до нужной позиции.
Так как высота дерева $\approx \log n$, значит максимум $\log n$ обменов.

- Remove Min (Sift Down / Погружение)
1. Минимум всегда корень, сохраняем, чтобы вернуть в конце как ответ. Но теперь там пустое место
2. Чтобы структура дерева не развалилась, мы берем самый последний элемент из массива и ставим его на место корня.
3. Теперь этот большой элемент должен спуститься на свое место: выбираем меньшего из двух детей, если наш элемент больше мина, свапаем их
4. Повторяем процесс, пока не найдет своего места

---

## 5. Randomized Algorithms
Входные данные подаются в алгоритм при помощи генератора случайных чисел. Исход может меняться от запуска к запуску.

Среднее время: $\overline{T}(n) = E(T(n))$ — это мат ожидание времени работы. 

Худшее среднее: $\hat{T}(n) = \max(E(T(input)))$ — это худший случай для мат ожидания.

---
## 6. Hoare Algorithm
Задача: найти элемент, который стоял бы на $k$-м месте, если бы массив был отсортирован (например, найти медиану)
Идея: После split мы смотрим, в какой части оказался индекс $k$. Если $k < m$ (индекс раздела), мы идем только в левую часть. Если больше — в правую.

```python
import random

def quick_select(arr, k):
    def select(left, right, k_idx):
        if left == right:
            return arr[left]
        
        pivot_idx = partition(left, right)
        
        if k_idx == pivot_idx:
            return arr[k_idx]
        elif k_idx < pivot_idx:
            return select(left, pivot_idx - 1, k_idx)
        else:
            return select(pivot_idx + 1, right, k_idx)

    def partition(left, right):
        rand_idx = random.randint(left, right)
        arr[rand_idx], arr[right] = arr[right], arr[rand_idx]
        x = arr[right]
        i = left
        for j in range(left, right):
            if arr[j] <= x:
                arr[i], arr[j] = arr[j], arr[i]
                i += 1
        arr[i], arr[right] = arr[right], arr[i]
        return i

    return select(0, len(arr) - 1, k)
```
Time complexity: $O(n \log n)$: Поскольку мы каждый раз отбрасываем примерно половину массива, время работы составляет $n + n/2 + n/4 \dots = O(n)$.

---

## 7. Median of Medians
Идея:
- Разбиваем массив на группы по 5 элементов.
- Находим медиану в каждой группе (сортировкой 5 элементов).
- Рекурсивно находим медиану среди полученных медиан — это наш опорный элемент $x$.
- Используем $x$ для разделения массива (partition) и продолжаем поиск в нужной части.
```python
def get_median(sub_arr):
    return sorted(sub_arr)[len(sub_arr) // 2]

def median_of_medians(arr, k):
    if len(arr) <= 5:
        return sorted(arr)[k]

    subgroups = [arr[i:i + 5] for i in range(0, len(arr), 5)]
    medians = [get_median(group) for group in subgroups]

    x = median_of_medians(medians, len(medians) // 2)

    less = [i for i in arr if i < x]
    equal = [i for i in arr if i == x]
    greater = [i for i in arr if i > x]

    m = len(less)
    e = len(equal)
    
    if k < m:
        return median_of_medians(less, k)
    elif k < m + e:
        return x
    else:
        return median_of_medians(greater, k - m - e)
```
Time complexity: $O(n)$ в худшем случае.
