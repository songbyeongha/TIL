###  단어가 등장하는 횟수

- parametric search ( binary search 로 c 값을 결정 > 해시 사용 + 정렬 후 중복검사 )
- 개수 세기 > 누적합 구하기 > !!역순!! 으로 정렬 --- 카운팅정렬
- hash function 풀이

```java
import java.util.Arrays;
import java.util.Scanner;

public class Solution {
   static Scanner sc = new Scanner(System.in);
   static int d = 257;
   static long m = Long.MAX_VALUE;
   static int answer;
   static String line;
   static int L;

   public static void main(String[] args) {
      int t = sc.nextInt();
      for (int tc = 1; tc <= t; tc++) {
         answer = 0;
         L = sc.nextInt();
         line = sc.next();
         bsearch(line.length());
         System.out.println("#" + tc + " " + answer);
      }
   }

   private static void bsearch(int max) { // binary search 분할정복
      int low = 1;
      int high = max;
      int mid = 1;
       
      while (low <= high) {
         mid = (low + high) / 2;
         if (test(mid)) {
            answer = mid;
            low = mid + 1;
         } else {
            high = mid - 1;
         }
      }
   }

   private static boolean test(int val) {
      long dx = d;
       
      for (int i = 1; i < val; i++)
         dx = (dx * d) % m;
       
      int end = line.length() - val;
      long ihash = hash(line.substring(0, val));
      long[] hashArr = new long[end + 1];
      hashArr[0] = ihash;

      for (int i = 1; i <= end; i++) {
         long nhash = ((d * ihash + line.charAt(i + val - 1) % m) - line.charAt(i - 1) * dx) % m;
         hashArr[i] = nhash;
         ihash = nhash;
      }
       
      boolean flag = false;
      Arrays.sort(hashArr);
      
      for (int i = 0; i < end; i++) {
         if (hashArr[i] == hashArr[i + 1]) {
            flag = true;
         }
      }
      return flag;
   }

   private static long hash(String key) {
      long h = 0;

      for (int j = 0; j < key.length(); j++)
         h = (d * h + key.charAt(j)) % m;

      return h;
   }
}

```

