# Utilities

Here are some basic Lemon utilities:

- `console(text, color=white)` - This function writes to console. ❗ It works only with development server.
- `redirect(path)` - Redirects user to given path
- `jsonify(data)` - Returns given data as json
- `isUserPodvodnik(user)` - Returns whenever is user podvodnik
    Example:
    ```php
    isUserPodvodnik("Mia") // false
    isUserPodvodnik("CoolFido") // true
    ```
- `dd(data)` - Dumps data and exits whole app
