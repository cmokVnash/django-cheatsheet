# django-cheatsheet

# DJANGO REST FRAMEWORK (DRF)

## Permission Classes
*AllowAny
*IsAuthenticated
*IsAdminUser
*IsAuthenticatedOrReadOnly

## Example
### class-based views
```python
permission_classes = [IsAuthenticated]
```
### function based views
```python
@api_view(['GET'])
@permission_classes([IsAuthenticated])
```

## Settings.py set default permission classes
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'oauth2_provider.contrib.rest_framework.OAuth2Authentication',
    ),

    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
```

# DRF VIEWS:

The APIView class is pretty much the same as using a regular View class, as usual, the incoming request is dispatched to an appropriate handler method such as .get() or .post()
[View Docs](https://www.django-rest-framework.org/api-guide/views/)

## class-based

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import authentication, permissions
from django.contrib.auth.models import User

class ListUsers(APIView):
    """
    View to list all users in the system.

    * Requires token authentication.
    * Only admin users are able to access this view.
    """
    authentication_classes = [authentication.TokenAuthentication]
    permission_classes = [permissions.IsAdminUser]

    def get(self, request, format=None):
        """
        Return a list of all users.
        """
        usernames = [user.username for user in User.objects.all()]
        return Response(usernames)
        ```

## function-based

```python
#
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view()
def hello_world(request):
    return Response({"message": "Hello, world!"})
    
#By default only GET methods will be accepted. Other methods will respond with "405 Method Not Allowed". To alter this behaviour, #specify which methods the view allows, like so:

@api_view(['GET', 'POST'])
def hello_world(request):
    if request.method == 'POST':
        return Response({"message": "Got some data!", "data": request.data})
    return Response({"message": "Hello, world!"})
```

# Django and DRF Requests and Response methods
#HttpRequest as request

```python
request.scheme #returns a string representing the scheme of the request (http or https usually).
#read url & urn section of the following

```
[read more on schemes](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#:~:text=Each%20URI%20begins%20with%20a,of%20identifiers%20using%20that%20scheme.)

```python
request.data (drf)
```
returns the parsed content of the request body. This is similar to the standard request.POST and request.FILES attributes except that:
*It includes all parsed content, including file and non-file inputs.
*It supports parsing the content of HTTP methods other than POST, meaning that you can access the content of PUT and PATCH requests.
*It supports REST framework's flexible request parsing, rather than just supporting form data. For example you can handle incoming JSON data similarly to how you handle incoming form data.


***
# OAuth

## [Workflow](https://www.geeksforgeeks.org/workflow-of-oauth-2-0/)
## Token Generation datas:
grant_type: 
client_id:
client_secret:
### #following depends on grant_type:
username:
password:

----
authorization_code:
----
etc







