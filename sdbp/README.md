# Software Development Best Practices

개발의 모든 측면에서 좋은 습관과 관행을 다룹니다.


## REST API

REST API(Representational State Transfer Application Programming Interface)는 웹 서비스 설계 스타일 중 하나로, **REST(Representational State Transfer)** 아키텍처 원칙을 따르는 API를 의미합니다. REST는 HTTP를 기반으로 클라이언트와 서버 간의 상호작용을 간결하고 확장 가능하게 만드는 것을 목표로 합니다. REST API는 웹 서비스가 데이터와 기능을 제공하는 데 매우 널리 사용되는 방식이며, 클라이언트-서버 구조에서 중요한 역할을 합니다. 
method와 url로 응답을 예상할 수 있습니다.

### 생겨난 이유
API 자체가 개발자 가 직접 설계 하기에, 사람마다 회사마다 각자의 규격으로 설계하였음. 이로인해 API를 사용하기 위해선 API 문서가 필수적인 요소가 되었음. 2000년도 로이필드 박사가 실제 개발자 업무 비율을 보았을때 약 개발관련 업무 중 60% 가량이 API 문서 작성 또는 분석 이였음. 이에 REST 아키텍쳐를 고안하여 API 설계의 규격을 제시함.

### **1. REST의 기본 개념**

- **클라이언트-서버 구조**: REST는 클라이언트와 서버 간의 명확한 분리를 강조합니다. 클라이언트는 사용자 인터페이스와 같은 역할을 하며, 서버는 데이터와 비즈니스 로직을 제공합니다. 클라이언트는 요청을 보내고, 서버는 요청을 처리한 후 응답을 반환합니다.

- **무상태성(Statelessness)**: REST는 무상태(stateless) 아키텍처를 요구합니다. 즉, 각 요청은 독립적이며, 서버는 클라이언트의 이전 요청 상태를 유지하지 않습니다. 모든 필요한 정보는 각 요청에 포함되어야 합니다.

- **캐시 가능(Cacheable)**: 서버의 응답은 캐시될 수 있어야 합니다. 이를 통해 클라이언트는 서버에 요청을 반복적으로 보내는 것을 줄일 수 있으며, 성능이 향상됩니다. HTTP 헤더를 통해 응답이 캐시 가능 여부를 지정할 수 있습니다.

- **계층화 시스템(Layered System)**: REST 아키텍처에서는 시스템이 여러 계층으로 구성될 수 있습니다. 클라이언트는 중간 서버(예: 로드 밸런서, 프록시)를 통해 최종 서버와 상호작용할 수 있으며, 각 계층은 독립적으로 동작합니다.

- **인터페이스 일관성(Uniform Interface)**: REST는 클라이언트와 서버 간의 인터페이스가 일관되어야 한다는 것을 강조합니다. 이는 API 설계의 단순성과 일관성을 높입니다. 인터페이스 일관성의 중요한 원칙은 자원(Resource) 식별, 자원 표현, 자원 조작 등이 있습니다.

- **자원 기반(Representation of Resources)**: REST에서는 모든 것이 자원(Resource)으로 간주됩니다. 자원은 URI(Uniform Resource Identifier)를 통해 고유하게 식별됩니다. 예를 들어, `https://api.example.com/users/123`는 특정 사용자 자원을 식별하는 URI입니다.

### **2. HTTP 메서드와 REST**

REST API는 주로 HTTP 메서드를 사용하여 자원을 조작합니다. 각 메서드는 특정한 작업을 수행하도록 설계되어 있으며, RESTful API 설계에서 중요한 역할을 합니다.

- **GET**: 
  - **역할**: 자원을 조회합니다. 서버에서 데이터를 가져올 때 사용됩니다.
  - **안전성**: GET 요청은 데이터를 수정하지 않으며, 항상 안전합니다.
  - **멱등성**: 같은 GET 요청을 여러 번 보내도 서버의 상태는 변하지 않습니다.
  - **예시**: `GET /users/123` (ID가 123인 사용자 정보 조회)

- **POST**:
  - **역할**: 자원을 생성합니다. 클라이언트에서 서버로 데이터를 전송하여 새로운 자원을 생성할 때 사용됩니다.
  - **안전성**: POST 요청은 서버 상태를 변경할 수 있습니다.
  - **멱등성 아님**: 동일한 POST 요청을 여러 번 보내면, 여러 개의 자원이 생성될 수 있습니다.
  - **예시**: `POST /users` (새로운 사용자 생성)

- **PUT**:
  - **역할**: 자원을 수정하거나 생성합니다. 지정된 자원을 대체하거나, 존재하지 않는 경우 새로운 자원을 생성합니다.
  - **안전성**: PUT 요청은 서버 상태를 변경할 수 있습니다.
  - **멱등성**: 동일한 PUT 요청을 여러 번 보내도 서버의 상태는 동일하게 유지됩니다.
  - **예시**: `PUT /users/123` (ID가 123인 사용자의 정보 갱신 또는 생성)

- **DELETE**:
  - **역할**: 자원을 삭제합니다.
  - **안전성**: DELETE 요청은 서버 상태를 변경할 수 있습니다.
  - **멱등성**: 동일한 DELETE 요청을 여러 번 보내도 결과는 동일합니다. 자원이 이미 삭제되었거나 존재하지 않더라도 동일한 응답을 받을 수 있습니다.
  - **예시**: `DELETE /users/123` (ID가 123인 사용자 삭제)

- **PATCH**:
  - **역할**: 자원의 일부를 수정합니다. PUT과 달리, 전체 자원이 아닌 일부만 수정할 때 사용됩니다.
  - **멱등성**: PATCH 요청은 일부만 변경하기 때문에, 동일한 요청을 여러 번 보내도 같은 결과를 보장해야 합니다.
  - **예시**: `PATCH /users/123` (ID가 123인 사용자의 일부 속성 수정)

### **3. REST API의 구성 요소**

RESTful API는 자원, URI, HTTP 메서드 외에도 몇 가지 중요한 요소를 포함합니다.

- **URI(Uniform Resource Identifier)**:
  - URI는 자원을 식별하는 고유한 주소입니다. 일반적으로 RESTful API에서는 명사 형태로 자원을 표현합니다.
  - 예시: `https://api.example.com/users/123`, 여기서 `users`는 자원, `123`은 자원의 식별자입니다.
  - APP, 도메인 이름, api, json, v1, open 등등 구성 부가요소가 있습니다.

- **HTTP 상태 코드**:

- **헤더(Headers)**:
  - 요청과 응답에서 메타데이터를 전달하기 위해 사용됩니다.
  - 예시: `Content-Type`, `Authorization`, `Accept`, `Cache-Control`.

- **페이징 및 필터링**:
  - 대용량 데이터를 처리할 때, 클라이언트가 데이터를 적절히 요청하고 서버가 페이징 또는 필터링하여 데이터를 제공할 수 있도록 설계할 수 있습니다.
  - 예시: `GET /users?page=2&limit=50` (2번째 페이지의 50개 사용자 데이터 조회)

### **4. REST API의 장점과 단점**

#### **장점**:

- **단순성**: REST API는 HTTP 프로토콜을 기반으로 하므로, 웹 서비스 구현이 단순하고 직관적입니다.
- **확장성**: REST는 클라이언트와 서버 간의 결합도가 낮아, 독립적으로 확장할 수 있습니다.
- **플랫폼 독립성**: 다양한 플랫폼에서 사용할 수 있습니다. 클라이언트와 서버가 서로 다른 기술 스택을 사용해도 문제없이 통신할 수 있습니다.
- **캐싱**: HTTP 프로토콜의 캐싱 메커니즘을 활용하여, 클라이언트 측에서 응답을 캐싱하여 성능을 향상시킬 수 있습니다.

#### **단점**:

- **복잡한 데이터 처리**: REST는 단순한 요청-응답 패턴에 적합하지만, 복잡한 트랜잭션이나 다중 자원 조작에는 적합하지 않을 수 있습니다.
- **대규모 작업**: 대규모 데이터 처리나 대량의 데이터 전송 시에는 성능이 저하될 수 있습니다.
- **상태 없는 통신의 한계**: 모든 요청이 독립적이므로, 클라이언트의 상태를 유지해야 하는 애플리케이션에서는 추가적인 설계가 필요할 수 있습니다.

### 추가적으로
- **패스 파라미터와 쿼리파라미터** : 전자는 리소스를 고유하게 **(개별)** 식별하는데 사용됩니다. 리소스의 위치를 표현하는 경로 자체에 포함되며, 주로 특정 자원(예: 특정 사용자, 특정 주문)을 조회하거나 수정할 때 사용합니다. 후자는 리소스에 대한 조건이나 옵션을 지정하는 데 사용됩니다.**(복수)** 선택적이고 유연한 방식으로 자원에 대한 필터링, 검색, 정렬, 페이징 등을 처리합니다. 
- **버전** : 웹을 쓰다가 앱으로 넘어갈 때 기존 api를 버리게 될 경우 웹은 사용하지 못하게 되는 경우를 방지하기 위해 uri에 v1, v2를 기입합니다. 더불어 회사에서 요구한 api가 다른 회사에서는 다른 데이터를 요구할 경우 구별할 수 있습니다.

<br>
<br>


## ADR

**ADR**(Architectural Decision Record)은 소프트웨어 개발 과정에서 **중요한 아키텍처 결정**을 기록하고 관리하는 문서 형식입니다. ADR은 팀이 프로젝트나 시스템 설계에서 내린 주요 결정들을 문서화하여, 왜 특정한 기술적 선택이 이루어졌는지, 그리고 그 결정이 어떤 영향을 미치는지를 명확히 기록하는 데 사용됩니다.

### **ADR의 주요 목적**

1. **의사결정의 투명성**:

   - 프로젝트나 시스템 설계에서 중요한 아키텍처 결정들이 왜 내려졌는지, 그 결정이 어떤 배경과 맥락에서 이루어졌는지를 기록합니다. 이를 통해 팀 전체가 결정의 이유를 명확히 이해할 수 있으며, 새로운 팀원이 합류했을 때도 빠르게 프로젝트의 방향을 이해할 수 있습니다.

2. **결정의 이력 관리**:

   - ADR은 시간이 지나면서 아키텍처 결정이 어떻게 변해왔는지 추적할 수 있는 이력을 제공합니다. 이는 과거의 결정을 돌아보거나, 특정 결정이 왜 변경되었는지를 이해하는 데 유용합니다.

3. **지식 공유 및 협업 촉진**:
   - 팀 내의 지식 공유를 촉진하여, 아키텍처 설계에 대한 이해를 팀원 전체가 공유할 수 있도록 합니다. 이를 통해 협업을 강화하고, 더 나은 의사결정을 내릴 수 있는 환경을 조성합니다.

### **ADR의 구조**

ADR 문서의 구조는 비교적 간단하며, 일반적으로 다음과 같은 항목들로 구성됩니다:

1. **제목(Title)**:

   - 해당 결정의 간단한 제목. 예: "사용자 인증을 위한 OAuth2 채택"

2. **상태(Status)**:

   - 결정의 현재 상태. 예: "Proposed", "Accepted", "Deprecated".

3. **컨텍스트(Context)**:

   - 이 결정을 내리게 된 배경과 상황을 설명합니다. 문제의 정의, 프로젝트의 요구사항, 제한사항 등을 포함합니다.

4. **결정(Decision)**:

   - 실제로 내린 결정이 무엇인지 기록합니다. 예: "OAuth2를 사용자 인증 메커니즘으로 채택한다."

5. **근거(Rationale)**:

   - 왜 이 결정을 내렸는지에 대한 이유를 설명합니다. 대안들과 비교하여 이 결정이 어떻게 최적의 선택이 되었는지 설명합니다.

6. **결과(Consequences)**:
   - 이 결정이 시스템이나 프로젝트에 미치는 영향을 기록합니다. 긍정적인 영향뿐만 아니라, 잠재적인 단점이나 리스크도 함께 기술합니다.

### **ADR의 예시**

```markdown
# ADR 001: 사용자 인증을 위한 OAuth2 채택

## Status

Accepted

## Context

프로젝트에서 사용자 인증이 필요하며, 여러 애플리케이션 간에 인증 정보를 공유해야 합니다. 사용자의 로그인 경험을 간소화하고, 보안이 중요한 요소입니다.

## Decision

OAuth2를 사용자 인증 메커니즘으로 채택합니다.

## Rationale

- OAuth2는 업계 표준이며, 다양한 프로바이더(Google, Facebook 등)에서 지원합니다.
- 사용자 경험이 간소화되며, 개발팀이 인증 메커니즘을 직접 구현할 필요가 없습니다.
- 보안성이 높으며, 다양한 구현체가 존재해 쉽게 도입할 수 있습니다.

## Consequences

- OAuth2를 지원하는 라이브러리를 도입해야 하며, 인증 흐름을 이해하고 구현해야 합니다.
- 타사 서비스의 가용성에 의존하게 되므로, 해당 서비스가 중단되면 문제가 발생할 수 있습니다.
```

### **ADR의 사용 시점**

- **초기 설계 단계**: 시스템 설계의 초기 단계에서 중요한 기술적 결정을 내릴 때 ADR을 작성하여 기록합니다.
- **변경 관리**: 시스템의 중요한 변경이 발생할 때마다, 기존 ADR을 업데이트하거나 새로운 ADR을 작성하여 변경 사항을 기록합니다.
- **지속적 기록**: 프로젝트의 전체 수명 주기 동안 주요 결정을 지속적으로 기록하여, 시스템의 아키텍처가 어떻게 발전했는지를 추적할 수 있습니다.

### **ADR의 장점**

- **명확한 문서화**: 중요한 결정을 문서화함으로써, 결정을 추적하고 이해할 수 있게 합니다.
- **협업 도구**: 팀 내에서 더 나은 의사결정을 내릴 수 있도록 협업을 촉진합니다.
- **프로젝트 지속성**: 새로운 팀원이 합류하거나 시간이 지나도 프로젝트의 아키텍처 결정과 방향성을 쉽게 이해할 수 있습니다.

### **ADR 도입 고려사항**

ADR은 모든 프로젝트에 필수적인 것은 아니며, 복잡한 시스템이나 장기적으로 유지보수해야 하는 프로젝트에서 특히 유용합니다. 간단한 프로젝트에서는 ADR을 간단하게 유지하거나 생략할 수도 있습니다. 중요한 것은 ADR이 팀의 의사결정 과정에 도움이 되고, 시스템의 아키텍처를 체계적으로 관리하는 데 유용하게 활용되어야 한다는 점입니다.

이와 같은 내용을 바탕으로 ADR을 도입하고 관리하면, 프로젝트의 아키텍처 결정이 더 명확하고 투명하게 이루어질 수 있습니다.

<br>
<br>

https://yozm.wishket.com/magazine/detail/2743/?utm_source=slack&utm_medium=bot&utm_campaign=T04VBKA4L4Q
