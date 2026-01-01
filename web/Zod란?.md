# Zod란?

## Zod
Zod는 TypeScript 기반의 스키마 선언 및 데이터 검증 라이브러리로, 데이터 유효성 검증과 타입 안전성을 강화하기 위해 만들어졌다. Zod를 사용하면 TypeScript와 함께 타입 정의와 데이터 검증을 손쉽게 수행할 수 있다.

## TypeScript
TypeScript는 JavaScript의 슈퍼셋으로, JavaScript 코드에 정적 타입 문법을 추가하여 안정성과 개발 효율을 높인 언어이다.

## Zod를 사용하는 이유
Zod를 사용하는 이유는 TypeScript의 한계 때문이다.
TS는 컴파일 타임에만 작동하고 코드가 JS로 변환되어 실행되는 런타임에서는 TS의 모든 타입 정보가 사라진다. 이것을 타입 소거(Type Erasure)라고 한다.
따라서 TS는 정적 타입 시스템을 통해 개발 단계에서 수 많은 오류를 예방해 주지만 런타임 환경에서 외부에서 들어보는 데이터는 그 타입을 신뢰할 수 없다. 런타임에서 작동하는 것은 TS가 아닌 JS이기 때문

이런 문제를 해결하기 위해 직접 검사 코드를 짜야한다.
```
function validateUser(data: any): data is User {
  return (
    typeof data.id === 'number' &&
    typeof data.name === 'string'
  );
}

const data = await response.json();
if (validateUser(data)) {
  
} else {
  
}
```
이런식으로 typeof를 사용해 런타임에서도 타입 검증을 할 수는 있지만 이렇게 되면 inferface와 타입이 맞는지 검증하는 if문을 이중으로 만들어야한다.

하지만 Zod를 사용하면 inferface 선언과 런타임에서 타입 검증을 한번에 해결할수 있다.
Zod는 스키마(Schema)를 한 번만 선언하면
1. 런타임 유효성 검증 (parse, safeParse)
2. 정적 타입 추론 (z.infer)
이 두 가지를 동시에 해결해 준다. 즉 타입 안정성(Type-level satety)과 런타임 안전성(Runtime-level safety)을 하나의 스키마로 통합하여 관리 할수 있다.
타입 안정성이란 컴파일 단계에서 값의 타입이 올바르게 사용되도록 보장하는 것
런타임 안정성이란 프로그램 실행 중 데이터가 기대한 조건을 만족하는지 검증하는 것

### 근데 Zod 쓰거나 안쓰거나 서버에서 이상한 값 반환하면 런타임에서 서비스 멈추는거 아님??
만약 예외처리를 하지 않는다면 서버에서 이상한 값을 반환할떄 Zod 사용 여부와 상관없이 서비스가 멈출것이다. 
예외처리를 한다는 가정하에 TS만 사용한다면 예외처를 한다고 하여도 런타임 환경에서는 타입소거가 일어나기 떄문에 서버에서 이상한 값을 반환해도 에러가 나지않고 그 데이터를 사용하려고 할떄 예상하지 못한 에러가 발생하여 서비스가 멈출 것이다.
하지만 Zod를 사용한다면 서버에서 이상한 값을 반환하더라도 바로 유효성 검증을 통해 예외를 발생시켜 에러 핸들링을 할 수 있고 따라서 서비스가 멈추지 않을 것이다.

참고자료 <br />
https://zod.dev/ <br />
https://isaac-christian.tistory.com/entry/TypeScript-Zod-%ED%83%80%EC%9E%85-%EA%B2%80%EC%A6%9D-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC <br />
https://velog.io/@jinyoung985/TypeScript-zod-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EB%9E%80 <br />
https://velog.io/@anlee/Zod-%EC%99%84%EC%A0%84-%EC%A0%95%EB%B3%B5-%ED%83%80%EC%9E%85-%EC%95%88%EC%A0%84%EC%84%B1%EA%B3%BC-%EB%9F%B0%ED%83%80%EC%9E%84-%EA%B2%80%EC%A6%9D-%EB%91%90-%EB%A7%88%EB%A6%AC-%ED%86%A0%EB%81%BC%EB%A5%BC-%EC%9E%A1%EB%8A%94-%EB%B2%95