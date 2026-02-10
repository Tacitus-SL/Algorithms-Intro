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
- [2. Сортировки](#2-сортировки)
  - [Insertion Sort](#21-insertion-sort)
  - [Bubble Sort](#22-bubble-sort)
  - [Selection Sort](#23-selection-sort)
  - [Merge Sort](#24-merge-sort-divide-and-conquer)
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

## 2. Сортировки
### 2.1 Insertion Sort
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
Time complexity: O(n²)

### 2.2 Bubble Sort
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
Time complexity: O(n²)

### 2.3 Selection Sort
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
Time complexity: O(n²)

### 2.4 Merge Sort (Divide and Conquer)
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
Time complexity: O(n log n)
