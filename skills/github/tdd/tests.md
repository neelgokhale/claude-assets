# Good and Bad Tests

## Good Tests

**Integration-style**: Test through real interfaces, not mocks of internal parts.

```python
# GOOD: Tests observable behavior
def test_user_can_checkout_with_valid_cart():
    cart = create_cart()
    cart.add(product)
    result = checkout(cart, payment_method)
    assert result.status == "confirmed"
```

Characteristics:

- Tests behavior users/callers care about
- Uses public API only
- Survives internal refactors
- Describes WHAT, not HOW
- One logical assertion per test

## Bad Tests

**Implementation-detail tests**: Coupled to internal structure.

```python
# BAD: Tests implementation details
def test_checkout_calls_payment_service_process(mocker):
    mock_payment = mocker.patch("myapp.checkout.payment_service")
    checkout(cart, payment)
    mock_payment.process.assert_called_once_with(cart.total)
```

Red flags:

- Mocking internal collaborators
- Testing private methods (e.g. patching `_helper` or asserting on name-mangled attributes)
- Asserting on call counts/order
- Test breaks when refactoring without behavior change
- Test name describes HOW not WHAT
- Verifying through external means instead of interface
- Asserting on prompt/string contents instead of the behavior those strings produce
- If prompts were written for this tracer bullet, do not test on its content. Checking if it contains words / phrases proves nothing, breaks on harmless rewording ("prioritize safety" → "stay safe") and passes even when it produces broken behavior. 
- The same anti-pattern shows up as `assert "error" in str(response)` or `assert "Dear" in email_body` — anywhere a test checks that a string contains a phrase the code was written to emit.
