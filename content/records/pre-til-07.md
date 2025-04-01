+++
title = "[사전캠프] 9일차."
date = "2025-01-23T23:41:13+09:00"
draft = false
topic = ["records"]
tag = ["사전캠프", "Java"]
+++

>**TASKS**  
✅ 달리기반 Java 보너스 문제  
>
>**+**  
☑️ 자바의 정석 강의  

---

## Java


### 보너스 문제: 가위 바위 보
![](https://velog.velcdn.com/images/ezro/post/03819eb6-b262-4cbe-8882-06c7ae3f042e/image.png)


### 첫 번째 코드

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Random;
import java.util.Scanner;

public class RockPaperScissors {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();

        Map<Integer, String> prize = new HashMap<>();
        prize.put(1, "10,000원");
        prize.put(2, "20,000원");
        prize.put(3, "30,000원");
        prize.put(4, "50,000원");
        prize.put(5, "100,000원");

        String[] rps = {"가위", "바위", "보"};

        int gameCount = 0;
        int winCount = 0;
        boolean playing = true;

        System.out.println("*********** GAME START ***********");

        while (playing) {

            System.out.println("------------- GAME " + (gameCount + 1) + " -------------");
            System.out.print("가위 바위 보! ");
            String user = scanner.nextLine();
            String pc = rps[random.nextInt(rps.length)];

            System.out.print("[당신: " + user + ", 상대: " + pc + "] ");

            switch (user) {
                case "가위":
                    if (pc.equals("바위")) {
                        System.out.println("당신이 졌습니다!");
                        gameCount++;
                        break;
                    } else if (pc.equals("보")) {
                        System.out.println("당신이 이겼습니다!");
                        gameCount++;
                        winCount++;
                        break;
                    } else {
                        System.out.println("무승부입니다!");
                        gameCount++;
                        break;
                    }
                case "바위":
                    if (pc.equals("보")) {
                        System.out.println("당신이 졌습니다!");
                        gameCount++;
                        break;
                    } else if (pc.equals("가위")) {
                        System.out.println("당신이 이겼습니다!");
                        gameCount++;
                        winCount++;
                        break;
                    } else {
                        System.out.println("무승부입니다!");
                        gameCount++;
                        break;
                    }
                case "보":
                    if (pc.equals("바위")) {
                        System.out.println("당신이 이겼습니다!");
                        gameCount++;
                        break;
                    } else if (pc.equals("가위")) {
                        System.out.println("당신이 졌습니다!");
                        gameCount++;
                        winCount++;
                        break;
                    } else {
                        System.out.println("무승부입니다!");
                        gameCount++;
                        break;
                    }
                default:
                    System.out.println("잘못된 입력입니다!");
                    break;
            }

            if (gameCount == 5) {
                System.out.println("----------------------------------");
                if (winCount > 0) {
                    System.out.println("총 " + winCount + "회 승리했습니다. 상품: " + prize.get(winCount));
                } else {
                    System.out.println("상품을 획득하지 못했습니다.");
                }
                System.out.println("************ GAME END ************");
                playing = false;
                break;
            }
        }
    }
}
```


### 개선 코드
```java
import java.util.HashMap;
import java.util.Map;
import java.util.Random;
import java.util.Scanner;

public class RpsRef {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();

        Map<Integer, String> prize = new HashMap<>();
        prize.put(1, "10,000원");
        prize.put(2, "20,000원");
        prize.put(3, "30,000원");
        prize.put(4, "50,000원");
        prize.put(5, "100,000원");

        String[] rps = {"가위", "바위", "보"};

        int gameCount = 0;
        int winCount = 0;

        System.out.println("*********** GAME START ***********");

        // 게임은 5회로 고정되어 있으므로 횟수 제한으로 변경
        while (gameCount < 5) {

            System.out.println("------------- GAME " + (gameCount + 1) + " -------------");
            System.out.print("가위 바위 보! ");
            String user = scanner.nextLine();

            // 사용자 입력 값 검사
            if (!checkInput(user, rps)) {
                System.out.println("잘못된 입력입니다!");
                continue;
            }

            String pc = rps[random.nextInt(rps.length)];
            System.out.print("[당신: " + user + ", 상대: " + pc + "] ");

            // 승패 판정: 중복 코드 제거
            int result = getResult(user, pc);
            
            switch (result) {
                case 1:
                    System.out.println("당신이 이겼습니다!");
                    winCount++;
                    break;
                case -1:
                    System.out.println("당신이 졌습니다!");
                    break;
                case 0:
                    System.out.println("무승부입니다!");
                    break;
            }
            
            gameCount++;

            if (gameCount == 5) {
                System.out.println("----------------------------------");
                if (winCount > 0) {
                    System.out.println("총 " + winCount + "회 승리했습니다. 상품: " + prize.get(winCount));
                } else {
                    System.out.println("꽝! 상품을 획득하지 못했습니다.");
                }
                System.out.println("************ GAME END ************");
                break;
            }
        }
    }

    // 사용자 입력 값 검사: 가위 바위 보가 아니면 false
    public static boolean checkInput(String input, String[] rps) {
        for (String check : rps) {
            if (check.equals(input)) {
                return true;
            }
        }
        return false;
    }

    // 승패 판정: 1 = 승리, -1 = 패배, 0 = 무승부
    public static int getResult(String user, String pc) {
        if (user.equals(pc)) {
            return 0; // 무승부
        }
        if ((user.equals("가위") && pc.equals("보")) ||
                (user.equals("바위") && pc.equals("가위")) ||
                (user.equals("보") && pc.equals("바위"))) {
            return 1; // 승리
        }
        return -1; // 패배
    }
}
```

![](https://velog.velcdn.com/images/ezro/post/b07a596c-bb42-4fff-8477-2f600eef0c1e/image.gif)
