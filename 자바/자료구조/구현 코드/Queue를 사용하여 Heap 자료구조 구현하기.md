## MinHeap
+ [문제 링크](https://www.acmicpc.net/problem/1927)
+ 코드

```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;

class MinHeap {
    private PriorityQueue<Integer> heap;

    public MinHeap() {
        heap = new PriorityQueue<>((o1,o2) -> o1 - o2);
    }

    public void insert(int value) {
        heap.offer(value);
    }

    public int delete() {
        if (heap.isEmpty()) return 0;

        Integer poll = heap.poll();
        return poll;
    }
}

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        MinHeap heap = new MinHeap();

        for (int i=0; i<N; i++) {
            int value = Integer.parseInt(br.readLine());
            if (value == 0) {
                System.out.println(heap.delete());
            } else {
                heap.insert(value);
            }
        }
    }

}
```

## MaxHeap
+ [문제 링크](https://www.acmicpc.net/problem/11279) 

+ 코드
```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;

class MaxHeap {
    private PriorityQueue<Integer> heap;

    public MaxHeap() {
        heap = new PriorityQueue<>((o1,o2)->o2 - o1);
    }

    public void insert(int value) {
        heap.offer(value);
    }

    public int delete() {
        if (heap.isEmpty()) return 0;

        Integer poll = heap.poll();
        return poll;
    }
}
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        MaxHeap heap = new MaxHeap();

        for (int i=0; i<N; i++) {
            int value = Integer.parseInt(br.readLine());
            if (value == 0) {
                System.out.println(heap.delete());
            } else {
                heap.insert(value);
            }
        }
    }

}
```
