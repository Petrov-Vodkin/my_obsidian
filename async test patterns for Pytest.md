2021-10-17 12:14
[link](https://tonybaloney.github.io/posts/async-test-patterns-for-pytest-and-unittest.html)
# Async test patterns for Pytest
## Async test functions

The easiest way to have `async` test functions in Pytest is to load the [`pytest-asyncio`](https://pypi.org/project/pytest-asyncio/) extension and use the `asyncio` marker:

```py
import pytest

@pytest.mark.asyncio
async def test_an_async_function():
    result = await call_to_my_async_function()
    assert result == 'banana'
```

**Caution ::** If you just add `async` before your test methods without the marker, Pytest won’t await them and they’ll pass regardless!!

The marker also applies to test class groups, awaiting any async methods and working normally on any sync test methods:

```py
@pytest.mark.asyncio
class TestGroup:
    async def test_an_async_function(self):
        result = await call_to_my_async_function()
        assert result == 'banana'

    async def test_another_async_function(self):
        result = await call_to_my_async_function()
        assert result == 'banana'

    def test_a_normal_function(self):
        result = call_to_my_normal_function()
        ...
```

## Async fixtures

The `pytest-asyncio` extension also enables async fixtures, for example I recently had to create an async fixture for a HTTP async client for FastAPI. The challenge with Httpx (and other HTTP/TCP async classes) is they require usage in a context-manager, so that sockets are closed. If you don’t close them, you’ll get a stack of warnings about unclosed HTTP sessions.

The solution was to create an async fixture, yielded within an async context-manager, so that once the test is completed, it will close the AsyncClient:

```py
from httpx import AsyncClient
import pytest
from my_app import app

@pytest.fixture
async def async_app_client():
    async with AsyncClient(app=app, base_url='http://test') as client:
        yield client

```

This example uses [httpx](https://github.com/encode/httpx), which comes with an async HTTP client. To use this fixture, you can await its methods:

```py
@pytest.mark.asyncio
async def test_create_user(async_app_client):
    response = await async_app_client.post(
        "/register",
        json={
            "name": "Test User",
            "email": "test@test.com",
            "password": "password",
        },
    )
    assert response.status_code == 200, response.text
```

## Unittest classes from Pytest

Pytest has support for running unittest `TestCase` classes, but there are lots of caveats to the support.

Once such caveat is that you can’t annotate `@pytest.mark.asyncio` on a test method that’s part of a `unittest.TestCase`. Instead, you have to create a class-level fixture for the event loop, then auto-use it on your test cases.

The challenge is that you can’t await a coroutine from a non-async method and unittest doesn’t easily support awaiting on test methods. Instead, you need to get the eventloop and then create a task. Newer versions of Python have a convenience method on the event loop (`run_until_complete(Coroutine)`).

This example sets an instance on test case so that you can write sync test methods will call coroutines.

Here is a `get_async_result()` method for convenience:

```py
import pytest
import asyncio


@pytest.fixture(scope="class")
def event_loop_instance(request):
    """ Add the event_loop as an attribute to the unittest style test class. """
    request.cls.event_loop = asyncio.get_event_loop_policy().new_event_loop()
    yield
    request.cls.event_loop.close()

@pytest.mark.usefixtures("event_loop_instance")
class TestTheThings(unittest.TestCase):

    def get_async_result(self, coro):
        """ Run a coroutine synchronously. """
        return self.event_loop.run_until_complete(coro)

    def test_an_async_method(self):
        result = self.get_async_result(run_what_ever_your_async_function_is())
        # result is the actual result, so whatever assertions..
        self.assertEqual(result,  "banana")

```

## Async mocks

The `unittest.mock` library, or `pytest-mock` comes with `Mock()` and `MagicMock()` classes for patching out functions in modules and methods in classes. This is brilliant, but they implement the `__call__()` magic-method, so your mocks wont work.

Python 3.8 comes with an `AsyncMock` class, but theres also the [`asyncmock` library](https://pypi.org/project/asyncmock/), which supports older versions of Python.

In this example, you want to patch a class that looks like this:

```py
class TypeToMock:
    async def the_function_to_mock(self):
        return some_real_data
```

Create a fixture (or just a mock instance within the test function) and instead of using `Mock()` you use `AsyncMock()`

```py
import pytest
from asyncmock import AsyncMock

@pytest.fixture
def mock_thing():
    mock_thing = AsyncMock()
    mock_thing.the_function_to_mock = AsyncMock(return_value = "fake_result_data")
    return mock_thing

@pytest.mark.asyncio
async def test_my_test_class(mock_thing):
    # the patched function can be awaited..
    result = await mock_thing.the_function_to_mock()
```

You can use this in the same way you’d use mock, for example testing side-effect exceptions:

```py
@pytest.mark.asyncio
async def test_raise_exception():
    my_mock = AsyncMock(side_effect=KeyError)

    with pytest.raises(KeyError):
        await my_mock()

    my_mock.assert_called()
```

That’s it for now! I’ll add more to this in the future as I come across more problems. If you have any challenges testing async, let me know.
_____________
#### Links
