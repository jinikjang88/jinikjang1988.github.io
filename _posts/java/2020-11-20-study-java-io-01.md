---
layout: post
title: "Java Input/Output Stream"
subheading: Java의 I/O에 대하여
author: Jeffrey
categories: Java
banner: "/assets/images/banners/java.jpeg"
tags: Java java.io JavaIO
---

## Java I/O 이해하기 

자바 I/O는 스트림(stream)으로 정의된다.  
스트림은 source(입력 스트림) 또는 destination(출력 스트림)을 가지는 정렬된 일련의 데이터 들이다.

I/O 클래스들을 사용하면 프로그래머는 파일들을 통해서 시스템 자원들을 사용할 수 있으며 하부의 OS의 특징과는 무관하게 작업할 수 있다.

자바 I/O를 이해하는 가장 좋은 방법은 기본 인터페이스와 추상 클래스들을 이해하는 것이다.

## 목차
    1. InputStream
    2. OutputStream

### 1. InputStream
추상 클래스인 InputStream은 특정 소스에서 입력을 읽어오는 메소드들을 선언하고 있다.  
InputStream은 java.io에 있는 모든 입력 스트림들의 기본 클래스(base class)이다.  
그리고 아래와 같은 메소드들을 제공한다.  


- public InputStream()  
    인자 없는 생성자를 지원한다.
      
- public abstract int read() throws IOException  
    한 바이트의 데이터를 읽어서 돌려준다. 값의 범위는 0에서 255이지, -128에서 127이 아니다. 스트림에 끝에 도달한 경우 -1을 돌려준다. 이 메소드는 입력을 받을 떄까지 기다린다.

- public int read(byte[] buf) throws IOException  
    바이트 배열로 읽어들인다. 이 메소드는 입력을 받을 때까지 기다린다. 그런 뒤 최대한 buf.length 개수의 바이트까지 읽어서 buf를 채운다. 실제로 읽어들인 바이트 개수를 돌려준다. 스트림 끝에 도달한 경우 -1을 돌려준다.

- public int read(byte[] buf, int off, int len) throws IOException  
    바이트 배열의 일부분으로 읽어드린다. 이 메소드는 입력을 받을 때까지 기다린다. 그런 뒤 buf의 off 위치부터 len byte까지 또는 buf의 끝에 도달할 떄까지 buf를 채운다.

- public long skip(long count) throws IOException  
    입력에서 count 바이트만큼 또는 입력 스트림의 끝으로 건너뛴다. 실제로 건너뛴 바이트 개수를 돌려준다.

- public int available() throws IOException  
    기다리지 않고 읽을 수 있는 바이트 개수를 돌려준다.
    
- public void close() throws IOException  
    입력 스트림을 닫는다. 이 메소드는 스트림과 관련되 모든 리소스(file descriptors 등)를 반환하기 위해서 반드시 불려져야한다. 만일 이 메소드가 불려지지 않으면 관련된 리소스는 사용 중인 상태로 남게된다. 쓰레기 수집기가 그 스트림의 finalize 메소드를 수행하기 전까지 이런 상태로 남아있게 된다.


```java
import java.io.*;
import java.util.Scanner;

public class CountSpace {

    public static void main(String[] args) throws IOException {
        InputStream inputStream;

        if (args.length == 0) {
            System.out.println("Please Input");
            Scanner sc = new Scanner(System.in);
            inputStream = new ByteArrayInputStream(sc.nextLine().getBytes());
        } else {
            inputStream = new FileInputStream(args[0]);
        }

        int ch;
        int total;
        int spaces = 0;

        for (total = 0; (ch = inputStream.read()) != -1; total++) {
            if (Character.isSpaceChar(ch)) {
                spaces++;
            }
        }

        System.out.println(total + " chars, " + spaces + " spaces");
    }
}
```

위의 프로그램은 명령어 라인에서 입력된 파일 이름을 취하거나 표준 입력 스트림인 System.in 에서 읽어온다.  
for 루프는 Character 클래스의 isSpaceChar 메소드를 사용하여 읽어들인 문자가 공백 문자인지를 검사하여 파일 안의 총 문자 개수와 공백 문자를 센다.   

### 2. OutputStream
추상 클래스인 OutputStream은 InputStream과 유사하다. 이것은 출력할 바이트 스트림의 추상클래스를 제공한다. 

- public OutputStream()
    인자 없는 생성자를 지원한다.
    
- public abstract void write(int b) throws IOException
    b를 바이트형으로 변환하여 쓴다. 바이트가 int 형으로 전달되는데 그 이유는 종종 바이트에 산술연산에 의한 결과가 전달되기 때문이다. 
    바이트를 포함할 수 있는 형이 int이기 때문에 인자를 int로 했다. 이것은 byte로 형 변환(cast)하지 않고 결과를 전달할 수 있음을 의미한다. 
    그러나 하위 8비트만 전달되고 상위 24비트는 잃어버린다는 것에 유념해야 한다. 이 메소드는 쓰기가 완료될 때까지 기다린다.
    
- public void write(byte[] buf) throws IOException
    바이트 배열을 쓴다. 이 메소드는 쓰기가 완료될 때까지 기다린다.
    
- public void write(byte[] buf, int offset, int count) 
    바이트 배열을 buf[offset]부터 count 바이트 만큼까지를 쓴다. 그러나 배열의 끝에 도달하게 되면 쓰기를 중단한다.
    
- public void flush() throws IOException
    스트림을 비운다. 그래서 버퍼 안의 바이트들이 쓰여진 뒤 이 메소드를 사용하여 비워지게 해야 한다.
    
- public void close() throws IOException
    스트림을 닫는다. 관련된 모든 리소스들을 반환하기 위해서 반드시 호출되어야 한다.
    만일 따로 선언되지 않았다면 위 메소드들은 출력 스트림에서 오류를 발견할 때 IOException을 던진다. 


```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

public class Translate {
    public static void main(String[] args) {
        InputStream inputStream = System.in;
        OutputStream outputStream = System.out;

        if(args.length != 2) {
            String from = "abcd";
            String to = "1234";
        } else {
            String from = args[0];
            String to = args[1];
        }

        int ch, i;

        if(from.length() != to.length()) {
            error("from and to must be same length");
        }

        try {
            while ((ch = inputStream.read()) != -1) {
                if ((i = from.indexOf(ch)) != -1) {
                    outputStream.write(to.charAt(i));
                } else {
                    outputStream.write(ch);
                }
            }
        } catch (IOException e) {
            error("I/O Exception : " + e);
        }
    }

    public static void error(String err) {
        System.err.println("Translate: " + err);
        System.exit(1);
    }

}
```

위의 예제는 입력받은 문자열의 일부 문자를 다른 문자로 변환한 뒤 복사하여 출력하는 예제 이다.  
Translate 프로그램은 두 개의 인자를 받아들인다. from 문자열 안의 각 문자는 to 문자열 안의 같은 위치의 문자로 변환된다.
