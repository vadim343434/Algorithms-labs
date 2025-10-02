```
# ЛР. 2. Варіант 19.
# Лістинг 2.1 - Python-код ітеративної реалізації алгоритму сортування злиттям.


def merge_sort_iterative(a):
    n = len(a)
    comparisons = 0
    assignments = 0
    i = 1
    while i < n:
        j = 0
        while j < n - i:
            left = j
            mid = j + i
            right = min(j + 2 * i, n)
            # Викликаємо допоміжну функцію merge, яка повертає підраховані операції
            c, a_count = merge(a, left, mid, right)
            comparisons += c
            assignments += a_count
            j += 2 * i
        i *= 2
    return a, comparisons, assignments


def merge(a, left, mid, right):
    comparisons = 0
    assignments = 0
    n1 = mid - left
    n2 = right - mid
    # Створюємо тимчасові підмасиви
    L = a[left:mid]
    R = a[mid:right]
    assignments += n1 + n2
    it1 = 0
    it2 = 0
    k = left
    assignments += 3  # присвоєння it1, it2, k

  // Зливаємо елементи, порівнюючи їх
    while it1 < n1 and it2 < n2:
        comparisons += 1
        if L[it1] <= R[it2]:
            a[k] = L[it1]
            it1 += 1
            assignments += 2
        else:
            a[k] = R[it2]
            it2 += 1
            assignments += 2
        k += 1
        assignments += 1

  // Копіюємо елементи, що залишилися з першого підмасиву
    while it1 < n1:
        a[k] = L[it1]
        it1 += 1
        k += 1
        assignments += 2

  // Копіюємо елементи, що залишилися з другого підмасиву
    while it2 < n2:
        a[k] = R[it2]
        it2 += 1
        k += 1
        assignments += 2

   return comparisons, assignments


// Приклад використання для варіанта 19
my_list = [80, 27, 37, 36, 91, 53, 86, 66, 98]
print("Оригінальний список:", my_list)
sorted_list, comps, assigs = merge_sort_iterative(my_list.copy())
print("Відсортований список:", sorted_list)
print(f"Кількість порівнянь: {comps}")
print(f"Кількість присвоювань: {assigs}")

# Лістинг 2.2 – Python-код рекурсивної реалізації алгоритму сортування злиттям.

def merge_sort_recursive_with_counters(arr):
    comparisons = 0
    assignments = 0
    recursive_calls = 1  # рахуємо поточний кадр

    if len(arr) <= 1:
        return arr, comparisons, assignments, recursive_calls

    mid = len(arr) // 2
    assignments += 1  # присвоєння mid

    # Рекурсивно ділимо масив на дві половини
    left_half, c1, a1, r1 = merge_sort_recursive_with_counters(arr[:mid])
    right_half, c2, a2, r2 = merge_sort_recursive_with_counters(arr[mid:])
    comparisons += c1 + c2
    assignments += a1 + a2
    recursive_calls += r1 + r2

    # Зливаємо відсортовані половини
    merged_arr, c_merge, a_merge = merge(left_half, right_half)
    comparisons += c_merge
    assignments += a_merge

    return merged_arr, comparisons, assignments, recursive_calls


def merge(left, right):
    merged_arr = []
    comparisons = 0
    assignments = 0
    i = 0
    j = 0

    # Зливаємо елементи з обох масивів
    while i < len(left) and j < len(right):
        comparisons += 1
        if left[i] <= right[j]:
            merged_arr.append(left[i])
            i += 1
            assignments += 1
        else:
            merged_arr.append(right[j])
            j += 1
            assignments += 1

    # Додаємо елементи, що залишилися
    while i < len(left):
        merged_arr.append(left[i])
        i += 1
        assignments += 1

    while j < len(right):
        merged_arr.append(right[j])
        j += 1
        assignments += 1

    return merged_arr, comparisons, assignments


# Приклад використання (варіант 19)
my_list = [80, 27, 37, 36, 91, 53, 86, 66, 98]
print("Оригінальний список:", my_list)

sorted_list, total_comparisons, total_assignments, total_recursive_calls = \
    merge_sort_recursive_with_counters(my_list)

print("Відсортований список:", sorted_list)
print(f"Загальна кількість порівнянь: {total_comparisons}")
print(f"Загальна кількість присвоювань: {total_assignments}")
print(f"Загальна кількість рекурсивних викликів: {total_recursive_calls}")

# Лістинг 2.3 – Python-код рекурсивної реалізації алгоритму швидкого сортування за схемою Хоара

def quicksort(a, l, r):
    """
    Рекурсивна реалізація алгоритму швидкого сортування
    з підрахунком операцій, за схемою Хоара.
    """
    comparisons = 0
    assignments = 0
    recursive_calls = 1
    if l < r:
        q, c1, a1 = partition(a, l, r)
        comparisons += c1
        assignments += a1
        # Рекурсивні виклики для лівого та правого підмасивів
        c2, a2, r2 = quicksort(a, l, q)
        c3, a3, r3 = quicksort(a, q + 1, r)
        comparisons += c2 + c3
        assignments += a2 + a3
        recursive_calls += r2 + r3
    else:
        return 0, 0, 1
    return comparisons, assignments, recursive_calls


def partition(a, l, r):
    """
    Допоміжна функція для розділення масиву за схемою Хоара.
    Повертає (j, comparisons, assignments), де j — індекс поділу.
    """
    comparisons = 0
    assignments = 0

    pivot = a[l]
    assignments += 1

    i = l - 1
    j = r + 1
    assignments += 2

    while True:
        # Шукаємо елемент, що більший за опорний
        i += 1
        assignments += 1
        while a[i] < pivot:
            comparisons += 1
            i += 1
            assignments += 1

        # Порівняння для виходу з циклу
        comparisons += 1

        # Шукаємо елемент, що менший за опорний
        j -= 1
        assignments += 1
        while a[j] > pivot:
            comparisons += 1
            j -= 1
            assignments += 1

        # Порівняння для виходу з циклу
        comparisons += 1

        comparisons += 1
        if i >= j:
            return j, comparisons, assignments

        # Обмін елементів
        a[i], a[j] = a[j], a[i]
        assignments += 3


# Приклад використання (варіант 19)
my_list = [80, 27, 37, 36, 91, 53, 86, 66, 98]
original_list = my_list.copy()

print("Оригінальний список:", original_list)

total_comparisons, total_assignments, total_recursive_calls = \
    quicksort(my_list, 0, len(my_list) - 1)

print("Відсортований список:", my_list)
print(f"Загальна кількість порівнянь: {total_comparisons}")
print(f"Загальна кількість присвоювань: {total_assignments}")
print(f"Загальна кількість рекурсивних викликів: {total_recursive_calls}")


```
