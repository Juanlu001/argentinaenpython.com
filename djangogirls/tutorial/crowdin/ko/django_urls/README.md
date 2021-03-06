# Django urls

첫 웹페이지를 만들어 보기로 해요. : 여러분의 블로그를 위한 홈페이지요! 하지만 먼저 장고 url에 대해서 조금 배워보기로 합시다.

## URL이란 무엇인가요?

URL은 단순히 웹 주소랍니다. 웹사이트를 방문할 때마다 URL을 볼 수 있죠. 브라우저의 주소창에 보이죠, 맞아요! `127.0.0.1:8000`가 바로 URL이에요! 그리고 `https://djangogirls.com` 또한 URL이랍니다:

![URL][1]

 [1]: images/url.png

인터넷에 있는 모든 페이지들은 자신만의 URL을 가지고 있어야 해요. 이런 방식으로 어플리케이션은 URL을 입력한 사용자에게 어떤 내용을 보여줘야할지 알게 됩니다. 장고는 `URLconf` (URL configuration)를 사용합니다. URLconf는 장고에서 URL과 일치하는 뷰를 찾기 위한 패턴들의 집합입니다.

## Django에서 URL은 어떻게 작동할까요?

코드 에디터에서 `mysite/urls.py`파일을 열면 아래와 같을 거에요. :

    python
    from django.conf.urls import include, url
    from django.contrib import admin
    
    urlpatterns = [
        # Examples:
        # url(r'^$', 'mysite.views.home', name='home'),
        # url(r'^blog/', include('blog.urls')),
    
        url(r'^admin/', include(admin.site.urls)),
    ]
    

장고가 이미 어떤 내용을 넣어 두었네요.

`#`로 시작되는 줄은 주석이에요. 이 말은 파이썬은 이 줄을 실행하지 않는다는 뜻이지요. 꽤 유용하겠죠?

이전 장에서 봤던 관리자 URL도 여기에 이미 있어요. :

    python
        url(r'^admin/', include(admin.site.urls)),
    

이 의미는 `admin/`으로 시작하는 모든 URL을 장고가 *view*와 대조해 찾아낸다는 뜻입니다. 이 경우 많은 admin URL을 포함해야 하기 때문에 작은 파일안에 모두 들어가지 않아요. 여기에 좀 더 읽기 좋고 깔끔한 방법이 있어요.

## 정규표현식(Regex)

장고가 URL을 뷰에 매칭시키는 방법이 궁금하죠? 이 부분은 조금 까다로울 수 있어요. 장고는 `regex`를 사용하는데, "정규표현식(regular expressions)"의 줄임말입니다. 정규식은 정말 (아주!) 많은 검색 패턴의 규칙을 가지고 있어요. 정규식은 심화 내용이기 때문에, 자세한 내용은 다루지 않을 거에요.

패턴을 만드는 방법이 궁금하다면, 아래에 있는 표기법을 확인하세요. 우리는 패턴을 찾기 위해 필요한 몇 가지 규칙만 필요합니다. :

    ^ 문자열이 시작할 떄
    $ 문자열이 끝날 때
    \d 숫자
    + 바로 앞에 나오는 항목이 계속 나올 때
    () 패턴의 부분을 저장할 때
    

이외에 url 정의는 문자적으로 만들 수 있어요.

이런 사이트 주소가 있다고 해봅시다. : `http://www.mysite.com/post/12345/` 여기에서 `12345`는 글 번호를 의미합니다.

뷰마다 모든 글 번호을 작성하는 것은 정말 힘든 일이 될 거에요. 정규 표현식으로 url과 매칭되는 글 번호를 뽑을 수 있는 패턴을 만들 수 있어요. 이렇게 말이죠. : `^post/(\d+)/$`. 어떤 뜻인지 하나 씩 나누어 어떤 뜻인지 알아볼게요. :

*   **^post/**는 장고에게 url 시작점에 (오른쪽부터) `post/`가 있다는 것을 말해 줍니다. `^`)
*   **(\d+)**는 숫자(한 개 또는 여러개) 가 있다는 뜻입니다. 내가 뽑아내고자 글 번호가 되겠지요.
*   **/**는 장고에게 `/`뒤에 문자가 있음을 말해 줍니다.
*   **$**는 URL의 끝이 방금 전에 있던 `/`로 끝나야 매칭될 수 있다는 것을 나타냅니다.

## 나의 첫 번째 Django url!

첫 번째 URL을 만들어 봅시다! 우리는 'http://127.0.0.1:8000/'가 홈페이지 주소로 만들어 글 목록이 보이게 만들어 볼 거에요.

또한 `mysite/urls.py`파일을 깨끗한 상태로 유지하기 위해, `blog` 어플리케이션에서 메인 `mysite/urls.py`파일로 url들을 가져올 거에요.

먼저 `#`로 시작하는 줄을 삭제하고 main url ('')로 `blog.urls`를 가져오는 행을 추가해 봅시다`''`).

이제 `mysite/urls.py` 파일은 아래처럼 보일 거에요.

    python
    from django.conf.urls import include, url
    from django.contrib import admin
    
    urlpatterns = [
        url(r'^admin/', include(admin.site.urls)),
        url(r'', include('blog.urls')),
    ]
    

지금 장고는 'http://127.0.0.1:8000/'로 들어오는 모든 접속 요청을 `blog.urls`로 전송하고 추가 명령을 찾을 거에요.

파이썬에서 정규 표현식을 작성할 때는 항상 문자열 앞에 `r`을 붙입니다. 이는 파이썬에게는 별 의미가 없지만, 파이썬에게 문자열에 특수 문자를 있다는 것을 알려줍니다.

## blog.urls

`blog/urls.py`이라는 새 파일을 생성하세요. 좋아요! 이제 다음 두 줄을 추가하세요.

    python
    from django.conf.urls import url
    from . import views
    

우리는 장고의 메소드와 `blog` 어플리케이션에서 사용할 모든 `views`들을 불러오고 있어요. (물론 아직 뷰를 하나도 안 만들었지만, 곧 만들거니 조금만 기다리세요!)

그 다음, 첫 번째 URL 패턴을 추가하세요.

    python
    urlpatterns = [
        url(r'^$', views.post_list, name='post_list'),
    ]
    

이제 `post_list`라는 이름의 `view`가 `^$` URL에 할당되었습니다. 이 정규표현식은 `^`에서 시작해 `$`로 끝나는 지를 매칭할 것입니다. 즉 문자열이 아무것도 없는 경우만 매칭하겠죠. 틀린 것이 아니에요. 왜냐하면 장고 URL 확인자(resolver)는 'http://127.0.0.1:8000/' 는 URL의 일부가 아니기 때문입니다. 이 패턴은 장고에게 누군가 웹사이트에 'http://127.0.0.1:8000/' 주소로 들어왔을 때`views.post_list`를 보여주라고 말할 거에요.

마지막 부분인 `name='post_list'` 는 URL에 이름을 붙인 것으로 뷰를 식별합니다. 이 부분은 뷰의 이름과 같을 수도 완전히 다를 수도 있습니다. 이름을 붙인 URL은 프로젝트의 후반에 사용할 거에요. 그러니 앱의 각 URL을 이름짓는 것은 중요합니다. 또 URL에 고유한 이름을 붙여, 외우고 부르기 쉽게 만들어야 해요.

모두 잘 되고 있나요? htpp://127.0.0.1:8000/으로 접속해 결과를 확인해보세요.

![Error][2]

 [2]: images/error1.png

이런, 잘 "작동" 하는 것 같지 않다구요? 걱정마세요. 이건 그냥 오류 페이지에요. 그러니 무서워하지마세요! 오류는 꽤 유용하게 쓰인 답니다. :

페이지에서 **no attribute 'post_list'**라는 오류가 보일 거에요. *post_list*에서 혹시 떠오르는 것이 있나요? 바로 뷰(view) 를 말하는 거죠! 모두 준비가 되었다는 뜻이에요. 아직 *view*를 안 만들었다는 것만 빼고요. 걱정 마세요. 금방 만들어 볼 거에요.

> 장고 URL 설정에 대해 더 알고 싶다면 공식 문서를 읽어보세요. : https://docs.djangoproject.com/en/1.8/topics/http/urls/