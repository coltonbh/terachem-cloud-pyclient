# Core Development Decisions

## \_RequestsClient Class

- `_RequestsClient` public http methods (like `compute()` and `result()` should always return Python objects. This gives a layer of abstraction between callers who want to think in terms of Python data objects and the `_RequestsClient` which thinks in terms of http requests and `json` data structures.

I'm starting to have second thoughts about this ^^ decision. It feels like the `_RequestsClient` is starting to take on too much responsibility. It accepts python data types as parameters, and returns python data types as it if were an end-user class. It isn't. It's meant to be a utility class used by end-user objects such as `CCClient` and `FutureOutput` objects. I think it should return data more directly from the ChemCloud API and let the other classes handle this data. This becomes more apparent as I add `pydantic` to my data models and realize I'd rather have them pass rawer data types to the `_RequestsClient` and then handle the results of an API call inside their own class. Maybe the `compute()` method on the `_RequestsClient` should go away and these should live exclusively on the `CCClient` object which then utilizes `request` and `authenticated_request` to access ChemCloud.