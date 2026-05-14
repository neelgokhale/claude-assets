# When to Mock

Mock at **system boundaries** only:

- External APIs (payment, email, etc.)
- Databases (sometimes - prefer test DB)
- Time/randomness
- File system (sometimes)

Don't mock:

- Your own classes/modules
- Internal collaborators
- Anything you control

## Designing for Mockability

At system boundaries, design interfaces that are easy to mock:

**1. Use dependency injection**

Pass external dependencies in rather than creating them internally:

```python
# Easy to mock
def process_payment(order, payment_client):
    return payment_client.charge(order.total)

# Hard to mock
def process_payment(order):
    client = StripeClient(os.environ["STRIPE_KEY"])
    return client.charge(order.total)
```

**2. Prefer SDK-style interfaces over generic fetchers**

Create specific methods for each external operation instead of one generic method with conditional logic:

```python
# GOOD: Each method is independently mockable
class Api:
    def get_user(self, user_id: int) -> dict:
        return self._session.get(f"/users/{user_id}").json()

    def get_orders(self, user_id: int) -> list[dict]:
        return self._session.get(f"/users/{user_id}/orders").json()

    def create_order(self, data: dict) -> dict:
        return self._session.post("/orders", json=data).json()


# BAD: Mocking requires conditional logic inside the mock
class Api:
    def request(self, method: str, endpoint: str, **kwargs) -> dict:
        return self._session.request(method, endpoint, **kwargs).json()
```

The SDK approach means:

- Each mock returns one specific shape (`mocker.patch.object(api, "get_user", return_value=...)`)
- No `side_effect` function branching on endpoint strings in test setup
- Easier to see which endpoints a test exercises
- Type safety per endpoint