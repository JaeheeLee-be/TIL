### Context
Django REST Framework (DRF)에서는 인증(Authentication) 및 인가(Authorization)의 실패를 구분하지 않고 `403 Forbidden` 에러를 반환합니다. 이로 인해 클라이언트는 인증 실패와 권한 부족을 혼동할 수 있습니다. 이를 해결하기 위해 인증 실패 시 `401 Unauthorized`로 응답할 수 있도록 커스텀 예외 핸들러를 구현했습니다. 이는 API 설계에서 클라이언트의 에러 핸들링을 개선하는 데 중요한 역할을 합니다.

### Core
커스텀 예외 핸들러를 구현하기 위해 `custom_exception_handler` 함수를 정의하였습니다. 이 함수는 DRF의 기본 핸들러를 오버라이드하고, `NotAuthenticated` 예외와 `PermissionDenied` 예외를 구분하여 처리합니다. 예시는 아래와 같습니다.

```python
from rest_framework.views import exception_handler
from rest_framework.exceptions import NotAuthenticated, PermissionDenied

def custom_exception_handler(exc, context):
    response = exception_handler(exc, context)
    if isinstance(exc, NotAuthenticated):
        return Response({"detail": "로그인이 필요합니다"}, status=401)
    elif isinstance(exc, PermissionDenied):
        return Response({"detail": "권한이 없습니다"}, status=403)
    return response
```

이렇게 설정된 커스텀 핸들러는 인증과 인가의 실패를 적절히 처리하여, 각각의 상황에 맞는 HTTP 상태 코드를 반환합니다.

### Insight
이 커스텀 예외 핸들러를 통해 클라이언트는 인증 실패와 권한 부족에 대한 더 정확한 정보를 알 수 있어, 에러를 보다 효과적으로 처리할 수 있게 됩니다. 인증 오류 시 `401 Unauthorized`를 반환함으로써 클라이언트에게 필요한 조치를 명확히 전달할 수 있습니다. 이는 API의 사용자 경험을 개선하고, 클라이언트와 서버 간의 상호작용을 더 원활하게 합니다. 끝으로, 이러한 차별화된 에러 처리는 개발자에게도 중요한 정보가 될 수 있습니다.  

**출처:** [Django REST Framework Documentation](https://www.django-rest-framework.org/api-guide/exceptions/)  
