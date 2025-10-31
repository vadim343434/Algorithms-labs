```
# Лістинг 4.1 – Python-код реалізації алгоритмів хешуванням з роздільними ланцюжками та візуалізації відкритої хеш-таблиці
# Варіант 19 — "Шануй батька й матір, то буде тобі в житті добре."

# Константа розміру таблиці
M = 13

# Список вхідних слів
WORDS = ["ШАНУЙ", "БАТЬКА", "Й", "МАТІР", "ТО", "БУДЕ", "ТОБІ", "В", "ЖИТТІ", "ДОБРЕ"]

# Словник позицій українських літер (за рисунком 4.3)
LETTER_POSITIONS = {
    'А':1,  'Б':2,  'В':3,  'Г':4,  'Ґ':5,  'Д':6,
    'Е':7,  'Є':8,  'Ж':9,  'З':10, 'И':11, 'І':12,
    'Ї':13, 'Й':14, 'К':15, 'Л':16, 'М':17, 'Н':18,
    'О':19, 'П':20, 'Р':21, 'С':22, 'Т':23, 'У':24,
    'Ф':25, 'Х':26, 'Ц':27, 'Ч':28, 'Ш':29, 'Щ':30,
    'Ь':31, 'Ю':32, 'Я':33
}

def simple_hash_from_map(key: str) -> int:
    """
    Хеш-функція: h(k) = (сума позицій букв) mod 13.
    Використовує позиції з LETTER_POSITIONS.
    """
    sum_of_positions = 0
    for char in key:
        position = LETTER_POSITIONS.get(char, 0)
        sum_of_positions += position
    hash_address = sum_of_positions % M
    return hash_address

def build_open_hash_table(words: list, m: int) -> list:
    """Будує хеш-таблицю з ланцюжками (списками)."""
    hash_table = [[] for _ in range(m)]
    for word in words:
        address = simple_hash_from_map(word)
        hash_table[address].append(word)
    return hash_table

def display_hash_table(table: list):
    """Виводить хеш-таблицю у зручному форматі."""
    print("\n--- Результат хешування (Таблиця M=13) ---")
    for i, chain in enumerate(table):
        print(f"Індекс {i:02d}: {chain}")

# Виконання:
hash_table = build_open_hash_table(WORDS, M)
display_hash_table(hash_table)



# Лістинг 4.2 – Python-код реалізації алгоритмів хешуванням з відкритою адресацією та візуалізації закритої хеш-таблиці
# Варіант 19 — "Шануй батька й матір, то буде тобі в житті добре."

# Константа розміру таблиці
M = 13

# Список вхідних слів
WORDS = ["ШАНУЙ", "БАТЬКА", "Й", "МАТІР", "ТО", "БУДЕ", "ТОБІ", "В", "ЖИТТІ", "ДОБРЕ"]

# Словник позицій українських літер (за рисунком 4.3)
LETTER_POSITIONS = {
    'А':1,  'Б':2,  'В':3,  'Г':4,  'Ґ':5,  'Д':6,
    'Е':7,  'Є':8,  'Ж':9,  'З':10, 'И':11, 'І':12,
    'Ї':13, 'Й':14, 'К':15, 'Л':16, 'М':17, 'Н':18,
    'О':19, 'П':20, 'Р':21, 'С':22, 'Т':23, 'У':24,
    'Ф':25, 'Х':26, 'Ц':27, 'Ч':28, 'Ш':29, 'Щ':30,
    'Ь':31, 'Ю':32, 'Я':33
}

def primary_hash(key: str) -> int:
    """h(k) = (сума позицій букв) mod M."""
    sum_of_positions = 0
    for char in key:
        position = LETTER_POSITIONS.get(char, 0)
        sum_of_positions += position
    return sum_of_positions % M

def build_closed_hash_table(words: list, m: int) -> list:
    """Будує хеш-таблицю з відкритою адресацією, використовуючи лінійне дослідження."""
    hash_table = [None] * m
    for word in words:
        start_address = primary_hash(word)
        for i in range(m):
            address = (start_address + i) % m
            if hash_table[address] is None:
                hash_table[address] = word
                break
        else:
            print(f"Помилка: Таблиця заповнена. Не вдалося додати слово: {word}")
    return hash_table

def display_hash_table(table: list):
    """Виводить хеш-таблицю у зручному форматі."""
    print("\n--- Хеш-таблиця (Відкрита адресація, M=13) ---")
    print("Індекс | Слово")
    print("-------|-------")
    for i, item in enumerate(table):
        value = item if item is not None else "(NULL)"
        print(f"{i:02d}     | {value}")

# Виконання:
hash_table = build_closed_hash_table(WORDS, M)
display_hash_table(hash_table)


```
