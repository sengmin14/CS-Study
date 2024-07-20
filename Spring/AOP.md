# 🥕 AOP
- AOP는 부가 기능을 핵심 기능에서 분리해 한 곳으로 관리하도록 하고, 이 부가 기능을 어디에 적용할지 선택하는 기능을 합한 하나의 모듈
- 애플리케이션을 바라보는 관점을 하나하나의 기능에서 횡단 관심사관점으로 달리 보는 것

## 🥕 핵심 기능과 부가 기능
- 애플리케이션 로직은 크게 핵심 기능과 부가 기능으로 나눌 수 있다.
- 핵심기능 : 해당 객체가 제공하는 고유의 기능
- 부가 기능 : 핵심 기능을 보조하기 위해 제공되는 기능(로그 추적 기능, 트랜잭션 기능)
- 프로젝트의 모든 클래스에 로그 기능을 추가 한다면 하나의 부가 기능(로그 추적)을 여러 곳에 동일하게 사용하게 된다.
- 이러한 부가 기능을 횡단 관심사라고 한다.

## AOP 용어 정리
### AOP 프록시
- AOP 기능을 구현하기 위해 만든 프록시 객체, 스프링에서 AOP 프록시는 JDK 동적 프록시 또는 CGLIB 프록시이다.
 
### 조인 포인트 ( Join Point )
- 어드바이스가 적용될 수 있는 위치로, AOP를 적용할 수 있는 모든 지점이라 생각하면 된다.
- 스프링 AOP는 프록시 방식을 사용하므로 조인 포인트는 항상 메소드 실행 지점으로 제한된다.
 
### 포인트컷 ( Pointcut )
- 조인 포인트 중에서 어드바이스(부가 기능)를 어디에 적용할 지, 적용하지 않을 지 위치를 판단하는 필터링하는 기능 ( 주로 AspectJ 표현식을 사용해서 지정 )
- 프록시를 사용하는 스프링 AOP는 메서드 실행 지점을 포인트 컷으로 필터링 한다.
 
### 타겟 ( Target )
- 어드바이스를 받는 객체, 포인트컷으로 결정
 
### 어드바이스 ( Advice )
- 부가 기능
- 특정 조인 포인트에서 Aspect에 의해 취해지는 조치
- Around(주변), Before(전), After(후)와 같은 다양한 종류의 어드바이스가 있음
 
### 애스펙트( Aspect )
- 어드바이스 + 포인트컷을 모듈화 한 것
- 하나의 어드바이스만이 아닌 여러 어드바이스와 포인트 컷이 함께 존재할 수 있다.
 
### 어드바이저 ( Advisor )
- 하나의 어드바이스와 하나의 포인트 컷으로 구성
- 즉, 어드바이스 + 포인트 컷 = 어드바이저

## AOP 적용 방식
### AOP의 적용 방식은 크게 3가지가 있다.
- 컴파일 시점
- 클래스 로딩 시점
- 런타임 시점 ( 프록시 사용 )

```
컴파일 시점과 클래스 로딩 시점 적용 방식은 AspectJ 프레임워크를 직접 사용해야 한다
이 AspectJ를 학습하기 위해선 엄청난 분량과 설정의 번거로움이 있어
주로 런타임 시점 적용 방식을 사용하는 스프링 AOP를 사용 한다.
```
![image](https://github.com/user-attachments/assets/f565e398-fab3-44cd-9613-9a3c8972b66a)

- 런타임 시점은 컴파일도 다 끝나고, 클래스 로더에 클래스도 다 올라가서 이미 자바가 실행되고 난 다음을 말한다.
( 자바의 메인( Main ) 메서드가 실행된 다음)
- 프록시를 사용하기 때문에 AOP 기능에 일부 제약이 있지만, 특별한 컴파일러나, 자바를 실행할 때 복잡한 옵션과 클래스 로더 조작기를 설정하지 않아도 스프링이 알아서 자동으로 설정 해주기 때문에 훨씬 편리하다.

### AOP 적용 가능 위치
- 스프링 AOP는 메서드 실행 지점에만 AOP를 적용할 수 있다.
- 프록시 방식을 사용하는 스프링 AOP는 스프링 컨테이너에 해당 @Aspect을 빈 등록을 해야 AOP를 적용할 수 있다.
