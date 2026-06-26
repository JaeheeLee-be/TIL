### Context
Django는 동기 기반 프레임워크로서 WSGI(Web Server Gateway Interface) 위에서 실행됩니다. 반면 FastAPI는 ASGI(Asynchronous Server Gateway Interface) 위에서 비동기적으로 작동합니다. 두 프레임워크의 특징은 매우 상이하며, 각각의 상황에 따라 장단점이 있습니다. Django의 경우 ORM(Object-Relational Mapping), 인증 및 다양한 풀스택 기능이 내장되어 있어 빠른 개발을 지원합니다. 그러나 높은 동시성 처리에 있어서는 FastAPI에 비해 한계가 있습니다. 반면 FastAPI는 `async/await` 문법을 지원하여 높은 동시성을 제공하나, ORM과 같은 여러 기능을 사용자 직접 구현해야 합니다.

TravelMaker 프로젝트에 Django를 채택한 이유는 Django REST Framework(DRF)의 높은 생산성과 내장 기능 때문입니다. DRF는 Django의 장점을 살리면서 API 개발을 효율적으로 만들어 줍니다.