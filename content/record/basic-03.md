+++
title = "객체 지향 프로그래밍"
date = "2025-02-07T21:00:00+09:00"
draft = false
topic = ["record"]
tag = ["Study", "Java"]
+++

## 절차 지향 프로그래밍
프로그램의 흐름을 순차적으로 따르며 처리하는 방식. 즉, `어떻게` 를 중심으로 프로그래밍 함.

```java
public class MusicPlayerData {
    int volume = 0;
    boolean isOn = false;
}
```
```java
public static void main(String[] args) {
    MusicPlayerData data = new MusicPlayerData();
    // 음악 플레이어 켜기
    on(data);
    // 볼륨 증가
    volumeUp(data);
    // 음악 플레이어 끄기
    off(data);
}

static void on(MusicPlayerData data) {
    data.isOn = true;
    System.out.println("음악 플레이어를 시작합니다.");
}

static void off(MusicPlayerData data) {
    data.isOn = false;
    System.out.println("음악 플레이어를 종료합니다.");
}

static void volumeUp(MusicPlayerData data) {
    data.volume++;
    System.out.println("음악 플레이어 볼륨: " + data.volume);
}
```
데이터와 기능이 분리되어 있으며, 주로 함수로 작업을 처리함.

<br>

## 객체 지향 프로그래밍
실제 세계의 사물이나 사건을 객체로 보고, 이러한 객체들 간의 상호작용을 중심으로 프로그래밍하는 방식이다. 즉, `무엇을` 중심으로 프로그래밍 한다.

```java
public class MusicPlayer {

	int volume = 0;
	boolean isOn = false;

	void on() {
    	isOn = true;
    	System.out.println("음악 플레이어를 시작합니다.");
	}

	void off() {
    	isOn = false;
    	System.out.println("음악 플레이어를 종료합니다.");
	}

	void volumeUp() {
   		 volume++;
   		 System.out.println("음악 플레이어 볼륨: " + volume);
	}

	void volumeDown() {
    	volume--;
    	System.out.println("음악 플레이어 볼륨: " + volume);
	}
}    
```
```java
public static void main(String[] args) {
    MusicPlayer player = new MusicPlayer();
    // 음악 플레이어 켜기
    player.on();
    // 볼륨 증가
    player.volumeUp();
    // 볼륨 감소
    player.volumeDown();
    // 음악 플레이어 끄기
    player.off();
}
```
**캡슐화**를 통해 데이터와 기능을 객체 내부에 묶어 객체 내에서만 데이터를 처리하도록 하고, 외부에서의 직접적인 접근을 제한함.

<br>

## 절차 지향 vs 객체 지향
객체 지향 프로그래밍과 절차 지향 프로그래밍은 서로 대치되는 개념이 아니라, 각기 다른 초점을 가진 두 가지 접근 방식임.

**절차 지향 프로그래밍**은 데이터와 해당 데이터에 대한 처리 방식이 분리되어 있으며, 프로그램의 실행 순서와 흐름에 초점을 맞춤.

반면, **객체 지향 프로그래밍**에서는 데이터와 그 데이터를 처리하는 메서드가 하나의 **객체**로 묶여 있으며, 객체 간의 설계와 관계를 중시함.