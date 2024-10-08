# Frontend를 위한 레포

## SSR과 CSR

### 1. **서버사이드 렌더링 (Server-Side Rendering, SSR) - PHP 예시**

**서버사이드 렌더링**이란 웹 페이지의 HTML을 서버에서 생성한 후 클라이언트(브라우저)로 보내는 방식을 의미합니다. PHP는 대표적인 서버사이드 스크립트 언어로, 서버에서 HTML을 동적으로 생성하는 데 사용됩니다.

#### **PHP의 서버사이드 렌더링 과정**:

1. **클라이언트 요청**:

   - 사용자가 웹 브라우저에서 URL을 입력하거나 링크를 클릭하면, HTTP 요청이 서버로 전송됩니다.

2. **서버에서 처리**:

   - 서버는 요청된 PHP 스크립트를 실행합니다. PHP는 데이터를 처리하고, 데이터베이스와 상호작용하며, HTML을 동적으로 생성합니다.

3. **HTML 생성**:

   - PHP는 필요한 데이터를 포함한 완전한 HTML 문서를 생성하고, 이를 클라이언트로 전송합니다.

4. **클라이언트에 전송**:

   - 서버는 생성된 HTML을 클라이언트(브라우저)로 응답으로 보냅니다.

5. **클라이언트에서 렌더링**:
   - 브라우저는 서버에서 받은 HTML을 렌더링하여 사용자에게 페이지를 표시합니다.

#### **장점**:

- **초기 로딩 속도**: 서버에서 이미 완성된 HTML을 보내기 때문에 초기 페이지 로딩 속도가 빠를 수 있습니다.
- **SEO 최적화**: 검색 엔진이 HTML 문서를 쉽게 크롤링할 수 있어, SEO에 유리합니다.
- **브라우저 호환성**: 모든 브라우저가 서버에서 전송된 HTML을 처리할 수 있으므로, 호환성 문제가 적습니다.

#### **단점**:

- **서버 부하**: 서버에서 모든 요청에 대해 HTML을 생성해야 하므로, 많은 요청이 있을 때 서버에 부하가 발생할 수 있습니다.
- **인터랙티브 기능 제한**: 클라이언트 측에서 즉각적인 반응이 필요할 때, 서버로부터 새로고침 없이 데이터를 요청하기 어렵습니다.

### 2. **클라이언트사이드 렌더링 (Client-Side Rendering, CSR)**

**클라이언트사이드 렌더링**은 웹 페이지의 렌더링을 클라이언트 측, 즉 사용자의 브라우저에서 처리하는 방식입니다. 이 접근법은 주로 **JavaScript** 프레임워크와 라이브러리(예: React.js, Vue.js, Angular)에서 사용됩니다.

#### **클라이언트사이드 렌더링 과정**:

1. **클라이언트 요청**:

   - 사용자가 웹 브라우저에서 URL을 입력하거나 링크를 클릭하면, 초기 요청이 서버로 전송됩니다.

2. **서버에서 데이터 및 애플리케이션 전송**:

   - 서버는 정적 HTML 페이지나 기본 템플릿을 전송하고, JavaScript 파일과 데이터를 함께 전송합니다. 이 HTML은 보통 최소한의 마크업만 포함하며, 콘텐츠는 거의 비어 있습니다.

3. **클라이언트에서 처리**:

   - 브라우저는 받은 JavaScript 파일을 실행합니다. JavaScript는 서버에서 추가 데이터를 받아와서 동적으로 HTML을 생성하고, 페이지에 렌더링합니다.

4. **렌더링 및 업데이트**:
   - 이후 사용자와의 상호작용은 대부분 클라이언트 측에서 이루어지며, 서버로부터 필요한 데이터만 비동기적으로 가져옵니다(예: AJAX, Fetch API).

#### **장점**:

- **인터랙티브한 사용자 경험**: 클라이언트에서 렌더링을 처리하기 때문에, 페이지 간의 전환이 부드럽고, 사용자가 빠르게 반응할 수 있는 인터랙티브한 웹 애플리케이션을 만들 수 있습니다.
- **서버 부하 감소**: 서버는 정적 리소스와 API 요청만 처리하면 되므로, 부하가 줄어들 수 있습니다.
- **모듈화**: 컴포넌트 기반 아키텍처를 통해 코드를 재사용하고, 유지보수를 쉽게 할 수 있습니다.

#### **단점**:

- **초기 로딩 시간**: 초기 페이지 로딩 시, 클라이언트가 JavaScript 파일을 다운로드하고 실행해야 하므로 로딩이 지연될 수 있습니다.
- **SEO 문제**: 초기 요청 시 HTML 문서가 거의 비어 있으므로, 검색 엔진이 콘텐츠를 크롤링하는 데 어려움이 있을 수 있습니다(이 문제를 해결하기 위해 SSR과 CSR을 혼합한 하이브리드 방식이 사용되기도 합니다).
- **브라우저 의존성**: JavaScript를 처리할 수 없는 브라우저나 설정에서는 애플리케이션이 제대로 작동하지 않을 수 있습니다.

### **요약 및 비교**

- **서버사이드 렌더링 (SSR)**:

  - 서버에서 HTML을 완성한 후 클라이언트로 전송.
  - 초기 로딩 속도가 빠르며, SEO에 유리.
  - 서버 부하가 클 수 있으며, 사용자 경험이 덜 인터랙티브.

- **클라이언트사이드 렌더링 (CSR)**:
  - 클라이언트에서 HTML을 생성 및 렌더링.
  - 사용자 경험이 더 인터랙티브하고, 서버 부하가 줄어듦.
  - 초기 로딩 시간이 길어질 수 있으며, SEO 문제 발생 가능.

각 접근 방식은 특정 상황에서 더 적합할 수 있으며, **SSR**과 **CSR**을 혼합하여 사용하는 **하이브리드 접근**도 존재합니다. 예를 들어, SEO가 중요한 페이지는 SSR을 사용하고, 이후 페이지 간 전환은 CSR로 처리하는 방식이 있습니다.
