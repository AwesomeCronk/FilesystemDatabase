# Thread safety
* `dbNode.acquire(wait=False)`
  * Generate access token (unique within the node, maybe semirandom)
  * Write (filemode: a) access token + `\n` to file `_lock`
  * Read back file `_lock` and check if the access token is first in the file.
  * If so
    * return access token
  * If not
    * If `wait`, then pause 5ms and check file `_lock` again.
    * Else, then return None or raise an exception or something
  * If token wasn't written to the file at all, rewrite the token
  * All `dbNode.node()`, `.get()`, and `.set()` must take an access token if locked
* `dbNode.release(token)`
