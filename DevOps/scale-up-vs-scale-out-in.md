# Scale Up vs Scale Out & In

서버 확장을 위한 두 가지 방법

![scale-up-out](https://imgur.com/8HeIBHj.png)

<br>

## Scale Up

- **수직적 확장**
- 성능 또는 용량을 늘리기 위해 서버의 디스크를 추가하거나 CPU나 메모리를 업그레이드 시키는 것을 의미한다.
- 한 대의 서버에서 모든 데이터를 처리하므로 데이터 갱신이 빈번하게 일어나는 '데이터베이스 서버'에 적합한 방식이다.

<br>

## Scale Out

- **수평적 확장**
- 서버를 여러 대 추가하여 시스템을 확장하는 방법
- 서버 한 대에 장애가 발생해도 다른 서버로 서비스 지원이 가능하다.
- 각 서버에 걸리는 부하를 균등하게 해주는 '로드 밸런싱'이 필수적으로 동반되어야 한다.

### Scale In

- Scale Out으로 늘렸지만 작업이 완료되어 더 이상 필요없는 컴퓨팅 수를 줄이는 것

<br>

## 📚 Reference

- https://library.gabia.com/contents/infrahosting/1222/
- https://minemanemo.tistory.com/131
