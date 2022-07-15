# AWS Linux EC2에서 Express Framwork with EJS 실행하기

----

1. AWS EC2 Linux 인스턴스 생성

2. pemkey 발급

   1. terminal에서 pemkey가 존재하는 디렉터리로 이동
   2. pemkey파일에 대한 권한 설정 변경
      - Chmod 600 {pemKeyName}.pem
      - 600 - 나/그룹/전체를 의미하며 read(4), write(2), execute(1)의 조합으로 권한을 나타낸다.
        나에게 모든 권한을 허용하고 싶을때 4+2+1, 600으로 진행하면 된다.

3. SSH 클라이언트와 .pem 파일을 통해 터미널 환경에서 Linux 인스턴스 접속

   - ```shell
     ssh -i /path/my-key-pair.pem my-instance-user-name@my-instance-public-dns-name
     ```

   - ```shell
     ssh -i /path/my-key-pair.pem my-instance-user-name@my-instance-IPv6-address
     ```

     > https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html

4. Node, Npm 설치

   > https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html

5. 파일 전송 (로컬 -> Linux 인스턴스)

   - ```shell
     scp -i /path/my-key-pair.pem /path/my-file.txt ec2-user@my-instance-public-dns-name:path/
     ```

   - ```shell
     scp -i /path/my-key-pair.pem /path/my-file.txt ec2-user@\[my-instance-IPv6-address\]:path/
     ```

     > https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html

6. 파일 전송 (Linux 인스턴스 -> 로컬)

   - ```shell
     scp -i /path/my-key-pair.pem ec2-user@my-instance-public-dns-name:path/my-file.txt path/my-file2.txt
     ```

   - ```shell
     scp -i /path/my-key-pair.pem ec2-user@\[my-instance-IPv6-address\]:path/my-file.txt path/my-file2.txt
     ```

     >https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html

7. npm install  & npm start

   - 실행되는 port 번호를 확인하고 aws EC2 보안규칙(인바운드)을 살펴보고 열어준다.

8. 8번의 방법으로 실행시, exit나 인스턴스와 세션이 끊어지게 되면 같이 종료되는 것을 알 수 있다. 

   따라서, 세션이 종료되어도 실행 되어질 수 있길 원한다면 아래 명령어로 실행한다.

   ```shell
   nohup npm start &
   ```

   - nohup: No Hang Up의 약자로 세션과 연결을 종료하더라도 실행시킨 프로그램을 종료하지말것을 명시!
   - &: 백그라운드 실행을 의미한다.

9. 프로그램 종료

   1. 실행중인 Node.js 프로세스 확인

      - ```shell
        ps -ef | grep node
        ```

   2. 해당 id로 프로세스 종료

      - ```shell
        kill -9 {process_id}
        ```

        