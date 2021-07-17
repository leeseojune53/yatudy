# gRPC란?

[TOC]

## 배경지식

gRPC는 Google에서 개발한 RPC(Remote Procedure) 시스템이다. 전송을 위해 TCP/IP 프로토콜과 HTTP 2.0 프로토콜을 사용하고 IDL(Interface Definition Language)로 protocol buffer를 사용한다.



### RPC(Remote Communication Mechanism)

RPC(원격 프로시저 호출)는 한 프로그램이 네트워크의 세부 정보를 이해하지 않고도 네트워크 안의 다른 컴퓨터에 있는 프로그램에서 서비스를 요청하는 프로토콜이다. RPC는 client-server 모델을 사용한다. 클라이언트에서 서비스를 요청(function call)하면 서버에서 서비스를 제공한다.

> RPC 개념

![img](https://wikihak.com/wp-content/uploads/2019/07/rpc-750x430.jpg)

### HTTP 프로토콜

HTTP(Hypertext Transfer Protocol)는 웹어서 쓰이는 통신 프로토콜이다. 프로토콜이란 상호간에 정의한 규칙을 의미한다.

HTTP 프로토콜은 TCP/IP 프로토콜 위의 레이어(Application layer)에서 동작한다.

![img](https://1.bp.blogspot.com/-YGgmbhSy1W8/XWodfUw_80I/AAAAAAAABrY/iynG65dE74ADmbEfuFAElE9KaSkG-Gd2ACLcBGAs/s1600/%25EC%25BA%25A1%25EC%25B2%2598.JPG)

HTTP 프로토콜은 stateless 프로토콜이다. 여기서 상태가 없다는 의미는 데이터를 주고 받기 위한 각각의 데이터 요청이 서로 독립적으로 관리된다는 의미이다.

HTTP는 기본적으로 서버-클라이언트 구조를 따릅니다. 이 구조에서 HTTP 프로토콜로 데이터를 주고받기 위해서는 Request를 보내고, Response를 받아야한다.

![img](https://joshua1988.github.io/images/posts/web/http/request-response.png)

### HTTP 1.1

#### 단점

- HOLB(Head Of Line Blocking) 특정 응답 지연
- RTT(Round Trip Time)증가
- heavy header

### HTTP 2.0

HTTP1.1의 프로토콜을 계승해 동일한 API면서 성능 향상에 초점을 맞췄다.

### IDL(Interface Definition Language)

#### XML

XML은 어떠한 데이터를 설명하기 위해 이름을 임의로 지은 태그로 데이터를 감싸며, 태그로 사용자가 직접 태그로 사용자가 직접 데이터 구조를 정의할 수 있다.

HTTP + XML 조합으로 많이 사용된다.

#### JSON

XML이 가진 읽기 불편하고, 복잡하고, 느린 속도 문제를 해결했다. 특히 key-value로 정의된 구조 자체가 굉장히 사람에게 직관적임.

HTTP + RESTful API + JSON 조합으로 많이 사용됨.

#### Protocol buffers(proto)

XML의 문제점을 개선하기 위해 제안된 IDL이며, XML보다 월등한 성능을 지닌다.

Protocol buffers는 구조화된 데이터를 직렬화(serialization)하기 위한 프로토콜로 XML보다 작고, 빠르고, 간단하다. XML 스키마처럼 .proto 파일에 protocol buffer 메세지 타입을 정의한다.

Protocol buffers는 구조화된 데이터를 직렬화하는데 있어서 XML보다 많은 장점들을 가지고 있다.

#### 장점

- 간단하다
- 파일 크기가 3~10배 정도 작다
- 속도가 20~100배 정도 빠르다
- XML보다 가독성이 좋고 명시적이다.

## gRPC란

gRPC는 구글에서 만든 RPC 플랫폼이며 protocol buffer와 RPC를 사용한다.

최신 버전의 IDL로 proto3를 사용한다. Java, C++, Python, Java Lite, Ruby, JavaScript, Objective-C 및 C#에서 사용 가능하다.

SSL/TLS를 사용하여 서버를 인증하고 클라이언트와 서버간에 교환되는 모든 데이터를 암호화한다. HTTP 2.0을 사용하여 성능이 뛰어나고 확장 가능한 API를 지원한다.

gRPC에서 클라이언트 응용 프로그램을 서버에서 함수를 바로 호출 할 수 있어 분산 MSA를 쉽게 구현할 수 있다. 서버 측에서는 서버 인터페이스를 구현하고 gRPC 서버를 실행하여 클라이언트 호출을 처리한다.

![img](http://thenewstack.io/wp-content/uploads/2016/09/gRPC-1.png)

## Protocol Buffers(proto3)

Protocol buffer는 구조화 데이터(structured data)를 직렬화(seriailize)하기위한 Google의 언어 중립적, 플랫폼 중립적이며 확장 가능한 메커니즘이다.

XML보다 작지만, XML보다 빠르고 단순하다.

IDL(Interface Definition Language)로서 data structure를 정의한 다음, .proto 파일을 protocol buffer compiler(protoc)를 이용해 compile 합니다. Complie된 소스 코드를 사용하여 다양한 데이터스트림에서 다양한 언어로 다양한 구조의 데이터를 쉽게 읽고 쓸 수 있다.

프로토콜 버퍼는 현재 Java, Python Objective-C 및 C++에서 생성된 코드를 지원한다. 새로운 proto3버전을 사용하면 proto2에 비해 더 많은 언어를 사용할 수 있다.

## Proto3 Guide

Protocol buffers는 크게 proto2와 proto3가 있지만 gRPC에선 최신 버전인 proto3를 사용한다.

### Defining 메세지 type

```proto
syntax = "proto3"; //syntax가 없으면 자동으로 proto2로 인식됨

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

SearchRequest 메시지 정의는 이 유형의 메시지에 포함하려는 각 데이터에 대해 하나씩 세 개의 필드(키, 값)를 지정한다. 각 필드에는 키와 값이 있다.

### Specifying Field Types(필드 유형 지정)

위의 예시에서 나온 Field types는 모두 scalar types이다. (int32, string)

하지만 scalar type 뿐만 아니라 enum과 같은 다른 field type도 선언 가능하다.

### Assigning Field Numbers(필드 번호 할당)

각 field는 unique number를 갖는다.

이 field number는 메세지 binary format에서 field를 식별하는데 사용된다. 즉 field number가 key가 되고 field type이 value가 되는 해시 구조를 지닌다.

Field numbers의 값이 1~15 사이면 1 byte로 encoding된다.

Field numbers의 값이 16~2047이면 2 bytes로 encoding된다.

그렇기에 Field numbers의 값을 1~15 사이로 encoding 하는것이 효율적이다.

### Specifying Field Rules(필드 규칙 지정)

메세지 fields는 다음 중 한 가지를 따른다.

1. singular: well-formed된 메세지는 하나 이하의 값을 가질 수 있다.
2. repeated: 이 field는 여러번 반복될 수 있다.

### Adding Message Type

하나의 .proto 파일 안에 다중 메세지 type이 정의될 수 있다.

```protobuf
message SearchRequest {
	string query = 1;
	int32 page_number = 2;
	int32 result_per_page = 3;
}

message SearchResponse {
	...
}
```

### .proto가 컴파일돼서 생성되는 것

.proto 파일을 protocol buffer compiler에 의해 compiled되면, 메세지는 serializing되어 output stream으로 나가고 parsing되어 input stream으로 들어온다.

#### C ++

컴파일러는 파일에 설명 된 각 메세지 type에 대한 class와 함께 각 .proto에서 .h 및 .cc파일을 생성한다.

#### Java

컴파일러는 각 메세지 type에 대한 class와 메세지 class instance 작성을 위한 builder class가 있는 .java 파일을 생성한다.

#### Python

컴파일러는 각 메세지 type의 static discriptor가 있는 모듈을 생성한 다음, meta class와 함께 런타임에 필요한 Python data access class를 작성하는 데 사용된다.

