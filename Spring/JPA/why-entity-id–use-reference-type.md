# Entity ID를 참조 타입으로 사용하는 이유

### 💡 Nullable

원시 타입은 기본 값이 정해져있는데(long: 0L, boolean: false, ...) 그게 실제 값인지, 아직 정해지지 않은 것인지 구분하기 어렵기 때문에 참조 타입을 주로 사용한다.

<br>

## 관련 용어

### 원시 타입(Primitive type)

- 실제 데이터 값을 저장

### 참조 타입(Reference type)

- 객체의 주소를 저장
- 원시 타입을 제외한 타입

### 원시 타입과 참조 타입 차이점

1. Null 사용 가능 여부
2. Generic 타입 사용 가능 여부

_Wrapper Class는 Primitive type을 객체화 한 것_

### Boxing

- 원시 타입을 Wrapper 클래스로 변환

### Unboxing

- Wrapper 클래스를 원시 타입으로 변환

<br>

## 📚 References

- https://www.inflearn.com/questions/35759/long-%ED%83%80%EC%9E%85%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A7%88%EB%AC%B8%EC%9E%85%EB%8B%88%EB%8B%A4
- https://velog.io/@combi_areum/JAVA-%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-vs-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85-and-Wrapper-Class
