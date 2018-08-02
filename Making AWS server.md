## EC2 , RDS 만들고 연결하는 과정이 잘 설명되어 있는 참고자료- 늦게 발견함
참고)https://www.linkedin.com/pulse/connecting-aws-rds-mysql-workbench-suresh-kumar

## EC2 인스턴스 만들기 
amazon llinux를 이용해서 ec2 instance 생성
amazon linux는 다른 인스턴스보다 메모리 2배 제공
amazon linux 2를 사용하면 httpd24, php56, mysqlnd 등… 의 패키지를 지원하지 않는 경우가 많으니 linux 2는 이용하지 말자

## DB 생성
mysql free tier로 database를 생성한다.
생성 시 vpc는 ec2를 만들 때 설정한 default 값으로 한다.
이외의 경우 vpc를 손수 만들어서 해야 한다.
tutorial로 vpc를 만들 수 있는 방법이 아마존 자습서에 등록되어 있다.
https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html    		
하지만 굳이 vpc를 만들지 않아도 default 값으로 만들어진 vpc를 사용하면 대부분의 서비스를 사용하는데 지장이 없다.
( 사실 지장이 있는 서비스가 어떤 것인지 모르겠다.)
생성 시 security group를 ec2에서 설정한 security group을 동일하게 해야 ec2 instance에서 접근이 가능하다.


## EC2 인스턴스에 서버 생성
https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateWebServer.html

*chown은 소유 권한을 바꾸는 것을 의미한다.					리눅스 기본 설명 https://conory.com/blog/19194

*chmod는 permission 권한을 바꾸는 것을 의미한다.				permission 권한에 대한 설명 추가(2775에 관한 설명) http://yaic.tistory.com/entry/chmod-%EC%A0%95%EB%A6%AC	

*find 명령어와 옵션								http://corej21.tistory.com/45
{} 의미 									https://stackoverflow.com/questions/447048/simple-unix-command-what-is-the-and-for
{} +              {} \;     의미						https://unix.stackexchange.com/questions/195939/what-is-meaning-of-in-finds-exec-command

## MySQL Workbench 로 RDS 접속하기
MySQL Workbench 주소
		https://dev.mysql.com/downloads/workbench/
RDS 접속 동영상						https://aws.amazon.com/ko/premiumsupport/knowledge-center/connect-rds-mysql-workbench/
Work bench 설치하려면 visual studio 필요 ( visual studio c++  설치 할것-work load에서)
접속하기 전 ec2 보안그룹에서 MYSQL 접속 허용 해야함.
=> Work Bench 로 접속 시 error 발생 - 원인 찾지 못함 

authentication that can continue publcik key 라고 나오는데 해결이 안된다.

putty gen에  private.ppk 를 load 해서 convert - > export opnSSH 해서 , openssh.ppk로 바꾸라는 답변들이 있었으나 2014, 2016 년도의 답변이라 2018년 현재 ver8 work bench에는 먹지 않는다.

따라서 visual studio를 필요로 하는 Workbench 를 포기하고 phpmyadmin으로 선회한다.



## phpmyadmin 설치 방법과 AWS RDS 연결 방법					https://linuxtogether.org/amazon-rds-mysql-instance-integration-with-phpmyadmin/

*mbstirng error 발생
해결									https://stackoverflow.com/questions/31190547/dependency-issue-trying-to-install-php-mbstring-on-ec2
sudo service httpd restart - 다시 시작 , package 새로 설치한 내역 반영

*ip/phpmyadmin 에서  아이디/비밀번호 입력했는데 로그인 안됨
ec2 인스턴스 보안 그룹에서 MYSQL 접근에 대한 인바운드를 열어주지 않아서 그름
인바운드 규칙에 추가( anywhere로 열어줘도 되고 내 ip만 열어줘도 되고)



===> 서버 완성 


요약

EC2 인스턴스 / RDS DB 인스턴스 두개를 따로 만들어서 연결
RDS DB는 phpmyadmin으로 접근 가능 ( Work Bench 쓰려다가 접속 에서 + visual studio 용량이 너무 큰)




추가로 해야 할일)
Elastic Ip 할당 받기 전에는 인스턴스 끄고 켜고 할 예정이기 때문에, ec2 인스턴스 활성화 되었을 때, 서버 자동으로 켜지게 하는 설정 하기 - 아마존 자습서에 나와있을 거임
