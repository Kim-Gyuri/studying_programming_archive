## 📌 해쉬테이블(Hash Table)을 자바로 구현하기
+ 구현 포인트
+ 코드
+ get()함수를 호출했을 때 결과
 
### 구현 포인트
+ key를 해시 함수(hash function) 로 계산하여 그 값을 배열의 index로 사용한다.
+ key값을 hash function을 통해서 배열의 인덱스로 바꿔주고, 그 자리에 데이터를 저장한다.
+ 데이터를 저장할 LinkedList를 배열로 선언했다.
+ 인덱스만 알면 해시 테이블이 아무리 커도 데이터에 빠르게 접근할 수 있다.


### 코드
```java
package hackerrank;

import java.util.LinkedList;

class HashTable {

    LinkedList<Node>[] data; // 데이터를 저장할 LinkedList를 배열로 선언한다.

    // 해시 테이블 크기를 미리 정해서, 배열 방을 미리 만들어 놓는다.
    HashTable(int size) {
        this.data = new LinkedList[size];
    }

    class Node {
        String key; // 검색할 key
        String value; //검색할 값
        public Node(String key, String value) {
            this.key = key;
            this.value = value;
        }

        String value() {
            return value;
        }

        void value(String value) {
            this.value = value;
        }
    }

    // 해시 함수 : key 를 해시 코드로 변환한다.
    int getHashCode(String key) {
        int hashcode = 0;
        // key 문자열을 돌면서, char ascii 값을 가져와서
        for (char c : key.toCharArray()) {
            hashcode += c; // 해시코드에 더해준다.
        }
        return hashcode; //해시코드를 반환한다.
    }

    // 해시코드를 받아서 배열 방의 인덱스로 변환해주는 함수를 정의한다.
    int convertToIndex(int hashcode) {
        return hashcode % data.length; // "해시코드/배열방 크기"
    }

     // 데이터 검색
    /*
     * 인덱스로 배열 방을 찾은 이후에
     * 배열방에 노드가 여러 개 존재하는 경우,
     * 검색키를 가지고 해당 노드를 찾아오는 함수를 정의하자.
     */
    Node searchKey(LinkedList<Node> list, String key) {
        if (list == null) return null; // 배열방이 null이면 null을 반환
        for (Node node : list) { //아닌 경우, 배열방 LinkedList를 돌면서
            if (node.key.equals(key)) return node; //노드키가 검색키와 같으면 노드를 반환
        }
        return null; // 아닌 경우, null을 반환
    }

    // 데이터 삽입
    void put(String key, String value) {
        int hashcode = getHashCode(key); // 해시 함수로 해시코드를 반환받는다.
        int index = convertToIndex(hashcode); // 해시코드를 배열의 인덱스로 사용한다.
        System.out.println(key + ", hashcode("+hashcode+"), index(" + index +")");
        LinkedList<Node> list = data[index]; // 해당 하는 인덱스에 데이터를 넣는다.
        if (list == null) { // 배열방이 null이면 LinkedList를 생성해서
            list = new LinkedList<Node>();
            data[index] = list; // 해당 리스트를 배열방에 넣어준다.
        }
        //배열방에 해당 key로 데이터를 가지고 있는지 노드를 가져온다.
        Node node = searchKey(list, key);
        if (node == null) { // 노드가 null이면, 데이터가 없다는 뜻이므로
            list.addLast(new Node(key, value)); //받아온 정보를 가지고 노드 객체를 생성해서 리스트에 추가한다.
        } else {
            node.value(value); //null이 아닌 경우, 해당 노드의 값을 대체해준다.
        }
    }

    String get(String key) {
        int hashcode = getHashCode(key); // 해시 함수로 해시코드를 반환받는다.
        int index = convertToIndex(hashcode); // 해시코드를 배열의 인덱스로 사용한다.
        LinkedList<Node> list = data[index];; // 해당 하는 인덱스에 데이터를 넣는다.
        Node node = searchKey(list, key);  //배열방에 해당 key로 데이터를 가지고 있는지 노드를 가져온다.
        return node == null ? "Not found" : node.value(); // 노드를 찾았다면 해당 value를 반환한다.
    }
}

public class Main {
    public static void main(String[] args) {
        HashTable table = new HashTable(3);
        table.put("jin", "She is a model");
        table.put("sung", "She is pretty");
        table.put("hee", "She is an angel");
        table.put("sung", "She is cute");
        table.put("min", "She is beautiful");
        System.out.println(table.get("sung"));
        System.out.println(table.get("jin"));
        System.out.println(table.get("min"));
        System.out.println(table.get("hee"));
        System.out.println(table.get("jae"));
    }

}
 
 ```

### get()함수를 호출했을 때 결과 
+ 해당 key가 없는 경우 : Not found를 출력한다.
+ 해당 key가 있는 경우 : value가 출력된다.
+ 해당 key가 중복된 경우 (기존 key에 덮어쓴 경우) : 새로 업데이트한 value가 출력된다.
> key가 중복인 경우, 새로 넣은 value로 업데이트된다.  <br>
> `System.out.println(key + ", hashcode("+hashcode+"), index(" + index +")");`으로 출력하여 확인했을 때 <br>
> 해시코드와 인덱스 번호가 중복됨을 확인할 수 있다.

```
jin, hashcode(321), index(0)
sung, hashcode(445), index(1)
hee, hashcode(306), index(0)
sung, hashcode(445), index(1)
min, hashcode(324), index(0)
She is cute
She is a model
She is beautiful
She is an angel
Not found
 ```
