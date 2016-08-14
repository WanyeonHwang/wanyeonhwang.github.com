# 자료구조 (Collection & Generic)

<font color="blue">(이 정리 내용은 Headfirt java를 참고해서 만든 자료입니다.)</font>

Java의 collection framework는 상황에 따라 필요한 거의 모든 자료구조를 가지고 있습니다.  
그래서 따로 정렬 알고리즘을 구현할 필요가 없습니다.

특히, Map/Hashtable/ArrayList 등은 Java에서 필수적인 요소들이며  
이것에 대한 자세한 이해가 필요합니다.

## Collection

다음은 **정렬 기능이 없는 예제** 입니다.  
*Jukebox1.java*
````java
import java.util.*;
import java.io.*;

public class Jukebox1 {
    // 곡 제목들을 String으로 구성된 ArrayList에 저장합니다.
    ArrayList<String> songList = new ArrayList<String>;

    // Main method
    public static void main(String[] args) {
        new Jukebox1().go();
    }

    // File Loading을 시작하고 songList ArrayList에 들어있는 내용을 출력합니다.
    public void go() {
        getSongs();
        System.out.println(songList);
    }

    void getSongs() {
        try {
            File file = new File("SongList.txt");
            /*
            [SongList.txt]
                Pink Moon/Nick Drake
                Somersault/Zero 7
                Shiva Moon/Prem Joshua
                Circles/BT
            */
            BufferedReader reader = new BufferedReader(new FileReader(file);
            String line = null;
            while ((line = reader.readLine()) != null) {
                addSong(line);
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }

    void addSong(String lineToParse) {
        String[] tokens = lineToParse.split("/");
        songList.add(tokens[0]);
    }
}
````
**출력 결과**
````
% java Jukebox1
[Pink Moon, Sommerault, Shiva Moon, Circles]
````
그런데, **ArrayList에 대한 sort() 메소드는 없습니다.**  
다른 방식을 알아봐야 합니다.

다음은 collection의 종류를 살펴보면 다음과 같습니다.

#### Collection의 종류
- **ArrayList**
: 일반적으로 많이 사용하는 collection
- **TreeSet**
: 원소들을 정렬된 상태로 유지하면서 원소가 중복되어 들어가지 않게 합니다.
- **HashMap**
: 원소들을 [이름/값] 세트와 같은 형식으로 저장하고 접근할 수 있습니다.
- **LinkedList**
: Collection 중간에서 원소를 추가하거나 삭제하는 작업을 더 빠르게 처리할 수 있습니다.  
(사실 ArrayList를 써도 상관은 없습니다.)
- **HashSet**
: Collection에 중복된 원소가 들어가지 않도록 해주고, collection 내에서 어떤 원소를 빠를게 찾을 수 있게 합니다.
- **LinkedHashMap**
: 일반 HashMap과 거의 똑같지만 원소([이름/값] 세트)가 <u>삽입된 순서를 그대로 유지</u>시켜줄 수도 있고, <u>원소에 마지막으로 접근했던 순서를 기억하도록 설정할 수도 있다</u>는 점이 다릅니다.


#### 정렬 기능 추가 방법
**1. TreeSet 사용**
ArrayList 대신에 TreeSet에 넣으면 자동으로 String들이 정렬됩니다.

**2. Collection.sort() 메소드 사용**
다음과 같이 추가 해줄 수 있습니다.
````java
public class Jukebox1 {
    ...
    public void go() {
        getSongs();
        System.out.println(songList);

        // 여기에 Collections.sort() 추가합니다.
        Collections.sort(songList);
        System.out.println(songList);   // 다실 출력될 때는 정렬된 결과입니다.
    }
    ...
}
````
> Collections.sort() method  
String으로 구성된 목록을 알파벳순으로 정렬해줍니다.

하지만 우리는 String 뿐만 아니라 일반적인 객체인 Song 객체를 정렬할 줄 알아야 합니다.


### 객체 정렬
위에서 살펴본 소스는 곡명/음악가 로 구성되어 있었습니다.  
그중에서 곡명만 사용해서 정렬하려고 했지요.  
지금부터는 4개의 정보를 가지고 있는 Song 객체를 사용해서 정렬을 해보려고 합니다.

song 클래스는 다음과 같습니다.
````java
class Song {
    // 파일의 4가지 속성을 저장하기 위한 4개의 인스턴스 변수
    String title;
    String artist;
    String rating;
    String bpm;

    // 변수는 모두 song 객체가 생성될 떄 생성자에서 설정 됩니다.
    Song(String t, String a, String r, String b) {
        title =t;
        artist = a;
        rating = r;
        bpm = b;
    }

    public String getTitle() {
        return title;
    }

    public String getArtist() {
        return artist;
    }

    public String getRating() {
        return rating;
    }

    public String getBpm() {
        return bpm;
    }

    // System.out.println(aSongObject) 메소드를 호출하였을 때
    // 곡 제목이 출력될 수 있도록하기 위해 toString()을 오버라이드합니다.
    // System.out.println(aListOfSongs)를 호출하면 목록에 들어있는
    // 각 원소의 toString() 메소드가 호출됩니다.
    public String toString() {
        return title;
    }
}
````
**String 대신 Song을 사용하도록 Jukebox1을 수정하기**
```java
import java.util.*;
import java.io.*;

public class Jukebox1 {
    // String이 아닌 Song으로 구성된 ArrayList를 만듭니다.
    // ArrayList<String> songList = new ArrayList<String>;
    ArrayList<Song> songList = new ArrayList<Song>;
    public static void main(String[] args) {
        new Jukebox1().go();
    }

    public void go() {
        getSongs();
        System.out.println(songList);
        Collections.sort(songList);
        System.out.println(songList);
    }

    void getSongs() {
        try {
            File file = new File("SongList.txt");
            BufferedReader reader = new BufferedReader(new FileReader(file);
            String line = null;
            while ((line = reader.readLine()) != null) {
                addSong(line);
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }

    void addSong(String lineToParse) {
        String[] tokens = lineToParse.split("/");
        // songList.add(tokens[0]);
        Song nextSong = new Song(tokens[0], tokens[1], tokens[2], tokens[3]);
        songList.add(nextSong);
    }
}
````
**그런데!!!!!!!!!! 컴파일이 안됩니다.!!!**  
무엇인 문제인걸까요?  
