
# To Do List API

An API built for a to do list app. It has authentication, allowing
the user to sign in, sign out, sign up, and change password
The API will allow for the creation of new list items, editing
existing list items, delete list items, and will get all the
list items for the associated user when they sign in.

## API end-points

| Verb   | URI Pattern            | Controller#Action |
|--------|------------------------|-------------------|
| POST   | `/sign-up`             | `users#signup`    |
| POST   | `/sign-in`             | `users#signin`    |
| DELETE | `/sign-out/:id`        | `users#signout`   |
| PATCH  | `/change-password/:id` | `users#changepw`  |
| GET    | `/items`               | `items#index`     |
| POST   | `/games`               | `games#create`    |
| GET    | `/games/:id`           | `games#show`      |
| PATCH  | `/games/:id`           | `games#update`    |
| GET    | `/games/:id/watch`     | `games#watch`     |

All data returned from API actions is formatted as JSON.

---

## User actions

*Summary:*

<table>
<tr>
  <th colspan="3">Request</th>
  <th colspan="2">Response</th>
</tr>
<tr>
  <th>Verb</th>
  <th>URI</th>
  <th>body</th>
  <th>Status</th>
  <th>body</th>
</tr>
<tr>
<td>POST</td>
<td>`/sign-up`</td>
<td><strong>credentials</strong></td>
<td>201, Created</td>
<td><strong>user</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>400 Bad Request</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>POST</td>
<td>`/sign-in`</td>
<td><strong>credentials</strong></td>
<td>200 OK</td>
<td><strong>user w/token</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>401 Unauthorized</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>DELETE</td>
<td>`/sign-out/:id`</td>
<td>empty</td>
<td>201 Created</td>
<td>empty</td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>401 Unauthorized</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>PATCH</td>
<td>`/change-password/:id`</td>
<td><strong>passwords</strong></td>
<td>204 No Content</td>
<td><strong>user w/token</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>400 Bad Request</td>
  <td><em>empty</em></td>
</tr>
</table>

### signup

The `create` action expects a *POST* of `credentials` identifying a new user to
 create.

```
```json
{
  "credentials": {
    "email": "an@example.email",
    "password": "an example password",
    "password_confirmation": "an example password"
  }
}
```

### signin

The `signin` action expects a *POST* with `credentials` identifying a previously
 registered user.

```json
{
  "credentials": {
    "email": "an@example.email",
    "password": "an example password"
  }
}
```

If the request is successful, the response will have an HTTP Status of 200 OK,
 and the body will be JSON containing the user's `id`, `email`, and the `token`
 used to authenticate other requests.
```json
{
  "user": {
    "id": 1,
    "email": "an@example.email",
    "token": "an example authentication token"
  }
}
```

If the request is unsuccessful, the response will have an HTTP Status of 401
 Unauthorized, and the response body will be empty.

### signout

The `signout` actions is a *DELETE* specifying the `id` of the user to sign out.

If the request is successful the response will have an HTTP status of 204 No
 Content.

If the request is unsuccessful, the response will have a status of 401
 Unauthorized.

### changepw

The `changepw` action expects a PATCH of `passwords` specifying the `old` and
 `new`.

If the request is successful the response will have an HTTP status of 204 No
 Content.

If the request is unsuccessful the reponse will have an HTTP status of 400 Bad
 Request.

---

The `sign-out` and `change-password` requests must include a valid HTTP header
 `Authorization: Token token=<token>` or they will be rejected with a status of
 401 Unauthorized.

## Item actions

All items action requests must include a valid HTTP header `Authorization: Token
 token=<token>


*Summary:*

<table>
<tr>
  <th colspan="3">Request</th>
  <th colspan="2">Response</th>
</tr>
<tr>
  <th>Verb</th>
  <th>URI</th>
  <th>body</th>
  <th>Status</th>
  <th>body</th>
</tr>
<tr>
<td>POST</td>
<td>`/items`</td>
<td><strong>item</strong></td>
<td>200, OK</td>
<td><strong>item w/ id</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>400 Bad Request</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>PATCH</td>
<td>`/items/:id`</td>
<td><strong>item</strong></td>
<td>200 OK</td>
<td><strong>item w/ id</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>422 Unprocessable Entity</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>DELETE</td>
<td>`/items/:id`</td>
<td>empty</td>
<td>204 no content</td>
<td>empty</td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>401 Unauthorized</td>
  <td><em>empty</em></td>
</tr>
<tr>
<td>GET</td>
<td>`/items/</td>
<td><strong>Items for signed in user</strong></td>
<td>200 OK</td>
<td><strong>user w/token</strong></td>
</tr>
<tr>
  <td colspan="3"></td>
  <td>400 bad request</td>
  <td><em>Items w/ ids</em></td>
</tr>
</table>

### index

The `index` action is a *GET* that retrieves all the items associated with a
 user.
The response body will contain JSON containing an array of games, e.g.:

```json
{
  "items": [
    {
      "content": "Item 1",
      "id": 43
    },
    {
      "content": "Item 2",
      "id": 3
    }
  ]
}
```
### create

The `create` action expects a *POST* 

Example JSON

```json
{
    "item": {
      "content": "Item Content"
    }
  }
 ```

If the request is successful, the response will have an HTTP Status of 200
 OK, and the body will contain JSON of the Item and its id.

```json
{
  "item": {
    "content": "Item Content",
    "id": 48
  }
}
```

If the request is unsuccessful, the response will have an HTTP Status of 400 Bad
 Request, and the response body will be JSON describing the errors.

### show

The `show` action is a *GET* specifing the `id` of the game to retrieve.
If the request is successful the status will be 200, OK, and the response body
 will contain JSON for the game requested, e.g.:

```json
{
  "game": {
    "id": 1,
    "cells": ["o","x","o","x","o","x","o","x","o"],
    "over": true,
    "player_x": {
      "id": 1,
      "email": "and@and.com"
    },
    "player_o": {
      "id": 3,
      "email": "dna@dna.com"
    }
  }
}
```

### update

#### join a game as player 'o'

This `update` action expects an empty (e.g `''` or `'{}'` if JSON) *PATCH* to
 join an existing game.

If the request is successful, the response will have an HTTP Status of 200 OK,
 and the body will be JSON containing the game joined, e.g.:

```json
{
  "game": {
    "id": 1,
    "cells": ["","","","","","","","",""],
    "over":false,
    "player_x": {
      "id": 1,
      "email": "and@and.com"
      },
    "player_o": {
      "id": 3,
      "email":
      "dna@dna.com"
    }
  }
}
```

If the request is unsuccessful, the response will have an HTTP Status of 400 Bad
 Request, and the response body will be empty (game cannot be joined, player_o
 already set or user making request is player_x) or JSON describing the errors.

#### update a game's states

This `update` action expects a *PATCH* with changes to to an existing game,
 e.g.:

```html
<form>
  <input name="game[cell][index]" type="text" value="0">
  <input name="game[cell][value]" type="text" value="x">
  <input name="game[over]" type="text" value="false">
</form>
```

```json
{
  "game": {
    "cell": {
      "index": 0,
      "value": "x"
    },
    "over": false
  }
}
```

If the request is successful, the response will have an HTTP Status of 200 OK,
 and the body will be JSON containing the modified game, e.g.:

```json
{
  "game": {
    "id": 1,
    "cells": ["x","","","","","","","",""],
    "over":false,
    "player_x": {
      "id": 1,
      "email": "and@and.com"
      },
    "player_o": {
      "id": 3,
      "email":
      "dna@dna.com"
    }
  }
}
```

If the request is unsuccessful, the response will have an HTTP Status of 400 Bad
 Request, and the response body will be JSON describing the errors.

### watch

The `watch` action is handled differently than all the others.  Because `watch`
 implements a streaming source of data, we'll use a wrapper around the html5
 object EventSource to handle the events sent.

You can find the wrapper [here](public/js/resource-watcher-0.1.0.js).
The wrapper is also available from the deployed app at the path
 `/js/resource-watcher-0.1.0.js`.

The events that `watch` implements let you know when a game has been updated.
By using this interface you can write code that lets a player see another's move
 almost as it happens.
Updates to the game from one player's browser are sent to the other's browser.

You create a watcher object using the resourceWatcher function.
This function takes two parameters, the watch url and a configuration object
 which must contain the Authorization token, and may contain an optional timeout
 in seconds, e.g.:

```js
let gameWatcher = resourceWatcher('<server>/games/:id/watch', {
      Authorization: 'Token token=<token>'[,
      timeout: <timeout>]
});
```

By default, watching a game times-out after 120 seconds.

You should add a handler for `change` and `error` events.
The error events are not the most informative.
The change event may pass to your handler an object with a root key of "timeout"
or "heartbeat".

Otherwise, it will pass an object with a root key of "game".  Each key in this
object will contain an array of length 2.  The first element of such an array
will contain the value for that key before the update.  The last element will
contain the value after the update.  The code example that follows shows
handling the most important case, a change to the game board.

```js
gameWatcher.on('change', function (data) {
  console.log(data);
  if (data.game && data.game.cells) {
    const diff = changes => {
      let before = changes[0];
      let after = changes[1];
      for (let i = 0; i < after.length; i++) {
        if (before[i] !== after[i]) {
          return {
            index: i,
            value: after[i],
          };
        }
      }

      return { index: -1, value: '' };
    };

    let cell = diff(data.game.cells);
    $('#watch-index').val(cell.index);
    $('#watch-value').val(cell.value);
  } else if (data.timeout) { //not an error
    gameWatcher.close();
  }
});

gameWatcher.on('error', function (e) {
  console.error('an error has occurred with the stream', e);
});
```

## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
