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

## Attaching Auth token to outgoing request.

**getting token from backend**

```
  const resData = await response.json();
  const token = resData.token;
```

**storing token to local storage**

```
  localStorage.setItem('token', token);
  const expiration = new Date();
  expiration.setHours(expiration.getHours() + 1);
  localStorage.setItem('expiration', expiration.toISOString());
```

**Refreshing token**

```
  export function getAuthToken() {
    const token = localStorage.getItem('token');

    if (!token) {
      return null;
    }

    const tokenDuration = getTokenDuration();

    if (tokenDuration < 0) {
      return 'EXPIRED';
    }

    return token;
  }
```

**sending token to backend**

```
  const token = getAuthToken();
  const response = await fetch('http://localhost:8080/events/' + eventId, {
    method: request.method,
    headers: {
      'Authorization': 'Bearer ' + token
    }
  });
```

## logout

```
{token && (
  <li>
    <Form action="/logout" method="post">
      <button>Logout</button>
    </Form>
  </li>
)}
```

```
import { redirect } from 'react-router-dom';

export function action() {
  localStorage.removeItem('token');
  localStorage.removeItem('expiration');
  return redirect('/');
}
```

## Adding Route Protection

To prevent unauthorized access to some routes we need to add route protection by adding authentication checking loader in them

```
{
  path: 'edit',
  element: <EditEventPage />,
  action: manipulateEventAction,
  loader: checkAuthLoader,
},
```

```
export function checkAuthLoader() {
	const token = getAuthToken();

	if (!token) {
		return redirect("/auth");
	}
	return null;
}
```

## Adding automatic logout

in Root.js we include

```
useEffect(() => {
	if (!token) {
		return null;
		// Returning "undefined" won't work as intended (the loader won't return that value to the component)
	}

	if (token === "EXPIRED") {
		submit(null, { action: "/logout", method: "post" });
		return;
	}

	const tokenDuration = getTokenDuration();
	console.log(tokenDuration);

	setTimeout(() => {
		submit(null, { action: "/logout", method: "post" });
	}, tokenDuration);
}, [token, submit]);
```
