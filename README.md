# 채팅창 코드 이해를 위한 자바스크립트 및 노드JS 설명 #

### ★★★ 여기 repository에 있는 파일만으로는 실행 안 된대요. ★★★ ###
### ★★★  .idea폴더(숨김파일)이랑 node_module파일이 빠져있어요 ★★★ ###
### ★★★  따로 IDE 켜서 직접 추가하셔야 하지 않나 싶어요 ★★★ ###
### =========================================================== ###
### app.js : '서버'의 코드 작성 ###
### index.js : '클라이언트'가 웹 접속 하면 실행할 코드 작성 ###
### index.html : '클라이언트'한테 보여줄 웹 문서 ###

##### 서버는 명령 프롬프트에서 프로젝트 폴더로 이동 후 "node app.js" 라고 입력하면 코드가 실행되면서 서버가 열리게 됨 #####
##### (인텔리제이처럼 다른 IDE에서도 가능할 텐데 난 신문물 못 받아들여서 그냥 저렇게 했더니 되더라구) ###### 
##### 클라이언트는 웹 브라우저에 "http://(서버 IP주소):(port 번호)" 를 입력하면서 웹 서버에 접속하게 됨 #####
##### 이때 기본적으로 index.html이 제시되고 html 내부에 "<script src ~~>" 코드가 들어감으로써 index.js로 넘어가게 되어 해당 자바스크립트 코드가 실행됨 #####

##### 노드JS도 java가 JVM위에서 실행되는 것처럼 똑같이 NVM 위에서 실행 된다고 함 #####
##### 세부적으로 자세한 것은 잘 모르겟어여 ㅜㅜㅜ #####

##### + 윈도우 명령 프롬프트 명령어 #####
##### cd : 리눅스랑 똑같이 폴더 이동함 #####
##### dir : 이건 리눅스로 따지면 'll'. 해당 폴더 내부의 전체 파일을 보여줌 #####
##### ipconfig : 내 아이피를 알 수 있음 #####

# 목차 #
##### 1) [intro] #####
##### 2) [app.js 코드 설명 1] #####
##### 3) [app.js 코드 설명 2] #####
##### 4) [app.js 코드 설명 3] #####
##### 5) [app.js 코드 설명 4] #####
##### 6) [index.html 코드 설명] #####
##### 7) [index.js 코드 설명 1] #####

# 1) [intro] #
##### 우리 학교 컴공 2-2 막 마친, 아무것도 모르는 내 입장에서 이하 작성하겠음. #####
##### 내가 몰라요 ㅜㅜ 그니까 틀린거 있으면 지적 좀 #####
##### 님들 스프링에서도 마찬가지였겠지만 학교 과제의 코딩류와 다른 점이 분명 있음. #####

## 1. 학교 과제의 경우 ##
##### 오류발생 -> main문 첫 문장부터 코드 흐름에 따라 한줄한줄 printf찍기 -> 오류나는 부분 찾기 -> 해결하기 #####
##### 대부분 과제가 오류나면 이 루틴으로 진행했을 거임. #####
##### 저게 가능했던건 코드 진행 흐름이 한 줄이였기 때문. #####
##### 그니까, 그냥 코드가 일방적으로 쭉 진행되고 나면 다 끝났음. #####
##### (중간에 멈추는 경우라 해봤자, scanf와 같이 입력 받는 경우. 그러나 이 경우도 사용자가 엔터를 누르지 않는 이상 코드 다음 줄로 넘어가지 않았음.) #####

## 2. 노드JS 코딩의 경우 ##
##### 그러나, '통신'이라는 특성에 맞게 일방적으로 진행될 수 없음. #####
##### 노드js 특징 이기도 하고, 채팅 구현의 특징 이기도 하다보니 이러한 특성이 반영됨. #####
##### 특정 조건이 발생 -> 이하 코드 실행. 노드JS 코드는 거의 다 이 패턴이야. #####

## 이거 쫌 쉽게 감 잡기 위해 비슷한 경우가 수업 때 있었는데 함 설명해보겠음 ##
##### 권건우 교수님의 '어셈블리어' 수업 중 '입출력' 부분 다룰 때 #####
##### "CPU가 I/O장치로부터 data를 받아오는데 방법이 3개가 있다." #####
##### "Programmed I/O", "Interrupt",  "DMA방식" #####
##### " 여기서 3개는 CPU처리 속도와 I/O장치가 data 받아오는 속도는 차이가 있기에 이에 따른 해결 방식을 다르게 한 것 " #####
##### Programmed I/O : CPU가 상태 레지스터 ready인지 계속 확인 -> CPU 다른 일 처리 못함. (Busy Wating 문제) #####
##### Interrupt : 입출력장치가 알아서 받은 다음 ready되면 CPU한테 Interrupt Signal을 보냄 -> CPU가 해당 Signal 받으면 기존에 하던건 잠깐 멈추고 입출력 관련 처리(ISR)함. #####

##### 여기 다들 기억나지?? #####

##### 그때 첫번째 방식은 어셈 코드 예시가 ppt에 있었고 두번째 방식부터는 그냥 이론으로만 설명해주셔서 뭐 이걸 어셈 코드로는 어쩌라는거지 싶었는데 #####
##### 이거 만들면서 느낀게 노드js가 두번째 방식이랑 비슷해 #####
#####  ??? : " 아니 싯팔 그게 뭔말인데 ㅡㅡ " #####

### 노드JS는 "무슨 상황 발생 -> 해당 상황에 맞는 코드 실행." 대부분 이거야. ### 
##### 결론적으로, "무슨 상황이 닥치면 그거에 맞는 코드를 실행한다" 이게 노드JS의 가장 큰 특징 인거 같아. #####

##### 노드JS에서는 "상황"을 "이벤트"로 이름을 붙였더라고. #####
##### "이벤트 수신 -> 콜백함수 실행." ######
##### 거의 웬만한 코드가 이런식이야... #####
##### ??? : " 아니 그러면! 콜백함수는 뭔데 씹덕아?? " #####
##### 일단 아래부터 코드 설명하면서 진행해볼게 (콜백함수 관련 부분은 [app.js 코드 설명 2] 부터) #####

### [app.js 코드설명 2] 부터 ㄱㄱ ### 

# 2) [app.js 코드 설명 1] #
<pre>
<code>
  const express = require('express');         // express 모듈 불러오기 그리고 객체 반환. 객체 이름은 express
  const http = require('http');                // http 모듈 불러오기 그리고 객체 반환. 객체 이름은 http
  
  const app = express();
  const server = http.createServer(app);
  
  const fs = require('fs');                  // fs 모듈 불러오기 그리고 객체 반환. 객체 이름은 fs
  const io = require('socket.io')(server);   // socket.io 모듈 불러오기 그리고 객체 반환. 객체 이름은 io
  
  app.use(express.static('src'));
</code>
</pre>

##### 일단,, 위에서 4개로 구분 할 수 있을 거 같고  #####

### 첫번째와 세번째 ###
##### 여기 보면 const (~~~) = require ('~~~') 꼴로 되어있어  #####
##### C언어에 비유하면 "#include <~~~>" 이거나 java로 따지면 "import ~~" 랑 같다고 보면될듯. #####
##### 노드JS의 모듈을 불러 오는 것 #####
##### 그리고 그 모듈에서 이미 만들어놓은 규칙에 맞게 내가 메소드를 사용하는 것 #####
##### 여기서는 특이하게 외부 모듈을 불러왔으면 그에 대한 객체를 반환할 수 있다는 게 기존 C/C++/java 봐왔던 코드랑 다르지 #####
##### 이 반환된 객체로 내가 이제부터 만들어가는 것이야.. #####

##### 첫번쨰, 세번째 관련 정리 #####
#### <사용된 모듈> ####
##### 1. express 모듈 (서버 여는 거 관련 메소드 정의되어있음) #####
##### 2. http 모듈 (http 통신 관련 메소드 정의되어있음) #####
##### 3. fs 모듈 (파일 읽기/쓰기 관련 메소드 정의 되어있음) #####
##### 4. socket.io 모듈 (socket통신 관련 메소드 정의 되어있음) #####

### 두번째 ###
##### 객체(java로 따지면 인스턴스) 생성하는거. #####

### 마지막 ####
##### 마지막 부분은 app객체 use 함수실행 ㄱㄱ 인 내용. #####

### + 그리고 여기서 자바 스크립트 특징 ###
### 1. 변수, 상수 선언 시 자료형을 int, char, bool(boolean), 객체 이렇게 구분해서 선언하지 않음 ###
##### 그냥 변수인지 상수인지만 구분해주면 된대, 각각 키워드 이름은 var, const 이것이구요 #####
##### 그니까 " 'var'키워드로 Boolean, Number, String, null, Undefined, Object를 다 표현할 수 있다" 라고 교재에 이렇게 써있었어 #####
##### 인텔리제이에서 var이라고 썼더니 빨간줄 뜨길래 불편해서 const로 썼는데 머 실행은 똑같이 되나봐 #####

# 3) [app.js 코드 설명 2] #

	app.get('/', function(req, res) {
	    fs.readFile('./src/index.html', (err, data)=> {
	        if(err) throw err;
	        res.writeHead(200, {    // 파일 리드에 성공하면 html의 파일 내용이 data에 전송됨.
	                'Content-Type' : 'text/html'
	        }).write(data).end();
	    });
	});


##### 자 저기서 "app객체의 get함수를 실행한다." (app은 [코드 설명 1]에서 express()를 호출하면서 생긴 서버 객체) #####

##### 이건 보일거고..  안에 첫번째 매개변수를 보면 '/' 이거있고, 또 요상하게 생긴게 두번째 매개변수에 있음. #####
##### 괄호들 잘 구분해서 보면 그냥 함수를 구현해놓은게 매개변수자리에 길게 있어. #####
##### 그냥 단적으로 얘기하면, 매개변수 자리에 함수를 구현해놓은 저런 함수 종류를 통칭해서 '콜백함수'라고 한대 #####
##### (원래 '콜백함수' 정의는 이게 아닐텐데 그냥 여기서 용어랑 코드 쉽게 알아보기 위해서 이렇게 설명했음. 정확한 정의는 나도 잘... 정확한건 구글 검색 ㄱㄱ) #####

### app.get() 역할 : " 서버에 '/'의 path로 어떤 요청이 들어오면 두번째 매개변수의 함수(콜백함수)를 실행한다 " ###

##### 여기서 "서버에 path로 어떤 요청이 들어온다" >>> 이게 이벤트 수신(어떤 상황 발생) 그리고 "콜백함수 실행" 위에서 말한 노드JS 특징 그대로 맞죠 ? #####

##### 자, 이제 코드 안에를 보면 fs.readFile([path와 파일명], 콜백함수) 형식으로 또 있다..  #####
##### ??? : " 장난하나 두번째 매개변수 자리에 콜백함수라는건 왜 또 저렇게 생겼는데? " #####
##### 일단 코드 구현하신 분이 익명함수로 구현을 해놓으셨더라고,,  #####
### fs.readFile() 역할 :  "[파일경로/파일명]"의 파일을 *비동기식으로 읽어들이고 다 읽어들였으면 두번째 매개변수의 콜백함수를 실행한다" ###

	*비동기식 관련 설명
	비동기 이건 그냥 다 거르고 디폴트로 비동기 ㄱㄱ. 
	동기식으로 읽으면 파일 읽는 동안 다른 코드 수행을 못함. (위에 Programmed I/O 예시처럼)
	비동기식은 파일 읽으면서 동시에 다른거 할 수 있음. (정확히 표현하면 "요청만 하고 그다음 작업을 바로 수행")

##### 여기서도 이벤트 수신(상황 발생) 그리고 콜백함수 실행 이렇게 되어있지??  #####

##### : '파일 입출력'이니까 읽으면서 '오류'가 났는지 확인하는 역할해주고.. #####
##### data 쏴주는 역할 합니다. #####

여기 더 자세하게 서술해야하는데 힘들어서 그 다음 코드 먼저 설명 작성할래.

# 4) [app.js 코드 설명 3] #

	io.sockets.on('connection', function(socket) {      // 이벤트 이름 : 'connection'
	
	    socket.on('newUserConnect', function(name) {      // 이벤트 이름 : 'newUserConnect'
	        socket.name=name;
	        io.sockets.emit('updateMessage', {                      // 이벤트 이름 : updateMessage'
	            name : 'SERVER',
	            message : name + '님이 접속했습니다.'
	        });
	    });
	
	    socket.on('disconnect', function() {              // 이벤트 이름 : 'disconnect'
	        io.sockets.emit('updateMessage', {                     // 이벤트 이름 : 'updateMessage'
	            name : 'SERVER',
	            message : socket.name + '님이 퇴장했습니다.'
	        });
	    });
			
	    socket.on('sendMessage', function(data) {         // 이벤트 이름 : 'sendMessage'
	        data.name=socket.name;
	        io.sockets.emit('updateMessage', data);                  // 이벤트 이름 : 'updateMessage'
	    });
	});

소켓 관련 설명은 아예 다른 리드미에서 써보겠음.

##### 자 여기서도 보면 #####
### 전체 함수 역할 : "io.sockets가 'connection'이라는 이름의 이벤트를 받으면 function(socket) {} 를 실행하세요" ###
##### 내부에도 보면 3개 부분으로 이뤄져 있어 #####

### 내부의 역할 : 각각 'newUserConnect', 'disconnect', 'sendMessage'라는 이름의 이벤트를 받으면 이후 콜백함수를 실행하세요" ###

##### ??? : "아니 잠깐만 이벤트 안에 이벤트가 또 있어? 이건 뭐냐 " #####

##### 바깥에 'connection' 이벤트는 서버 열리면 자동으로 발생하는 이벤트고  #####
##### 일회성으로 이벤트를 쏘고 받으면 끝이 아니라 그냥 연결되어있으면 계속 connection 이벤트 수신 ㅇㅋ 상태에 있다고 보면 될듯 #####

##### 그런데?? 내부의 내부를 보면 또 "뭐시기랑 콜백함수" 꼴이 있음 #####
##### 여기서 이 글 위에서 노드js 대부분 "이벤트 수신 -> 콜백함수 실행" 이라고 했는데 #####
##### "수신"을 했으면 보낼 수도 있어야 하잖아? 내부의 내부에서 "emit()"은 이벤트를 만들고 쏘는 역할 하는거야 #####

##### "이벤트 발생 -> 콜백함수 실행" 여기서는 이렇게 되는거지 #####
##### emit =  방출하다??? 맞아떨어지지??  #####

##### 그러니까 io.sockets에서 emit() 메소드 : 이벤트 발생 #####
#####                          on() 메소드 : 이벤트 수신 #####

콜백함수 매개변수 socket은 이벤트에서 같이 넘어온 매개변수 입니다.

emit 은 이벤트를 '발생' 시키는 것입니다. 이벤트 발생 -> index.js 에서 이벤트 '수신'하는 코드가 있어요.




내부에도 보면
socket.on('~~', callbackFunction() { ~~ } ) 이해 되죵?

# 5) [app.js 코드 설명 4] #

	server.listen(8080, function() {
	    console.log('서버 실행중...');
	});
  
##### 자 이건 "server인스턴스에 listen()메소드를 실행한다"이건 보일텐데 #####
### 일단 함수 역할은  "서버 객체를 만들어서 특정포트에서 대기시킨다." ###
##### 요것인데,, 여기서는.. #####
##### 지금까지 내가 위에서 설명한 거 바탕으로 따지면 "8080이라는 이벤트를 받으면 이하 콜백함수를 실행한다" 이렇게 될텐데  #####
### 여기서는 그렇지 않다. ###
##### 일단 http 모듈의 메소드는 아래와 같음 #####
##### listen 메소드 : 서버를 실행하여 대기 시킨다. #####
##### close 메소드 : 서버를 종료한다 #####
##### 그러니까 서버를 8080포트에 대기시킨다. << 요걸 다 했으면 이하 콜백함수를 실행한다 #####
#####  "~~했으면 이하 콜백함수를 실행한다" 요것이야 #####  



# 6) [ index.html 코드 설명 ] #

<pre>
<code>

아니 HTML은 밑에 두줄 빼고 코드 삽입이 안돼요 ㅜㅜ

<script src="/socket.io/socket.io.js"></script>  <!-- 서버 실행 시 자동으로 실행 됨 -->
<script src="/js/index.js"></script>


</code>
</pre>

##### 근데 HTML은 님들이 더 잘 아실 것 같아요.. #####
##### 다른거 거르고 웹 브라우저가 index.html 제시하게 되면 #####
##### HTML 내용 다 제시하고 #####
##### 그 다음에 <script src="/js/index.js"></script> 이게 실행된다고 해요. #####
##### 그래서 (프로젝트 폴더)\js\index.js로 타고 넘어가서 해당 코드 실행 #####
##### 이렇게 되는 거죠 #####
##### 근데 윗줄에 <script src="/socket.io/socket.io.js"></script> 이거 있는데 주석에 써있는 거처럼 그냥 신경 안 써도 된대요 #####
##### socket.io 모듈 관련된 내용 이래요 #####
##### 요렇게 ~ #####


# 7) [index.js 코드 설명 1] #
<pre>
<code>
socket.on('connect', function() {
    var name = prompt('닉네임 입력');
    socket.emit('newUserConnect', name);    // 
});
</code>
</pre>

##### 여기 함수는 클라가 딱 "https://(서버 IP주소):(Port 번호)"로 접속한 순간 그 팝업 뜨는 그 부분임니다 #####
##### 이제 위에서 언급한대로 " 'connect'라는 이벤트를 받으면 이하 콜백함수를 실행하라 " 알 수 있죵 ?? ######
##### ??? : " 아니 근데 connect 이벤트는 니새끼가 발생시킨 적 없지 않느냐? " #####
##### connect 이벤트는 socket모듈에 이미 정의되어 있답니다. #####
##### 소켓이 연결되면 자동으로 connect 이벤트가 발생. #####
##### 그러면 이제 함수 내부를 보면 #####
##### prompt('~~')는  '~~~'를 보여주고 문자열 입력받는 자바스크립트의 기본 함수입니다 #####
##### 다음 줄은 newUserConnect 이벤트를 발생시켜라~ #####
##### 이 이벤트 받아서 처리하는 부분은 app.js에 있었쥬?? #####
##### 그리고 이벤트를 쏘는데 "name"을 포함시켜서 날아갑니다. #####
