public class MyHashTable<K, V> {
    private static class HashNode<K, V> {
        private K key;
        private V value;
        private HashNode<K, V> next;
        public HashNode(K key, V value) {
            this.key = key;
            this.value = value;
            this.next = null;
        }
        @Override
        public String toString() {
            return "{" + key + "," + value + "}";
        }
    }
    private HashNode<K, V>[] chainArray; 
    private int M = 11;              
    private int size = 0;   
    @SuppressWarnings("unchecked")
    public MyHashTable() {
        chainArray = (HashNode<K, V>[]) new HashNode[M];
    }
    @SuppressWarnings("unchecked")
    public MyHashTable(int M) {
        this.M = M;
        chainArray = (HashNode<K, V>[]) new HashNode[M];
    }
    private int hash(K key) {
        if (key == null) return 0;
        return Math.abs(key.hashCode() % M);
    }
    public void put(K key, V value) {
        if (key == null) return;
        int index = hash(key);
        HashNode<K, V> current = chainArray[index];
        while (current != null) {
            if (current.key.equals(key)) {
                current.value = value; 
                return;
            }
            current = current.next;
        }
        HashNode<K, V> newNode = new HashNode<>(key, value);
        newNode.next = chainArray[index];
        chainArray[index] = newNode;
        size++;
    }
    public V get(K key) {
        if (key == null) return null;
        int index = hash(key);
        HashNode<K, V> current = chainArray[index];
        while (current != null) {
            if (current.key.equals(key)) {
                return current.value;
            }
            current = current.next;
        }
        return null;
    }
    public V remove(K key) {
        if (key == null) return null;
        int index = hash(key);
        HashNode<K, V> current = chainArray[index];
        HashNode<K, V> previous = null;
        while (current != null) {
            if (current.key.equals(key)) {
                if (previous == null) {
                    chainArray[index] = current.next;
                } else {
                    previous.next = current.next;
                }
                size--;
                return current.value;
            }
            previous = current;
            current = current.next;
        }
        return null; 
    }
    public boolean contains(V value) {
        if (value == null) return false;
        for (int i = 0; i < M; i++) {
            HashNode<K, V> current = chainArray[i];
            while (current != null) {
                if (current.value.equals(value)) {
                    return true;
                }
                current = current.next;
            }
        }
        return false;
    }
    public K getKey(V value) {
        if (value == null) return null;
        for (int i = 0; i < M; i++) {
            HashNode<K, V> current = chainArray[i];
            while (current != null) {
                if (current.value.equals(value)) {
                    return current.key;
                }
                current = current.next;
            }
        }
        return null;
    }
    public int[] getBucketSizes() {
        int[] sizes = new int[M];
        for (int i = 0; i < M; i++) {
            HashNode<K, V> current = chainArray[i];
            int count = 0;
            while (current != null) {
                count++;
                current = current.next;
            }
            sizes[i] = count;
        }
        return sizes;
    }
    
    // ДОПОЛНИТЕЛЬНЫЙ МЕТОД - получить общее количество элементов
    public int size() {
        return size;
    }
}
