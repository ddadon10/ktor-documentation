[//]: # (title: Session authentication)

<microformat>
<p>
Required dependencies: <code>io.ktor:ktor-auth</code>, <code>io.ktor:ktor-server-sessions</code>
</p>
<var name="example_name" value="auth-form-session"/>
<include src="lib.xml" include-id="download_example"/>
</microformat>


[Sessions](sessions.md) provide a mechanism to persist data between different HTTP requests. Typical use cases include storing a logged-in user's ID, the contents of a shopping basket, or keeping user preferences on the client. 

In Ktor, a user that already has an associated session can be authenticated using the `session` provider. For example, when a user logs in using a [web form](form.md) for the first time, you can save a username to a cookie session and authorize this user on subsequent requests using the `session` provider.

> You can get general information about authentication and authorization in Ktor in the [](authentication.md) section.

## Add dependencies {id="add_dependencies"}
To enable `session` authentication, you need to include the following artifacts in the build script:

* Add the `ktor-server-sessions` dependency for using sessions:
  <var name="artifact_name" value="ktor-server-sessions"/>
  <include src="lib.xml" include-id="add_ktor_artifact"/>

* Add the `ktor-auth` dependency for authentication:
  <var name="artifact_name" value="ktor-auth"/>
  <include src="lib.xml" include-id="add_ktor_artifact"/>

## Session authentication flow {id="flow"}

The authentication flow with sessions might vary and depends on how users are authenticated in your application. Let's see how it might look with the [form-based authentication](form.md):

1. A client makes a request containing web form data (which includes the username and password) to a server.
2. A server validates credentials sent by a client, saves a username to a cookie session, and responds with the requested content and a cookie containing a username.
3. A client makes a subsequent request with a cookie to the protected resource.
4. Based on received cookie data, Ktor checks that a cookie session exists for this user and, optionally, performs additional validations on received session data. In a case of successful validation, a server responds with the requested content.


## Install session authentication {id="install"}
To install the `session` authentication provider, call the [session](https://api.ktor.io/ktor-features/ktor-auth/ktor-auth/io.ktor.auth/session.html) function with the required session type inside the `install` block:

```kotlin
install(Authentication) {
    session<UserSession> {
        // Configure session authentication
    }
}
```

## Configure session authentication {id="configure"}
This section demonstrates how to authenticate a user with a [form-based authentication](form.md), save information about this user to a cookie session, and then authorize this user on subsequent requests using the `session` provider. You can find the complete example here: [auth-form-session](https://github.com/ktorio/ktor-documentation/tree/main/codeSnippets/snippets/auth-form-session).

### Step 1: Create a data class {id="data-class"}
First, you need to create a data class for storing session data. Note that this class should inherit [Principal](https://api.ktor.io/ktor-features/ktor-auth/ktor-auth/io.ktor.auth/-principal/index.html) since the [validate](#configure-session-auth) function should return a `Principal` in a case of successful authentication.

```kotlin
```
{src="snippets/auth-form-session/src/main/kotlin/com/example/Application.kt" lines="11"}

### Step 2: Install and configure a session {id="install-session"}
After creating a data class, you need to install and configure the `Sessions` plugin. A code snippet below shows how to install and configure a cookie session with the specified cookie path and expiration time.

```kotlin
```
{src="snippets/auth-form-session/src/main/kotlin/com/example/Application.kt" lines="14-19"}

You can learn more about configuring sessions from [](sessions.md#configure).


### Step 3: Configure session authentication {id="configure-session-auth"}

The `session` authentication provider exposes its settings via the [SessionAuthenticationProvider/Configuration](https://api.ktor.io/ktor-features/ktor-auth/ktor-auth/io.ktor.auth/-session-authentication-provider/-configuration/index.html) class. In the example below, the following settings are specified:
* The `validate` function checks the [session instance](#data-class) and returns `Principal` in a case of successful authentication.
* The `challenge` function specifies an action performed if authentication fails. For instance, you can redirect back to a login page or send [UnauthorizedResponse](https://api.ktor.io/ktor-features/ktor-auth/ktor-auth/io.ktor.auth/-unauthorized-response/index.html).

```kotlin
```
{src="snippets/auth-form-session/src/main/kotlin/com/example/Application.kt" lines="20,32-44"}


### Step 4: Save user data in a session {id="save-session"}

To save information about a logged-in user to a session, use the [call.sessions.set](sessions.md#set-content) function.

```kotlin
```
{src="snippets/auth-form-session/src/main/kotlin/com/example/Application.kt" lines="67-73"}

### Step 5: Define authorization scope {id="authenticate-route"}

After configuring the `session` provider, you can define the authorization for the different resources in our application using the `authenticate` function. In a case of successful authentication, you can retrieve an authenticated `Principal` (the [UserSession](#data-class) instance) inside a route handler using the [call.principal](https://api.ktor.io/ktor-features/ktor-auth/ktor-auth/io.ktor.auth/principal.html) function and information about a user.

```kotlin
```
{src="snippets/auth-form-session/src/main/kotlin/com/example/Application.kt" lines="75-81"}

You can find the full example here: [auth-form-session](https://github.com/ktorio/ktor-documentation/tree/main/codeSnippets/snippets/auth-form-session).
