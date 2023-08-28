# **Socket 클래스**

#### *[UDPATE_HISTORY](#update-history)*
#### *마지막 빌드 : 2022.07.22, 배포자 : 김시홍*
<br/>
### **정의**

네임스페이스 : Socket_  
어셈블리 : Socket.dll

TCP Server, TCP Client, UDP 통신 구성을 위한 클래스입니다.  
싱글톤으로 구성되어 있어, 다음과 같은 방법으로 인스턴스 참조가 가능합니다.
<br/>

### **예제**
```C#
using Socket_;

class Sample()
{
    public static void Main()
    {
        // Socket 싱글톤 인스턴스 생성
        Socket instanceSocket = Socket.GetInstance();
        
        // Socket 사용준비
        instanceSocket.Init(true);

        /*
            소켓을 사용하는 작업
        */

        // Socket 종료
        instanceSocket.Exit();        
    }
}
```

## Shortcuts
Method, Delegate, Enum   
<br/><br/>

## **Method**
+ [GetInstance()](#socketgetinstance-메서드)
+ Init(bool hasInternalThreadForMonitoring = false)
+ Exit()
+ Execute()
+ Connect(int indexOfItem)
+ Disconnect(int indexOfItem)
+ Send
+ Receive
+ GetState(int indexOfItem)
<br/><br/>

## **Delegate**
+ public delegate void DelegateWritingSocketLog(string strID, PROTOCOL_TYPE enType, bool bSend, string strMessage);
<br/><br/>

## **Enum**
+ PROTOCOL_TYPE
+ LOG_TYPE
+ SOCKET_ITEM_STATE
+ PARAM_LIST
+ CONTROL_CHARACTER
<br/><br/>

* * *
## **Socket.GetInstance 메서드**

### **정의**

네임스페이스 : Socket_  
어셈블리 : Socket.dll

싱글톤 패턴으로 생성된 Socket 인스턴스를 반환합니다.
<br/>

```C#
public static Socket GetInstance();
```
<br/><br/>

## **Socket.Init(bool hasInternalThreadForMonitoring = false) 메서드**

### **정의**

네임스페이스 : Socket_  
어셈블리 : Socket.dll

매개변수에 따라 소켓 아이템의 상태를 관리하는 쓰레드를 생성하여 Socket 인스턴스의 사용을 준비합니다. 매개변수가 `false`인 경우는 수행하는 동작이 없고 항상 `true`를 반환합니다.
<br/>

```C#
public bool Init(bool hasInternalThreadForMonitoring = false);
```

### **매개변수**
`hasInternalThreadForMonitoring` **Boolean**  
Socket.dll 내부에 쓰레드를 생성하여 SocketItem의 상태를 관리하려면 `true`로 설정하고, dll 외부의 별도의 쓰레드에서 Excute 함수를 호출하여 SocketItem의 상태를 관리하려면 `false`로 설정합니다.   

### **반환**
**Boolean**  
Socket 인스턴스 초기화에 성공하면 `true`, 실패하면 `false`를 반환합니다.   
`hasInternalThreadForMonitoring == false` 인 경우에는 항상 true를 반환합니다.

<br/><br/>

## **Socket.Exit 메서드**

### **정의**

네임스페이스 : Socket_   
어셈블리 : Socket.dll

[Init](#socketinitbool-hasinternalthreadformonitoring--false-메서드) 메서드의 매개변수에 따라 생성된 쓰레드를 종료하고, Socket 인스턴스의 사용을 종료합니다.
```C#
public void Exit();
```
<br/><br/>

## **Socket.Execute 메서드**

### **정의**

네임스페이스 : Socket_   
어셈블리 : Socket.dll

현재 활성화된 TcpConnectionInformation 정보와 소켓 아이템의 내부 상태와 비교를 통해,
소켓 아이템 내부에 별도로 정의된 [상태](#socketcomunicatingstate-열거형)를 갱신하여, 대기 중인 소켓의 연결 요청, 연결 중인 소켓의 종료, 임의 중단된 소켓의 상태를 감시하여 종료 후 대기 등의 동작을 수행합니다.   
[Init](#socketinitbool-hasinternalthreadformonitoring--false-메서드) 메서드에 매개변수가 false가 전달된 경우, 사용자는 별도의 쓰레드에서 Execute 함수를 호출하여 소켓 아이템의 상태를 관리해주어야 합니다.

```C#
public void Execute();
```
<br/><br/>

## **Socket.Connect 메서드**

### **정의**

네임스페이스 : Socket_   
어셈블리 : Socket.dll

```C#
public bool Connect(int indexOfItem)
```

### **매개변수**
`indexOfItem` **Int**  
설정된 Socket Protocol 종류에 따라 연결대기/연결요청 동작을 시도합니다.
### **반환**
**Boolean**  
해당 동작의 성공이면 true, 실패하면 false를 반환합니다.
<br/><br/>



설정된 Socket Protocol 종류에 따라 연결대기/연결요청 동작을 시도합니다.









Socket 인스턴스 초기화에 성공하면 `true`, 실패하면 `false`를 반환합니다.   
`hasInternalThreadForMonitoring == false` 인 경우에는 항상 true를 반환합니다.



















* * *
## **SocketComunicatingState 열거형**

네임스페이스 : Socket_   
어셈블리 : Socket.dll

SocketItem의 상태 속성을 지정합니다.

```C#
internal enum SocketComunicatingState
```

상태 | 설명
|---| ---|
READY|TCP SERVER : BIND, LISTEN을 수행하기 전 초기상태입니다.<br>TCP CLIENT : SERVER에 CONNECT 요청하기 전 초기상태입니다.<br>UDP : 수신측 UDP와 연결하기 전 초기상태입니다.
TRY_CONNECT|TCP SERVER : 상태없음<br>TCP CLIENT : Connect 요청을 받아 Server 연결을 시도하고 있는 상태입니다.<br>UDP : Connect 요청을 받아 수신측 UDP와 연결을 시도하고 있는 상태입니다.
CONNECTED|TCP SERVER : Client와 연결이 완료된 상태입니다.<br>TCP CLIENT : Server와 연결이 완료된 상태입니다.<br>UDP : 수신측 UDP와 연결이 완료된 상태입니다.
WAITING_CONNECTION|TCP SERVER : Bind->Listen 이후 Client의 연결을 기다리는 상태입니다.<br>TCP CLIENT : Server에 연결을 시도한 후, 연결완료에 대한 콜백을 기다리는 상태입니다.<br>UDP : 상태없음
REQUEST_DISCONNECT|Disconnect 명령을 받아 현재의 연결을 종료합니다.<br>TCP SERVER : 현재 Bind 된 소켓 연결을 닫고, 해당 소켓을 다시 사용할 수 있도록 설정하고 연결된 리소스를 모두 해제합니다.<br>TCP CLIENT, UDP : 현재 소켓을 닫고 연결된 리소스를 모두 해제합니다.
<br/><br/>








* * *
## **UPDATE HISTORY**