# Collections案例

![image-20220127125557061](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220127125557061.png)

```java
package d4_collections_test;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

/**
 * 功能:
 *      1. 做牌
 *      2. 洗牌
 *      3. 定义3个玩家
 *      4. 发牌
 *      5. 排序
 *      6. 看牌
 */
public class GameDemo {
    /**
     * 定义一个静态集合存储54张牌对象
     */
    public static List<Card> allCards = new ArrayList<>();

    /**
     *2. 做牌: 定义静态代码块初始化牌数据
     */
    static{
        //3. 定义点数: 个数确定, 类型确定, 使用数组
        String[] sizes = {"3", "4", "5", "6", "7", "8", "9", "10",
        "J", "Q", "K", "A", "2"};
        //4.定义花色: 个数确定, 类型确定, 使用数组
        String[] colors = {"♠", "♥", "♣", "♦"};
        //5. 组合点数和花色
        int index = 0;//记录牌的大小
        for(String size : sizes){
            index++;
            for(String color : colors){
                //6. 封装为一个牌对象
                Card c = new Card(size, color, index);
                //7. 存入集合容器中
                allCards.add(c);
            }
        }
        //8. 大小王存入集合对象中
        Card c1 = new Card("", "🃏", ++index);
        Card c2 = new Card("", "👲", ++index);
        Collections.addAll(allCards, c1, c2);
        System.out.println(allCards);
    }

    public static void main(String[] args) {
        //9. 洗牌
        Collections.shuffle(allCards);
        System.out.println("洗牌后: " + allCards);

        //10. 发牌(定义三个玩家, 每个玩家的牌也是一个集合容器)
        List<Card> wang = new ArrayList<>();
        List<Card> chu = new ArrayList<>();
        List<Card> ma = new ArrayList<>();

        //11. 开始发牌(从牌集合中发出51张牌, 剩余三张作为底牌)
        for(int i = 0; i < allCards.size() - 3; ++i){
            //先拿到当前牌对象
            Card c = allCards.get(i);
            switch (i % 3){
                case 0:
                    wang.add(c);
                    break;
                case 1:
                    chu.add(c);
                    break;
                case 2:
                    ma.add(c);
                    break;
            }
        }

        //12. 拿到最后三张底牌
        List<Card> lastThreeCards = allCards.subList(allCards.size()-3, allCards.size());

        //13. 给玩家的牌排序(从大到小排序)
        sortCards(wang);
        sortCards(chu);
        sortCards(ma);

        //14. 输出玩家的牌
        System.out.println("wang: " + wang);
        System.out.println("chu: " + chu);
        System.out.println("ma: " + ma);
        System.out.println("三张底牌: " + lastThreeCards);
    }

    /**
     * 给牌排序
     * @param cards
     */
    public static void sortCards(List<Card> cards){
        Collections.sort(cards,(o1, o2) -> o2.getWeight() - o1.getWeight());
    }
}
```

```java
package d4_collections_test;

public class Card {
    private String size;
    private String color;
    private int weight;//牌的真正大小

    public Card() {
    }

    public Card(String size, String color, int weight) {
        this.size = size;
        this.color = color;
        this.weight = weight;
    }

    public String getSize() {
        return size;
    }

    public void setSize(String size) {
        this.size = size;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getWeight(){
        return weight;
    }

    public void setWeight(int weight){
        this.weight = weight;
    }

    @Override
    public String toString() {
        return size + color;
    }
}
```