# About Project App

## Language / Library Used

1. React JS
2. react-router-dom

## Future Plans

# NOTES

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
