# About Project App

## Language / Library Used

1. React JS
2. react-router-dom

## Future Plans

# NOTES

## How does authentication work?

there are two ways -

1. Server side Sessions
   Server side sessions are very popular solution for full stack application where we don't have a decoupled frontend and backend. as we do have in react so its not ideal for react.

- Store unique identifier on server, send same identifier to client.
- Client sends identifier along with requests to protected resources.
- Server can then check if the identifier is valid ( = previously issued by server to client)

2. Authentication Tokens

- Create (but not stored) "permission" token on server & send it to the client
- Client attaches token to future requests for protected resources
- Server can then verify the attached token

## Query Parameters

to get query parameters -

```
import { Link, useSearchParams } from 'react-router-dom';

function AuthForm() {
  const [searchParams, setSearchParams] = useSearchParams();
  const isLogin = searchParams.get('mode') === 'login';

  return (
    ...
  )}
export default AuthForm;

```

Here searchParams will give us all query parameters. to get value of a specific query parameter we need to use -

```
searchParams.get('mode')
```

_setSearchParams_ is used to update value of query params

to update query parameters -

```
<Link to={`?mode=${isLogin ? 'signup' : 'login'}`}>
    {isLogin ? 'Create new user' : 'Login'}
</Link>
```
