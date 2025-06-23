# 18. AVL-деревья: идея, операции вставки и удаления, балансировка, оценка сложности операций

[[17_Множество_и_словарь_на_БДП|← Предыдущий билет]] | [[19_Красно_черные_деревья|→ Следующий билет]]

---

## 🌳 Что такое AVL-дерево

**AVL-дерево** — это **самобалансирующееся двоичное дерево поиска**, в котором **разница высот** между левым и правым поддеревом **любого узла не превышает 1**.

> Названо в честь изобретателей: Адельсон-Вельский и Ландис (1962)

---

## ⚖️ Балансировка

**Баланс-фактор (BF)** узла = `высота(правое) − высота(левое)`

Допустимые значения BF: -1, 0, +1

Если нарушено — требуется **балансировка с помощью поворотов**:

- LL: правый поворот
    
- RR: левый поворот
    
- LR: левый поворот у потомка, потом правый
    
- RL: правый поворот у потомка, потом левый
    

---

## 🔁 Визуализация поворотов

### ▶️ LL (Left-Left) — правый поворот

```
      30                20
     /     →          /  \
   20               10   30
  /
10
```

### ▶️ RR (Right-Right) — левый поворот

```
  10                  20
    \     →         /   \
    20             10   30
      
      30
```

### ▶️ LR (Left-Right) — сначала левый, затем правый

```
    30               30              20
   /     →          /      →       /  \
  10              20              10  30
    \            /
     20         10
```

### ▶️ RL (Right-Left) — сначала правый, затем левый

```
  10                10                 20
    \     →           \       →       /  \
     30               20             10  30
    /                   \
   20                   30
```

---

## 🧱 Структура узла AVL

```java
class Node {
    int key, height;
    Node left, right;

    Node(int key) {
        this.key = key;
        this.height = 1;
    }
}
```

---

## ➕ Вставка с балансировкой

```java
Node insert(Node node, int key) {
    if (node == null) return new Node(key);

    if (key < node.key) node.left = insert(node.left, key);
    else if (key > node.key) node.right = insert(node.right, key);
    else return node;

    updateHeight(node);
    return balance(node);
}

void updateHeight(Node node) {
    node.height = 1 + Math.max(height(node.left), height(node.right));
}

int height(Node node) {
    return node == null ? 0 : node.height;
}
```

---

## ⚙️ Балансировка

```java
Node balance(Node node) {
    int bf = getBalance(node);

    if (bf > 1) {
        if (getBalance(node.right) < 0)
            node.right = rotateRight(node.right); // RL
        return rotateLeft(node); // RR
    }

    if (bf < -1) {
        if (getBalance(node.left) > 0)
            node.left = rotateLeft(node.left); // LR
        return rotateRight(node); // LL
    }

    return node;
}

int getBalance(Node node) {
    return height(node.right) - height(node.left);
}
```

---

## 🔄 Реализация поворотов

```java
Node rotateRight(Node y) {
    Node x = y.left;
    Node T2 = x.right;

    x.right = y;
    y.left = T2;

    updateHeight(y);
    updateHeight(x);

    return x;
}

Node rotateLeft(Node x) {
    Node y = x.right;
    Node T2 = y.left;

    y.left = x;
    x.right = T2;

    updateHeight(x);
    updateHeight(y);

    return y;
}
```

---

## ❌ Удаление

1. Ищем элемент и удаляем его, как в BST
    
2. После удаления — `updateHeight()` и `balance()` вверх по дереву
    

> Балансировка выполняется на каждом уровне пути от удаляемого узла к корню

---

## 📈 Сложность операций

|Операция|Сложность|
|---|---|
|Поиск|O(log n)|
|Вставка|O(log n)|
|Удаление|O(log n)|

AVL-деревья поддерживают **гарантированно логарифмическую высоту** даже в худшем случае

---

## ✅ Выводы

- AVL-дерево строго сбалансировано: разница высот ≤ 1
    
- Использует повороты (LL, RR, LR, RL) для восстановления баланса
    
- Обеспечивает эффективные операции поиска, вставки и удаления
    

---

[[19_Красно_черные_деревья|→ Следующий билет: Красно-черные деревья]]