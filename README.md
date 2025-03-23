# Docker Study
> Docker 학습 및 실습코드

<h2>🛠 사용 기술</h2>
<div align="left">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=Apache&logoColor=white" alt="Apache" />
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL" />
  <img src="https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java" />
  <img src="https://img.shields.io/badge/Spring Boot-6DB33F?style=for-the-badge&logo=Spring Boot&logoColor=white" alt="Spring Boot">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5" />
  <img src="https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white" alt="YAML" />
  <img src="https://img.shields.io/badge/Docker Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker Compose" />
</div>

## 예제 구성

| 예제   | 내용                                 | 사용 기술                      |
|--------|--------------------------------------|-------------------------------|
| **ex1** | Apache 웹 서버 컨테이너 구축         | Docker, Apache, HTML         |
| **ex2** | Java 애플리케이션 컨테이너 구축      | Docker, Java, OpenJDK        |
| **ex3** | MySQL 데이터베이스 컨테이너 구축     | Docker Compose, MySQL        |
| **ex4** | Spring Boot + MySQL 연동 애플리케이션| Docker Compose, Spring Boot, MySQL |

## 예제 상세 설명

### ex1: Apache 웹 서버 컨테이너
<div align="center">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Apache-D22128?style=flat-square&logo=Apache&logoColor=white" alt="Apache" />
  <img src="https://img.shields.io/badge/HTML-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML" />
</div>

**핵심 개념**: Dockerfile, 이미지 빌드, 웹 서버 컨테이너

```dockerfile
FROM httpd
COPY ./index.html /usr/local/apache2/htdocs
```
- 공식 Apache 이미지를 기반으로 컨테이너 구축
- 정적 웹 페이지를 Apache 서버의 문서 디렉토리에 복사
- 간단한 HTML을 통해 컨테이너화된 웹 서버 구현

### ex2: Java 애플리케이션 컨테이너
<div align="center">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white" alt="Java" />
  <img src="https://img.shields.io/badge/OpenJDK-ED8B00?style=flat-square&logo=openjdk&logoColor=white" alt="OpenJDK" />
</div>

**핵심 개념**: 자바 애플리케이션 컨테이너화, ENTRYPOINT

```dockerfile
FROM openjdk:17
ARG JAR_FILE=./*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```
- OpenJDK 17 이미지를 기반으로 컨테이너 구축
- JAR 파일을 컨테이너 내부로 복사
- 컨테이너 시작 시 자바 애플리케이션 자동 실행

### ex3: MySQL 데이터베이스 컨테이너
<div align="center">
  <img src="https://img.shields.io/badge/Docker Compose-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker Compose" />
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" alt="MySQL" />
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
</div>

**핵심 개념**: 환경 변수, 포트 매핑, docker-compose

```yaml
version: "3.8"
services: 
    minhi_db:
        image: mysql
        container_name: minhi_db
        ports:
            - 3307:3306
        environment:
            MYSQL_ROOT_PASSWORD: 1234
            MYSQL_DATABASE: minhi
```
- Docker Compose를 이용한 MySQL 컨테이너 구성
- 환경 변수를 통한 데이터베이스 설정
- 포트 매핑을 통한 외부 접근 허용

### ex4: Spring Boot + MySQL 연동 애플리케이션
<div align="left">
  <img src="https://img.shields.io/badge/Docker Compose-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker Compose" />
  <img src="https://img.shields.io/badge/Spring Boot-6DB33F?style=flat-square&logo=Spring Boot&logoColor=white" alt="Spring Boot">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" alt="MySQL" />
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
</div>

**핵심 개념**: 다중 컨테이너 구성, 네트워크, 볼륨, 의존성 관리

```yaml
version: "3.8"
services:
  sboard_db:
    image: mysql
    container_name: sboard_db
    restart: always
    ports:
      - 3307:3306
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: root
      MYSQL_USER: root
      MYSQL_PASSWORD: 1234
    volumes:
      - ./db/mysql/data:/var/lib/mysql
    networks:
      - springboot-mysql-net
  
  sboard_app:
    build: .
    container_name: sboard_app
    restart: always
    ports:
      - 8080:8080
    depends_on:
      - sboard_db
    networks:
      - springboot-mysql-net

networks:
  springboot-mysql-net:
    driver: bridge
```
- Spring Boot 애플리케이션과 MySQL 데이터베이스의 다중 컨테이너 구성
- 컨테이너 간 통신을 위한 네트워크 설정
- 볼륨을 통한 데이터 영속성 확보
- 서비스 의존성 관리(depends_on)

## 실행 방법

<div align="center">
  <img src="https://img.shields.io/badge/CLI-4D4D4D?style=for-the-badge&logo=windows%20terminal&logoColor=white" alt="Command Line Interface" />
</div>

### ex1: Apache 웹 서버
```bash
cd ex1
docker build -t my-apache-app .
docker run -d -p 8080:80 my-apache-app
```
접속: http://localhost:8080

### ex2: Java 애플리케이션
```bash
cd ex2
docker build -t my-java-app .
docker run -d -p 8080:8080 my-java-app
```
접속: http://localhost:8080

### ex3: MySQL 데이터베이스
```bash
cd ex3
docker-compose up -d
```
접속: localhost:3307 (사용자: root, 비밀번호: 1234)

### ex4: Spring Boot + MySQL
```bash
cd ex4
docker-compose up -d
```
접속: http://localhost:8080 (애플리케이션)  
MySQL 접속: localhost:3307 (사용자: root, 비밀번호: 1234)

## Docker 기본 개념

<div align="center">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
</div>

| 개념 | 설명 |
|------|------|
| **Docker** | 애플리케이션을 컨테이너화하여 개발, 배포, 실행할 수 있는 플랫폼 |
| **Container** | 애플리케이션과 그 의존성을 포함하는 독립적인 실행 환경 |
| **Image** | 컨테이너를 생성하기 위한 읽기 전용 템플릿 |
| **Dockerfile** | 도커 이미지를 만들기 위한 설명서 |
| **Docker Compose** | 다중 컨테이너 애플리케이션을 정의하고 실행하는 도구 |
| **Volume** | 컨테이너와 호스트 간의 데이터 공유를 위한 저장 공간 |
| **Network** | 컨테이너 간 통신을 위한 가상 네트워크 |

## 자주 사용하는 Docker 명령어

<div align="center">
  <img src="https://img.shields.io/badge/Docker Commands-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker Commands" />
</div>

| 명령어 | 설명 |
|--------|------|
| **docker build** | 도커 이미지 빌드 |
| **docker run** | 도커 컨테이너 실행 |
| **docker ps** | 실행 중인 컨테이너 목록 조회 |
| **docker images** | 로컬에 저장된 이미지 목록 조회 |
| **docker pull** | 도커 허브에서 이미지 다운로드 |
| **docker logs** | 컨테이너 로그 확인 |
| **docker exec** | 실행 중인 컨테이너에 명령 실행 |
| **docker-compose up** | 컴포즈 파일로 정의된 서비스 시작 |
| **docker-compose down** | 컴포즈 파일로 정의된 서비스 중지 |

---
