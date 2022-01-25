# Domain 레이어
도메인 레이어는 UI 레이어와 데이터 레이어 사이에 있는 **선택적 레이어**입니다.

<div align="center">
<img src = "https://user-images.githubusercontent.com/48902047/150910648-247c470f-c5ca-48e8-881e-885b1b296e43.png" width="50%" height="50%">
</div>

도메인 레이어는 복잡한 비즈니스 로직이나 여러 ViewModel에서 **재사용되는 간단한 비즈니스 로직의 캡슐화**를 담당합니다. 모든 앱에 이러한 요구사항이 있는 것은 아니므로 이 레이어는 선택사항입니다. 따라서 복잡성을 처리하거나 재사용성을 선호하는 등 필요한 경우에만 도메인 레이어를 사용해야 합니다.

도메인 레이어는 다음과 같은 이점을 제공합니다.

+ 코드 중복을 방지합니다.
+ 도메인 레이어 클래스를 사용하는 클래스의 가독성을 개선합니다.
+ 앱의 테스트 가능성을 높입니다.
+ 책임을 분할하여 대형 클래스를 방지합니다.
이러한 클래스를 간단하고 가볍게 유지하려면 각 사용 사례에서는 **기능 하나만 담당해야 하고 변경 가능한 데이터를 포함해서는 안 됩니다.** 대신 개발자가 UI 레이어 또는 데이터 레이어의 변경 가능한 데이터를 처리해야 합니다.

## 이 가이드의 이름 지정 규칙
이 가이드에서 사용 사례의 이름은 각각 담당하고 있는 단일 작업에 따라 지정됩니다. 규칙은 다음과 같습니다.

+ **현재 시제의 동사 + 명사/대상(선택사항) + UseCase.**

## 종속 항목
일반적인 앱 아키텍처에서 사용 사례 클래스는 UI 레이어의 ViewModel과 데이터 레이어의 저장소 사이에 적합합니다. 즉, 사용 사례 클래스는 일반적으로 저장소 클래스에 종속되며, 저장소와 동일한 방법으로 콜백(자바의 경우) 또는 코루틴(Kotlin의 경우)을 사용하여 UI 레이어와 통신합니다. 

예를 들어 뉴스 저장소의 데이터와 작성자 저장소의 데이터를 가져와서 이를 결합하는 사용 사례 클래스가 앱에 있을 수 있습니다.
 ```Kotlin
class GetLatestNewsWithAuthorsUseCase(
  private val newsRepository: NewsRepository,
  private val authorsRepository: AuthorsRepository
) { /* ... */ }
```
사용 사례는 재사용 가능한 로직을 포함하기 때문에 다른 사용 사례에 의해 사용될 수도 있습니다. 도메인 레이어에 여러 수준의 사용 사례가 있는 것은 정상입니다. 예를 들어 아래 예에 정의된 사용 사례는 UI 레이어의 여러 클래스가 시간대를 사용하여 화면에 적절한 메시지를 표시하는 경우 FormatDateUseCase 사용 사례를 사용할 수 있습니다.

또한 invoke() 함수를 정의하여 호출 메소드를 재정의 하는 함수도 만들 수도 있습니다.

 ```Kotlin
class GetLatestNewsWithAuthorsUseCase(
  private val newsRepository: NewsRepository,
  private val authorsRepository: AuthorsRepository,
  private val formatDateUseCase: FormatDateUseCase
) {
    operator fun invoke(date: Date): String {
        return formatter.format(date)
    }
}
```

<div align="center">
<img src = "https://user-images.githubusercontent.com/48902047/150912103-142a43a3-59b1-492a-a5eb-dc4f8c74ccca.png" width="50%" height="50%">
</div>













