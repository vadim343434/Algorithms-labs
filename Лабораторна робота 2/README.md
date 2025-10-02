# ЛР. 2. Варіант 19.
# Python-код ітеративної реалізації алгоритму сортування злиттям.

```
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
```
