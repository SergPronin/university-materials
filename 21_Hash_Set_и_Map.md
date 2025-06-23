# 21. Реализация множества (Set) и словаря (Map) на основе hash-таблицы

[[20_Hash_таблицы|← Предыдущий билет]] | [[22_Графы_основы|→ Следующий билет]]

---

## 🔑 Основная идея

Hash-таблица может быть основой для:

- `Set<T>` — уникальные значения, без привязки к значениям
    
- `Map<K, V>` — пары ключ-значение с уникальными ключами
    

В Java:

- `HashSet` — построен на `HashMap`
    
- `HashMap` — основа для большинства реализаций словарей
    

---

## 🧱 Реализация HashSet (на основе HashMap)

### Внутренняя структура:

```java
class HashSet<T> {
    private HashMap<T, Object> map;
    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public void add(T value) {
        map.put(value, PRESENT);
    }

    public boolean contains(T value) {
        return map.containsKey(value);
    }

    public void remove(T value) {
        map.remove(value);
    }
}
```

> HashSet просто обёртка над HashMap, где значения — фиктивные

---

## 🧱 Реализация HashMap (ключ-значение)

```java
class HashMap<K, V> {
    private static class Entry<K, V> {
        final K key;
        V value;
        Entry<K, V> next;

        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }

    private Entry<K, V>[] table;
    private int size;

    public void put(K key, V value) {
        int index = hash(key);
        Entry<K, V> current = table[index];
        while (current != null) {
            if (current.key.equals(key)) {
                current.value = value;
                return;
            }
            current = current.next;
        }
        Entry<K, V> newEntry = new Entry<>(key, value);
        newEntry.next = table[index];
        table[index] = newEntry;
        size++;
    }

    public V get(K key) {
        int index = hash(key);
        Entry<K, V> current = table[index];
        while (current != null) {
            if (current.key.equals(key)) return current.value;
            current = current.next;
        }
        return null;
    }

    public void remove(K key) {
        int index = hash(key);
        Entry<K, V> current = table[index];
        Entry<K, V> prev = null;
        while (current != null) {
            if (current.key.equals(key)) {
                if (prev == null) table[index] = current.next;
                else prev.next = current.next;
                size--;
                return;
            }
            prev = current;
            current = current.next;
        }
    }

    private int hash(K key) {
        return (key.hashCode() & 0x7fffffff) % table.length;
    }
}
```

---

## ⚙️ Особенности реализации

- Использование **цепочек (LinkedList)** для обработки коллизий
    
- При достижении порога (например, load factor 0.75) выполняется **rehashing**
    
- Современные реализации используют **деревья (TreeNode)** при большом количестве коллизий в одном бакете (в Java 8+)
    

---

## 📈 Сложность операций

|Операция|Среднее|Худшее|
|---|---|---|
|get|O(1)|O(n)|
|put|O(1)|O(n)|
|remove|O(1)|O(n)|

> На практике — очень быстро, если хорошая hash-функция и правильный размер таблицы

---

## ✅ Выводы

- HashMap и HashSet — важные структуры с O(1) доступом
    
- Построены на hash-таблицах с цепочками или открытой адресацией
    
- Java активно использует их внутри библиотек и фреймворков
    

---

[[22_Графы_основы|→ Следующий билет: Графы — основные понятия и представление]]