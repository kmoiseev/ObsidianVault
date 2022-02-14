- Placing a key or value in a Hashtable, synchronizedMap, or Concurrent- Map safely publishes it to any thread that retrieves it from the Map (whether directly or via an iterator);

- Placing an element in a Vector, CopyOnWriteArrayList, CopyOnWrite- ArraySet, synchronizedList, or synchronizedSet safely publishes it to any thread that retrieves it from the collection;
    
- Placing an element on a BlockingQueue or a ConcurrentLinkedQueue safely publishes it to any thread that retrieves it from the queue