## High Level Description

Our company wants to write a checking tool for internal JSONs that store data. These JSONs are checked, everytime our checking system finds an error, it generates a JSON error object like:

```json
{
  "index": SOME_NUMERIC_INDEX,
  "code": SOME_NUMERIC_ERROR_CODE,
  "text": SOME_TEXT_DESCRIPTION
}
```

We then wrote an API that will get all errors generated so far, seperated by their status. A human operator has to be able to see and understand these errors in order to fix them in the original data. Our company strives to reduce errors to almost zero by providing a flawless UI/UX for operators to check errors and resolve them.

## VueJS along with FASTAPI Python
