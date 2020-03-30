## Using a list

Everything you send to `foo@hostname` will be send to all subscribers, but they will see only certain headers and the body of your email. The selection of these headers can be configured for each list individually by the list-admins.

### Getting a list's public key

Each Schleuder-list replies with its public key to any email sent to `foo-sendkey@hostname`. E.g. to receive the key for our contact address write an email to `team-sendkey@schleuder.org`.

### Special keywords

Schleuder knows some special keywords that trigger different behaviour. You can e.g. subscribe someone, or resend an email to a non-subscriber using keywords. See a list of available keywords below.

Keywords require that:

* they start the line and begin with "x-",
* they are written into the beginning of the *first text-part* of the email (usually that's just the normal body of the email),
* possible arguments must be written *on the same line* as the keyword (exceptions are mentioned in the descriptions below),
* the email must be *encrypted and signed* by a list-member's key.
* the email must be formatted as a *plain text* message and not with HTML, RTF or similar formatting.

Keywords can be repeated within one email at will.
Letter case doesn't matter.

There are two types of keywords: those to enhance messages sent over the list ("list-keywords"), and those to request something from Schleuder ("request-keywords").

#### Security

To mitigate replay attacks of emails containing keywords, every email using a keyword **must** contain the special `x-list-name` keyword followed by the list's emailaddress. Example:

x-list-name: someone@example.org
: **You must always provide this keyword once per email.** Without it, no other keyword will be considered but you will receive an error message.

