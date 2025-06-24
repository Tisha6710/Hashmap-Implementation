# Hashmap-Implementation
package HashMap;
import java.util.*;
public class Implementation {
static class MyHashMap<K,V>{
    public static final int DEFAULT_CAPACITY=4;
    public static final float DEFAULT_LOAD_FACTOR=0.75f;
    private class Node{
        K key;
        V value;
        Node(K key,V value){
            this.key=key;
            this.value=value;

        }

    }

    private int n;
     private LinkedList<Node>[] buckets;
     private void initBuckets(int N){
         buckets =new LinkedList[N];// N-capacity /size of buckets array
         for(int i=0;i<buckets.length;i++){
             buckets[i] =new LinkedList<>();
         }

     }
     private int HashFun(K key){
         int hc=key.hashCode();
         return (Math.abs(hc)) % buckets.length;
     }
     // traverse the ll and looks for a node with key,If found it return it's index otherwise it returns null
     private int searchInBucket(LinkedList<Node> ll,K key){
for(int i=0;i<ll.size();i++){
    if(ll.get(i).key==key){
        return i;
    }
}
return -1;
     }
    public MyHashMap(){ // constructor
        initBuckets(DEFAULT_CAPACITY);


    }
    private void rehash(){
         LinkedList<Node>[] oldBucket=buckets;
         initBuckets(oldBucket.length*2);
         n=0;
         for(var bucket: oldBuckets){
             for(var node:bucket){
                 put(node.key,node.value);
             }


         }

    }
    public int size(){ // return the number of entries in map
       return n;
    }
    public void put(K key,V value){// insert/update
         int bi=HashFun(key);
         LinkedList<Node> currBucket =buckets[bi];

         int ei=searchInBucket(currBucket,key);
         if(ei==-1){ // key doees not exist,insert a new node
             Node node=new Node(key,value);
          currBucket.add(node);
          n++;

         } else{ // update case
             Node currNode=currBucket.get(ei);
             currNode.value=value;

         }
         if(n>=buckets.length*DEFAULT_LOAD_FACTOR){
             rehash();
         }


    }
    public V get(K key){
         int bi=HashFun(key);
         LinkedList<Node> currBucket =buckets[bi];
         int ei=searchInBucket(currBucket,key);
         if(ei!=-1){ //key exist
             Node currNode=currBucket.get(ei);
             return currNode.value;

         }  // key doesnt exist
             return null;



    }
    public V remove(K key){
        int bi=HashFun(key);
        LinkedList<Node> currBucket =buckets[bi];
        int ei=searchInBucket(currBucket,key);
        if(ei!=-1){  // key exists
            Node currNode=currBucket.get(ei);
            V  val=currNode.value;
            currBucket.remove(ei);
            n--;
            return val;

        }  // key doesn't exist
            return null;


    }
}
public static void main(String[] args) {
    MyHashMap<String,Integer> mp= new MyHashMap<>();
    System.out.println("testing put");
    mp.put("a",1);
    mp.put("b",2);
    mp.put("c",3);
    mp.put("x",61);
    mp.put("y",71);
    System.out.println("testing size" +mp.size());
    mp.put("c",30);
    System.out.println("Testing size"+ mp.size());
// testing get
    System.out.println(mp.get("x")); //1
    System.out.println(mp.get("y"));   // 2
//    System.out.println(mp.get("c"));   // 30
//    System.out.println(mp.get("college")); // null
//    System.out.println(mp.remove("c")); //30
//    System.out.println(mp.remove("c")); // null
//    System.out.println("Testing size"+mp.size()); //2



    }
}
