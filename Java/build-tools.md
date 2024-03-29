# Java 빌드 도구

빌드 도구는 소스 코드에서 애플리케이션 생성을 자동화하기 위한 프로그램이다.

> **Build란?**
>
> 소스 코드를 컴파일, 테스트, 정적 분석, 패키징 등을 하여 실행 가능한 애플리케이션으로 만들어주는 과정

<br>

### 사용 이유

프로젝트가 진행될수록 종속된 라이브러리는 쌓여가고, 소스 코드를 직접 컴파일, 테스트하는 과정도 점점 피로해진다.

빌드 도구는 테스트, 라이브러리 자동 추가 및 버전 동기화 등 다양한 작업을 자동화하여 개발자의 짐을 덜어준다.

<br>

### 빌드 도구 역사

1. Make
   - 빌드 개념 확립
   - C언어로 제작되어 많은 제약이 있다.
2. Ant
   - 범용성 증가(크로스 플랫폼)
   - XML 사용
   - 공식적인 규약 X
3. Maven
   - XML 사용
   - 공식적인 규약 O
   - 라이브러리 자동 호출 & 관리
4. **Gradle**
   - 스크립트 언어 사용
   - 유연함
   - 가독성
   - 성능

<br>

## 참고

- https://tecoble.techcourse.co.kr/post/2020-09-17-java-build-tool/
